````markdown
# QuickKart E-commerce Migration - Local Setup and Testing

## Task 1: Local Machine Installations and Setup

This section outlines the necessary software installations and configurations on your local machine to set up and test the QuickKart frontend and backend applications.

### 1. Installations on Local Machine:

* **Visual Studio Community Edition 2022:**
    * Ensure you install with the **Azure SDK** and **.NET Core 3.1** components.
* **Visual Studio Code**
* **Git**
* **Node.js:**
    * Install **Angular 13.3** globally (or at the project level, as needed). You can typically install Angular CLI using `npm install -g @angular/cli@13.3`.
* **Azure Portal Account**

### 2. Testing the Frontend and Backend on Local Machine:

#### A. Uploading Package Source (NuGet):

1.  Open **Visual Studio 2022**.
2.  Navigate to `Tools` -> `NuGet Package Manager` -> `Package Manager Settings`.
3.  In the left-hand navigation, select `Package Sources`.
4.  Click the `+` icon to add a new package source.
    * **Name:** `nuget.org`
    * **Source:** `https://api.nuget.org/v3/index.json`
5.  **Important:** Ensure only `nuget.org` is selected. If you have any other package sources, deselect them.

#### B. Importing the Repos for Frontend and Backend:

1.  **Clone the Backend Project:**
    * **Backend Clone URL:** `https://akspks880187@dev.azure.com/akspks880187/QuickCart/_git/QuickCart`
    * In **Visual Studio 2022**, go to `File` -> `Open` -> `Folder`.
    * Navigate to the directory where you cloned the `QuickCart` (backend) repository.
2.  **Clone the Frontend Project:**
    * **Frontend Clone URL:** `https://akspks880187@dev.azure.com/akspks880187/QuickCart/_git/Quick-Cart-FrontEnd`
    * In **Visual Studio Code** (recommended for frontend development), open a new folder.
    * Navigate to the directory where you cloned the `Quick-Cart-FrontEnd` repository.

#### C. Create a Storage Account (Azure):

1.  Open the **Azure Portal**.
2.  Create a new **Storage Account** with a globally unique name.
3.  Inside the newly created storage account, create a **container**.
4.  Upload images from your cloned **Frontend repository** into this container.
5.  Set the **anonymous access** level for the container to "Blob" or "Container" to allow public access to the images.

#### D. Editing the `appsettings.json` file (Backend):

1.  Open the `appsettings.json` file located in your cloned **backend repository** (e.g., `QuickCart/QuickCart.csproj` folder) in **Visual Studio 2022**.
2.  Edit the file by adding the provided connection string (you will need to obtain this connection string from your Azure SQL Database once it's set up, or a placeholder if provided in problem statement).

    *Example Placeholder (replace with actual connection string):*
    ```json
    {
      "Logging": {
        "LogLevel": {
          "Default": "Information",
          "Microsoft.AspNetCore": "Warning"
        }
      },
      "AllowedHosts": "*",
      "ConnectionStrings": {
        "DefaultConnection": "Server=tcp:[your_sql_server_name].database.windows.net,1433;Initial Catalog=[your_database_name];Persist Security Info=False;User ID=[your_user_id];Password=[your_password];MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"
      }
    }
    ```

#### E. Testing the Backend in Local Machine:

1.  Open **Command Prompt** or **PowerShell**.
2.  Navigate to the directory where your backend `.csproj` file exists (e.g., `cd path\to\QuickCart\QuickCart`).
3.  Run the command: `dotnet run`
4.  Check the backend by Browse to: `http://localhost:5001/api/home/getproducts` (ensure you replace `/openbrowser` if it's not part of the actual endpoint).

#### F. Testing the Frontend in Local Machine:

1.  Open **Command Prompt** or **PowerShell**.
2.  Navigate to the directory of your frontend project where `angular` has already been installed (e.g., `cd path\to\Quick-Cart-FrontEnd`).
3.  Run the command: `npm install` (to install project dependencies).
4.  Run the command: `ng serve`
5.  Check the application by Browse to: `http://localhost:4200/`
````
