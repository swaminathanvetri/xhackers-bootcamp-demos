# Demo 2.4: Use Shared Access Signature (SAS) with Azure Storage feature

This demo should take about 5 minutes.

## Objectives

In this demo we will begin to secure our backend services. In the last demo, we had the keys to the storage service in the code running on the mobile clients. We’ll now move this to be only on the backend and grant temporary write access to the client application to upload images.

## \
Requirements\

Hyper-V enabled PC. Required for the Visual Studio UWP and Visual Studio Android emulators.

Internet connection is required in order to setup and run the demos.

You will need Visual Studio 2015/ or Community edition with Update 3

To download Visual Studio 2015 Community edition, [https://www.visualstudio.com/vs/mobile-app-development/](https://www.visualstudio.com/vs/mobile-app-development/%20)

Visual Studio Android emulator: <https://www.visualstudio.com/vs/msft-android-emulator/>

If you encounter issues with connecting Visual Studio debugger with the Visual Studio I would recommend following the steps from this blog article: <http://dotnetbyexample.blogspot.ca/2016/02/fix-for-could-not-connect-to-debugger.html>

(Optional) Mac for compiling and run the iOS projects. Mac is also required to use the XCode designers within Visual Studio for PC or Mac.

## Setup

**It is required to setup the backend prior to delivering this demo.**

1.  Login into the Azure Portal

2.  Open the mobile service that was created from demo 2.1. (1). Search with “Easy” to locate. Then click Easy tables option (2).

> <img src="./media/image1.png" width="484" height="243" />

1.  Click on the photo table to open it (1).

> <img src="./media/image2.png" width="360" height="150" />

1.  Click the Edit script option to open the Node.js backend (1).

> <img src="./media/image3.png" width="688" height="174" />

1.  Click on the Open Console option (1).

> <img src="./media/image4.png" width="590" height="212" />

1.  Enter “npm install azure-storage” into the console and press enter to execute the command (1). This will add the Node.js modules for the AzureStorage service to the Easy Table backend.

> <img src="./media/image5.png" width="674" height="260" />

1.  Bring up the pop menu and select New Folder option (1). Enter the api for the folder name. This is the default folder the Node.js service looks for customized endpoints to the service.

> <img src="./media/image6.png" width="362" height="356" />

1.  Select the api folder. Bring up pop menu and select the New File option. Use

> <img src="./media/image7.png" width="308" height="257" />

1.  Use the name azurestoragesasgen.js for the new file (1).

> <img src="./media/image8.png" width="520" height="565" />

1.  Paste the following code into the new javascript file. The online editor will auto save for you.

> function CreateGuid() {  
>
>    function \_p8(s) {  
>
>       var p = (Math.random().toString(16)+"000000000").substr(2,8);  
>
>       return s ? "-" + p.substr(0,4) + "-" + p.substr(4,4) : p ;  
>
>    }  
>
>    return \_p8() + \_p8(true) + \_p8(true) + \_p8();  
>
> }  
>
>   
>
> var api = {
>
> // To create a Shared Access Signature (SAS), use the generateSharedAccessSignature method
>
> get: (request, response, next) =&gt; {
>
> var containerName = 'photos';
>
> var guid = CreateGuid();
>
> var blobName = guid + '.png';
>
> var accountName = 'gpstagimagestorage';
>
> var accountKey = 'y8zzh5OH5l3mAQoP/MVL/3ZSsJT/YomNvO/4zur2LSACCEtyMLCbAw3WEH65ctqjrkk23/jgPh0/5fGSPizbbA==';
>
> var azure = require('azure-storage');
>
> var blobService = azure.createBlobService(accountName,accountKey);
>
> var startDate = new Date();
>
> var expiryDate = new Date(startDate);
>
> expiryDate.setMinutes(startDate.getMinutes() + 100);
>
> startDate.setMinutes(startDate.getMinutes() - 100);
>
> var sharedAccessPolicy = {
>
> AccessPolicy: {
>
> Permissions: 'rwc',
>
> Start: startDate,
>
> Expiry: expiryDate
>
> },
>
> };
>
> var token = blobService.generateSharedAccessSignature(containerName, blobName, sharedAccessPolicy);
>
> var sasUrl = blobService.getUrl(containerName, blobName, token);
>
> response.send(JSON.stringify(sasUrl));
>
> }
>
> };
>
> module.exports = api;

## Demo Steps

1.  Go to the Solutions folder with this content, locate the Demo2.4.zip file, extract it to a new folder under your Documents folder.

2.  Open up the solution file under the Start folder.

3.  Go to the Azure portal. Show the api folder and JavaScript file you added. Explain that this gets an Azure storage access key to give write permission for a new blob file.

4.  Go back to Visual Studio. Now we need to update the Azure Storage service to use Shared Access Signatures. Select the GPSImageTag.Shared.API project (1). In the pop menu select the Add option (2). Now, select the Existing Item option (3).

> <img src="./media/image9.png" width="594" height="296" />

1.  Select the file AzureStorageService.cs from the folder the Documents &gt; Demo2.4 &gt; SRC &gt; Start &gt; Code Files&gt; Services (1). Next, click the Add button.

> <img src="./media/image10.png" width="397" height="78" />

1.  Click on the Yes button to overwrite the existing file.

> <img src="./media/image11.png" width="297" height="111" />

1.  Delete the file Configuration.cs.

2.  Open the AzureStorageServices.cs file in the GPSImageTag.Shared.API. Show the audience that we are no longer reading the Azure Storage connection information from the Configuration.cs file.

> <img src="./media/image12.png" width="689" height="523" />

1.  Next, we need to update the interfaces files within the GPSImageTag.Core. Select the Interfaces folder in the GPSImageTag.Core project (1). In the pop menu select the Add option (2). Now, select the Existing Item option (3).

> <img src="./media/image13.png" width="497" height="288" />

1.  Select the files IAzureService.cs and IAzureStorageService from the folder the Documents &gt; Demo2.4 &gt; SRC &gt; Start &gt; Code Files&gt; Interfaces (1). Next, click the Add button.

> <img src="./media/image14.png" width="405" height="82" />

1.  Enable the Apply to all items (1). Then Click the Yes button to confirm replacing the existing files (2).

> <img src="./media/image15.png" width="299" height="104" />

1.  Next, we need to update the Azure service class file within the GPSImageTag.Core. Select the Services folder in the GPSImageTag.Core project (1). In the pop menu select the Add option (2). Now, select the Existing Item option (3).

> <img src="./media/image16.png" width="508" height="197" />

1.  Select the file AzureService.cs from the folder the Documents &gt; Demo2.4 &gt; SRC &gt; Start &gt; Code Files&gt; Interfaces (1). Next, click the Add button.

> <img src="./media/image17.png" width="428" height="68" />

1.  Click the Yes button to confirm replacing the existing file (1).

> <img src="./media/image18.png" width="332" height="116" />

1.  Press F6 to build the solution.

2.  Press F5 to run the UWP client.

3.  Take a photo. Enter the name and description for the photo (1). Press the Upload Photo button.

> <img src="./media/image19.png" width="578" height="486" />

1.  Log into the Azure portal and navigate to the mobile service easy table created for this demo and show the photo has been uploaded (1).

> <img src="./media/image20.png" width="1065" height="316" />

1.  Summarize what was accomplished in the demo.
