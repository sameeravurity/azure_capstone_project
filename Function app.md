### Implement Login functionality using Azure Function: 
#### Clone the artifact which has the login code into visual studio. 
https://akspks880187@dev.azure.com/akspks880187/QuickCart/_git/LoginService.git 
#### Create a local.settings.json file in the login service folder. 
```json
{ 
"IsEncrypted": false, 
"Values": { 
"AzureWebJobsStorage": "UseDevelopmentStorage=true", 
"FUNCTIONS_WORKER_RUNTIME": "dotnet" 
} 
}
```
#### Create an Azure key vault and add the connection string as a secret in the key vault. Fill 
in details in the Basic Tab. In Access Configurations --> Select Vault Access Policy --> 
Select Your Account --> Create Keyvault account. 
* Add secrets in Keyvault. --> Add  Name as DBConnectionString --> Add URL as vaule --> Create 
#### Now to integrate this with Azure pipeline, go to Azure DevOps --> Azure Pipeline --> Library 
Create a New Variable Group 
Give name --> Enable "Link Secrets from Azure KeyVault 
Authenticate the subscription and keyvault here 
If not connecting --> Go to Keyvault --> Go to Access Policy --> Select all the permissions 
Once a variable group is created, reference it in the release pipeline. 
#### Edit release pipeline --> Go to variables --> Go to Variable Groups --> Click on "Link Variable Group" --> Select Scope --> Link 
* Goto Task in the same release pipeline --> Add "Azure App Service Settings" --> This task should be the first task in that stage. 
* Fill in the necessary details --> Scroll down --> In connection string section --> Add keyvault details 
```
[ 
] 
{ 
} 
"name": "kvquickkartsam", 
"value": "$(DBConnectionString)" 
* Keyvault name is Variable Group name. Value to be referenced with $ symbol. --> Save 
and run 
[ 
] 
{ 
} 
"name": "kvquickkartsam", 
"value": "$(DBConnectionString)"
```
#### Change the Loginfunction.cs file to refer to the variable groups declared in the library to refer to the keyvault where the db connection string is stored. 
* Once the changes are made we can build the code by pressing F5. 
* Once the code  is built we can then publish it to the Azure portal by selecting publish 
project and connect to Azure portal and create a new function app by filling required 
fields.  
* Once the function app is created we can check if it is up and running by clicking its url. 
#### Make the required change in the home-page.service.ts file from 
C:\Users\Sameera\source\repos\Quick-Cart(FrontEnd)\src\app\home\HomePage-Services 
in visual studio. 
```C#
import { HttpClient, HttpErrorResponse } from '@angular/common/http'; 
import { Injectable } from '@angular/core'; 
import { catchError, Observable, throwError } from 'rxjs'; 
import { IProduct } from '../Home-Interfaces/IProduct'; 
import { environment } from 'src/environments/environment'; 
@Injectable({ 
providedIn: 'root' 
}) 
export class HomePageService { 
products: IProduct[]=[]; 
constructor(private http: HttpClient)  
{ 
} 
// 
//Getting the Products from backend API 
getProducts():Observable<IProduct[]>{ 
let tempVar = 
this.http.get<IProduct[]>('https://qucktest-test-ceaca0akbjajeaen.centralindia-01.azurewebsites.ne
 t/api/home/getproducts') 
console.log(tempVar) 
return tempVar 
} 
MakePayment(CardNumber1:string,cvv1:string,ex:string,pid:number,cost:number):Observable<
 boolean>{ 
var pay:Payment 
pay={cardNumber:CardNumber1,CVV:cvv1,Expiry:ex,ProdCost:cost,ProdID:pid} 
console.log(pay) 
let tempVar = this.http.post<boolean>('http://localhost:7181/api/PaymentFunction',pay) 
return tempVar 
} 
PostNewSubscriber(emailID:string):Observable<boolean>{ 
console.log(emailID); 
const url = 
`${environment.subscribeFunctionBaseUrl}/SubscribeFunction?code=${environment.subscribeF
 unctionKey}&emailID=${emailID}`; 
let tempVar = this.http.get<boolean>(url); 
console.log(tempVar); 
return tempVar; 
} 
ValidateUser(userEmailID:string, userPassword:string, type:string):Observable<number> 
{ 
var user:User 
user = { emailID: userEmailID, password: userPassword, usertype: type }; 
const url = 
`${environment.functionAppBaseUrl}/LoginFunction?code=${environment.functionKey}`; 
console.log(user) 
return this.http.post<number>(url, user); 
} 
public uploadImage(image: File): Observable<Response>{ 
const formData = new FormData(); 
formData.append('image', image); 
console.log(formData) 
let 
result=this.http.post<Response>('https://localhost:5001/api/admin/upload',formData).pipe(catchE
 rror(this.errorHandler)) 
console.log(result) 
return result 
} 
errorHandler(error: HttpErrorResponse) { 
console.log(error); 
return throwError(error.message|| "server error") 
} 
} 
export class User{ 
emailID:string=''; 
password:string=''; 
usertype:string=''; 
} 
export class Payment{ 
cardNumber:string=''; 
CVV:string=''; 
Expiry:string=''; 
ProdCost:number=0; 
ProdID:number=0; 
}
```
#### Make the necessary changes in environment.ts and environment.prod.ts files from 
C:\Users\Sameera\source\repos\Quick-Cart(FrontEnd)\src\environments folder. 
environment.ts  
```
export const environment = { 
production: false, 
functionAppBaseUrl: 'https://funappsam.azurewebsites.net/api', 
functionKey: '****', 
subscribeFunctionBaseUrl: 'https://quickcart-microservice.azurewebsites.net/api', 
subscribeFunctionKey: 
'*****' 
}; 
```

environment.prod.ts 
```
export const environment = { 
production: true, 
functionAppBaseUrl: 'https://funappsam.azurewebsites.net/api', 
functionKey: '****', 
subscribeFunctionBaseUrl: 'https://quickcart-microservice.azurewebsites.net/api', 
subscribeFunctionKey: 
'*****' 
}; 
```
#### The files are changed such that the home-page.service.ts file refers to the environment 
files for the function key instead of hardcoding the value in the code itself which is 
publicly accessible. 
* Once the changes are made and saved we can test it in the local machine by once again 
building the application using ng build and testing it on the local host. 
* We can then re deploy the application by running the pipeline for frontend and check the 
changes made. 
#### Download and install Azure Functions Runtime 
https://learn.microsoft.com/en-us/azure/azure-functions/functions-run-local?tabs=window
 s%2Cisolated-process%2Cnode-v4%2Cpython-v2%2Chttp-trigger%2Ccontainer-apps&p
 ivots=programming-language-csharp 
* Click on Publish 
* Go to Azure Portal with Function App. Scroll down and see LoginFunction getting 
deployed 
#### Enable CORS policy for Azure Functions 
* Copy static web app URL. 
* Go inside the LoginFunction App 
* Search for CORS 
* Add your static web app link 
* Add * in next text box 
* Save 
* It will take up to 10 min to reflect this functionality. 
* See to it that the pipeline is running. Once the pipeline finishes, then login. 
* You should be able to login now.
