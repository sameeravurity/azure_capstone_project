
# Task 1: Local Machine Setup and Testing Instructions

## Installations Required on Local Machine

- **Visual Studio Community Edition 2022** with Azure SDK and .NET Core 3.1
- **Visual Studio Code**
- **Git**
- **Node.js** with **Angular 13.3**
- **Azure Portal Account**

---

## Testing the Frontend and Backend on Local Machine

### Uploading NuGet Package Source

1. Open Visual Studio and navigate to:  
   `Tools -> NuGet Package Manager -> Package Manager Settings -> Package Sources`
2. Add a new package source:  
   - **Name**: `nuget.org`  
   - **Source**: `https://api.nuget.org/v3/index.json`
3. Select only this package source. Deselect any others if listed.

---

## Importing Repos for Frontend and Backend

### Backend

- Clone the backend project using Visual Studio.  
- **Clone URL**:  
  `https://akspks880187@dev.azure.com/akspks880187/QuickCart/_git/QuickCart`
- Go to: `File -> Open -> Folder -> QuickCart (backend)`

### Frontend

- Clone the frontend project using Visual Studio.  
- **Clone URL**:  
  `https://akspks880187@dev.azure.com/akspks880187/QuickCart/_git/Quick-Cart-FrontEnd`
- Go to: `File -> Open -> Folder -> QuickCart (frontend)`

---

## Create a Storage Account

1. Open the [Azure Portal](https://portal.azure.com/).
2. Create a storage account with a unique name.
3. Create a container inside the storage account.
4. Upload images from the frontend repo into the container.
5. Set the container access level to **Anonymous Access**.

---

## Editing the JSON File

- Open the `appsettings.json` file from the backend repo in Visual Studio.
- Add the required **connection string** provided to you.

---

## Testing the Backend on Local Machine

1. Open **Command Prompt**.
2. Navigate to the directory containing the `.csproj` file:  
   ```bash
   cd path\to\QuickCart\QuickCart
   dotnet run
   ```
3. Test the backend by browsing to:  
   [http://localhost:5001/api/home/getproducts](http://localhost:5001/api/home/getproducts)

---

## Testing the Frontend on Local Machine

1. Open **Command Prompt** where Angular is installed.
2. Run the following commands:  
   ```bash
   npm install
   ng serve
   ```
3. Test the frontend by browsing to:  
   [http://localhost:4200/](http://localhost:4200/)
