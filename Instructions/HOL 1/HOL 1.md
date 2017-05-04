# Hands on lab 1: Using Azure mobile services and storage

This HOL should take about 30 minutes.

## Requirements\

Hyper-V enabled PC. Required for the Visual Studio UWP and Visual Studio Android emulators.

Internet connection is required in order to setup and run the demos.

You will need Visual Studio 2015 Professional+ or Community edition with Update 3

To download Visual Studio 2015 Community edition, [https://www.visualstudio.com/vs/mobile-app-development/](https://www.visualstudio.com/vs/mobile-app-development/%20)

Visual Studio Android emulator: <https://www.visualstudio.com/vs/msft-android-emulator/>

If you encounter issues with connecting Visual Studio debugger with the Visual Studio I would recommend following the steps from this blog article: <http://dotnetbyexample.blogspot.ca/2016/02/fix-for-could-not-connect-to-debugger.html>

## Steps

### Testing Setup

1.  Go to the Solutions folder with this content, locate the HOL1.zip file, extract it to a new folder under your Documents folder.

2.  In the Documents\\HOL1\\src\\Start\\GPSImageTag locate the \*.sln. Double click the file to open the solution in Visual Studio

3.  Here we want to check to make sure our development setup is good. First we need to set the GPsImageTag.UWP (1) as the Startup Project (2).

> <img src="./media/image1.png" width="361" height="440" />

1.  Press F5 to run the UWP app.

2.  Here we want to check everything is setup correctly. In the camera page take a photo (1). Within the camera window snap a picture (2).

> <img src="./media/image2.png" width="364" height="429" />

1.  (**Optional Android steps**) **Select** the GPSImageTag.Droid project (1). Click on the Set as Startup Project (2).

> <img src="./media/image3.png" width="357" height="408" />

1.  (**Optional Android steps**) Press F5 to run the Android app.

2.  (**Optional Android steps**) Here we test to make sure everything is setup correctly. Click on the Take Photo button (1). Next click on the camera button (2) and then click the Check button (3).

> <img src="./media/image4.png" width="236" height="456" /> <img src="./media/image5.png" width="237" height="457" />

### Azure Mobile App Services:

1.  For this Hands on Lab we will be leveraging Azure to share our photos across devices. We will be setting up Mobile App Services to store our photo data.

2.  To setup the Azure Account Using a browser navigate to the following url: <https://azure.microsoft.com>

3.  On the Azure portal page, click on the Start free button.

    <img src="./media/image6.png" width="584" height="453" />

4.  Click on the Add button and enter the name XamarinDemos for the new resource group. The name only needs to be unique with the same Azure subscription. First, open the resource group window (1). Next, click the Add button (2).

> <img src="./media/image7.png" width="282" height="255" />

1.  In the create resource group window, enter the name XamarinDemos(3). Next, make sure to set the resource group location to your closet region from the drop list (4). Last step, click on the Create button (5)

> <img src="./media/image8.png" width="247" height="282" />

1.  Next, we need to add in a Mobile Apps service to our resource group. Click on the Add button (1). Enter “Mobile Apps QuickStart” into the text search (2). Click on the Mobile Apps QuickStart (3). Finally click on the Create button (4)

> <img src="./media/image9.png" width="817" height="396" />

1.  At this step we need to name the new Mobile App service. The name has to be globally unique on Azure. For this setup process for example I’ve used the name “*gpsimagetagmobileservice*” (1). Next, we need to setup the App service plan (2).

> <img src="./media/image10.png" width="237" height="433" />

1.  This will display a new blade where we can click on the Create New option (1).

> <img src="./media/image11.png" width="407" height="355" />

1.  In the new blade panel, we are setting up the service plan. You need to give the App Service plan unique names within your Azure account (1). Select \\ Location to a region close to you (2). We will change the pricing tier to the free tier for the hands on labs (3).

> <img src="./media/image12.png" width="254" height="408" />

1.  What we are setting is the free tier. By default, Azure sets S1 Tier as the default. For a learning hands on lab we don’t need that level of service. Make sure to click on the View all option (1). Next, we need to scroll down the list of service tiers until we locate the F1 Free tier. Select the F1 Free tier (2). Finally, click on the Select(3).

> <img src="./media/image13.png" width="405" height="564" /> <img src="./media/image14.png" width="409" height="566" />

1.  Back on the New App App Service Plan blade click on the OK button (1).

> <img src="./media/image15.png" width="230" height="387" />

1.  Back on the Mobile Apps QuickStart blade, click on the Create button (1).

> <img src="./media/image16.png" width="192" height="389" />

1.  Now we need to create the Easy table within our mobile app service. Open the mobile app service we’ve just created. Locate the Easy tables option (1). Next, click on the Add button (2).

> <img src="./media/image17.png" width="506" height="386" />

1.  On the Add a table blade. Enter the name of “photo” for the table (1). Then, press the OK button (2).

> <img src="./media/image18.png" width="252" height="422" />

1.  Open the photo table. We need to update the table schema to have the correct fields for our application. See the highlight fields in the image below for the column names and data type to add.

> <img src="./media/image19.png" width="422" height="374" />

1.  Next, we need to copy the url of our new mobile service. We will need this later on for our mobile app as the endpoint to call for the mobile services api. Open the ‘gpsimagetagmobileservice’. On the App Service blade select (1). Then copy the url (2) into notepad for later referencen.

> <img src="./media/image20.png" width="816" height="267" />

### Azure Storage Account:

1.  To store our picture files in Azure we need to setup an Azure Storage account. We need to add a ‘Storage account’ service to our resource group. Click the Add button (1). Enter the Storage account as the search option (2). Make sure to select the Storage account with the category of ‘Storage’ (3)

> <img src="./media/image21.png" width="819" height="256" />

1.  Make sure to give the storage account a unique name. For this setup process I’ve chosen **gpsimagetagstorage** as the name (1). Also, make sure to select **Locally-redundant storage** (LRS) for the replication setting (2). Click the Create button (3).

    <img src="./media/image22.png" width="237" height="540" />

2.  After the Azure setup process is complete for the storage account, click on Refresh in the XamarinDemos group to update the list of services. (1)

> <img src="./media/image23.png" width="369" height="274" />

1.  Select the storage account

2.  What we need to do here is create a specifically-named container within our Azure Storage account for the picture files. Select the Overview option (1). Next, select the Blobs option (2).

> <img src="./media/image24.png" width="635" height="345" />

1.  In the Blob service blade, click on the Container button (1).

> <img src="./media/image25.png" width="486" height="210" />

1.  In the New container blade, enter the name of “photos” (1). We need to set the security access to the blob container. To make this available for our mobile app we want give public read access. Set the Access type to ‘Blob’. Finally, click on the Create button (3)

> <img src="./media/image26.png" width="354" height="273" />

1.  Next, In the settings section select Access keys (1). Copy both the Storage account name (2) and the key 1 value (3). We will need both of these to complete the data setup for the hands on lab.

> <img src="./media/image27.png" width="553" height="273" />

### Add Services to Solution:

1.  Go back to Visual Studio with the hands on lab 1 opened.

2.  We need to add the code for AzureStorage services into the GPSImageTag.Shared.API. Normally we would have to provide code in each platform project to work with the AzureStorage service. Fortunately, we can use a shared project to contain the common code for the upload feature.

3.  Select the GPSImageTag.Shared.API project (1). Bring up the pop menu and select the Add option (2). Next, click the Existing Item (3).

> <img src="./media/image28.png" width="376" height="239" />

1.  Select the files AzureStorageService.cs and Configuration.cs from the folder the Documents &gt; HOL 1 &gt; SRC &gt; Start &gt; Code Files&gt; Services (1). Next, click the Add button.

> <img src="./media/image29.png" width="519" height="122" />

1.  Open the Configuration.cs file in the GPSImageTag.Shared.API. We need to update the Azure Storage connection string. Copy in the Azure Storage Account name and Key copied from the Azure portal to the placed indicated.

> <img src="./media/image30.png" width="1112" height="183" />

1.  In this step we will be adding the mobile service class file in the GSPImageTag.Core. Select the Services folder in the GPsImageTag.Core project. Now, in the pop up menu select the Add option (2). Next, select the Existing Item option (3).

> <img src="./media/image31.png" width="530" height="327" />

1.  Select the file AzureService.cs in the folder Documents &gt; Demo2.3 &gt; SRC &gt; Start &gt; Code Files&gt; Service (1). Next, click the Add button.

> <img src="./media/image32.png" width="387" height="96" />

1.  Open the AzureService.cs file in the GPSImageTag.Core project. Update the MobileServiceUrl with the Url you copied earlier for the Azure mobile service from the Azure portal.

> <img src="./media/image33.png" width="576" height="341" />

### Update ViewModels:

1.  Open up CameraPageViewModel.cs as we need to add a call to the upload code for Azure mobile service.

2.  In the CameraPageViewModel.cs add in the following line of code for the Azure Mobile Service (1).

> private IAzureService mobileService;
>
> <img src="./media/image34.png" width="384" height="303" />

1.  In the constructor of the CameraPageviewModel.cs we need to initialize the mobileServcie. Enter the following line of code (1).

> mobileService = ServiceManager.GetObject&lt;IAzureService&gt;();
>
> <img src="./media/image35.png" width="564" height="444" />

1.  In the CameraPageviewModel.cs we will need to update the UploadPhoto method to call the mobile service for adding a photo to Azure. Add in the following line of code.

> await mobileService.AddPhoto(photo, name, desc);
>
> <img src="./media/image36.png" width="637" height="354" />

1.  Now that we’ve update the CameraPageviewModel we need to update the UI in CameraPage.xaml. Add in the following Xaml code for the Upload Photo button (1).

&lt;Button x:Name="ButtonUploadPhoto" Command="{Binding UploadPhotoCommand}" Text="Upload Photo"/&gt;

> <img src="./media/image37.png" width="687" height="337" />

### Register Services:

1.  Now, we need to update the service registration within each of the Xamarin.Form head projects.

2.  Open the MainActivity.cs file in the GPSImageTag.Droid project. Update the RegisterService function to include the registration of AzureStorageService and AzureService. Add the following code lines to the RegisterService function.

> ServiceManager.Register&lt;IAzureStorageService&gt;(new AzureStorageService());
>
>         ServiceManager.Register&lt;IAzureService&gt;(new AzureService());
>
>   
>
> <img src="./media/image38.png" width="576" height="541" />

1.  Open the App.xaml.cs file in the GPSImageTag.UWP project. Update the RegisterService function to include the registration of AzureStorageServcie and AzureService. Add the follow code lines to the RegisterService function (1).

>         ServiceManager.Register&lt;IAzureStorageService&gt;(new AzureStorageService());
>
>         ServiceManager.Register&lt;IAzureService&gt;(new AzureService());
>
> <img src="./media/image39.png" width="535" height="495" />

### Test:

1.  Next we need to set the GPSImageTag.UWP (1) as the Startup Project (2).

> <img src="./media/image1.png" width="361" height="440" />

1.  Press F5 to run the UWP app.

2.  In the camera page take a photo (1). Within the camera window snap a picture (2). Enter the photo name and descry (3). Finally click on the Upload Photo button (4). In may take some time for the image to upload.

> <img src="./media/image2.png" width="308" height="363" /> <img src="./media/image40.png" width="313" height="369" />

1.  (**Optional Android steps**) **Select** the GPSImageTag.Droid project (1). Click on the Set as Startup Project (2).

> <img src="./media/image3.png" width="357" height="408" />

1.  (**Optional Android steps**) Press F5 to run the Android app.

2.  (**Optional Android steps**) In the camera page take a photo (1). Within the camera window snap a picture. Enter the photo name and descry (3). Finally click on the Upload Photo button (4).

> <img src="./media/image41.png" width="266" height="512" />

1.  Log back into your Azure account. Open the mobile app service you have created. Enter ‘easy’ into the search field to narrow down the properties list (1). Click on the Easy tables (2). Next, click on the photo (3).

> <img src="./media/image42.png" width="495" height="300" />

1.  Confirm that the photo you’ve uploaded exists in the table. (1)

> <img src="./media/image43.png" width="792" height="309" />

### Add new pages:

1.  Now that we have everything configured to upload our photos from our Xamarin.Form app. Let’s now take a look at how we can download the photos and data down to our app.

2.  In the GPSImageTab.ViewModels project lets add in a new viewmodel for the new page. Select the ViewModels folder. Bring up the pop menu select the Add option (2). Next, select Existing Item option (3).

> <img src="./media/image44.png" width="647" height="276" />

1.  The file we will be adding is the PhotosPageViewModel.cs found in the folder in Documents\\HOL 1\\src\\ Start \\Code Files\\ViewModels (1).

> <img src="./media/image45.png" width="694" height="192" />

1.  Now we need to create a new PhotoPage that will use the new viewmodel. In the GPSImageTag project select the Pages folder (1). Bring up the pop menu click the Add (2). Then select New Item (3).

> <img src="./media/image46.png" width="553" height="326" />

1.  In the Add New Item dialog window, select Cross-Platform (1). Next select Forms Xaml Page template (2). Give the new page the name of PhotosPage.xaml (3). Click the Add button (4).

> <img src="./media/image47.png" width="734" height="399" />

1.  Open the new PhotoPage.xaml in the Xaml view and replace the Xaml code with the code below (1).

> &lt;?xml version="1.0" encoding="utf-8" ?&gt;
>
> &lt;ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
>
>              xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
>
>              xmlns:local="clr-namespace:GPSImageTag"
>
>              x:Class="GPSImageTag.PhotosPage"
>
>              Title="{Binding Title}"&gt;
>
>     &lt;ContentPage.Content&gt;
>
>         &lt;StackLayout Spacing="0" Margin="0,10,0,0" Padding="10"&gt;
>
>             &lt;Button x:Name="btnSyncPhotos" Text="Sync Photos" Command="{Binding GetPhotosCommand}"/&gt;
>
>             &lt;ActivityIndicator IsRunning="{Binding IsBusy}" IsVisible="{Binding IsBusy}"/&gt;
>
>             &lt;ListView x:Name="ListViewPhotos"
>
>               ItemsSource="{Binding Photos}" HasUnevenRows="True"&gt;
>
>                 &lt;ListView.ItemTemplate&gt;
>
>                     &lt;DataTemplate&gt;
>
>                         &lt;ViewCell&gt;
>
>                             &lt;Grid Padding="10"&gt;
>
>                                 &lt;Grid.RowDefinitions&gt;
>
>                                     &lt;RowDefinition Height="Auto"/&gt;
>
>                                     &lt;RowDefinition Height="Auto"/&gt;
>
>                                 &lt;/Grid.RowDefinitions&gt;
>
>                                 &lt;Grid.ColumnDefinitions&gt;
>
>                                     &lt;ColumnDefinition Width="Auto"/&gt;
>
>                                     &lt;ColumnDefinition Width="\*"/&gt;
>
>                                 &lt;/Grid.ColumnDefinitions&gt;
>
>                                 &lt;Image  HeightRequest="75"
>
> HorizontalOptions="CenterAndExpand"
>
> VerticalOptions="CenterAndExpand"
>
> Aspect="AspectFill"
>
>                                     WidthRequest="75"
>
>                                     Grid.RowSpan="2"
>
>                                     Source="{Binding Uri}"/&gt;
>
>                                 &lt;Label Grid.Column="1"
>
>                                    Grid.Row="0"
>
>                                    VerticalOptions="Start"
>
>                                    Text="{Binding Name}"/&gt;
>
>                                 &lt;Label Grid.Column="1"
>
>                                    Grid.Row="1"
>
>                                    Text="{Binding Description}"/&gt;
>
>                             &lt;/Grid&gt;
>
>                         &lt;/ViewCell&gt;
>
>                     &lt;/DataTemplate&gt;
>
>                 &lt;/ListView.ItemTemplate&gt;
>
>             &lt;/ListView&gt;
>
>         &lt;/StackLayout&gt;
>
>     &lt;/ContentPage.Content&gt;
>
> &lt;/ContentPage&gt;
>
> <img src="./media/image48.png" width="761" height="409" />

1.  Next, we need to set the page data context to the PhotosPageViewModel. Open the Photospage.xaml.cs and replace the code with the code below (1).

> using GPSImageTag.ViewModels;
>
> using Xamarin.Forms;
>
> namespace GPSImageTag
>
> {
>
>     public partial class PhotosPage : ContentPage
>
>     {
>
>         PhotosPageViewModel vm;
>
>         public PhotosPage()
>
>         {
>
>             InitializeComponent();
>
>             vm = new PhotosPageViewModel();
>
>             BindingContext = vm;
>
>         }
>
>     }
>
> }
>
> <img src="./media/image49.png" width="599" height="358" />

1.  Now that we have two pages within our app we need to add in additional page navigation to navigate between the pages. In the GPSImageTag project select the Pages folder (1). Bring up the pop menu click the Add (2). Then select New Item (3).

> <img src="./media/image46.png" width="553" height="326" />

1.  In the Add New Item dialog window, select Cross-Platform (1). Next select Forms Xaml Page template (2). Give the new page the name of StartPage.xaml (3). Click the Add button (4).

> <img src="./media/image50.png" width="700" height="431" />

1.  Open the new StartPage.xaml in the Xaml view and replace the Xaml code with the code below (1).

> &lt;?xml version="1.0" encoding="utf-8" ?&gt;
>
> &lt;TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
>
>              xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
>
>              x:Class="GPSImageTag.StartPage"&gt;
>
> &lt;/TabbedPage&gt;
>
> <img src="./media/image51.png" width="652" height="209" />

1.  Now we need to define each of the tab page for each of the pages within our application. Open the StartPage.xaml.cs and add in the code below (1).

> using GPSImageTag.Helpers;
>
> using System;
>
> using Xamarin.Forms;
>
> namespace GPSImageTag
>
> {
>
>     public partial class StartPage : TabbedPage
>
>     {
>
>         public StartPage()
>
>         {
>
>             InitializeComponent();
>
>             Title = "Welcome to GPSImageTag";
>
>             Children.Add(new PhotosPage());
>
>             Children.Add(new CameraPage());
>
>             BarBackgroundColor = Colours.TabBackgroundColor;
>
>         }
>
>     }
>
> }

1.  Now we need to update the starting page for our application. Open the App.xaml.cs file and update the App constructor with the following code (1).

> public App()
>
> {
>
>         InitializeComponent();
>
>         MainPage = new GPSImageTag.StartPage();
>
> }
>
> <img src="./media/image52.png" width="569" height="289" />

1.  Let’s try out the changes to our application.

2.  Next we need to set the GPsImageTag.UWP (1) as the Startup Project (2).

> <img src="./media/image1.png" width="361" height="440" />

### Test:

1.  Press F5 to run the UWP app.

2.  Now we have tabs being displayed (1). Click on the Sync Photos button (2). This will download the photos from Azure and displayed it in a list (3).

> <img src="./media/image53.png" width="378" height="447" />

1.  (**Optional Android steps**) **Select** the GPSImageTag.Droid project (1). Click on the Set as Startup Project (2).

> <img src="./media/image3.png" width="357" height="408" />

1.  (**Optional Android steps**) Press F5 to run the Android app.

2.  (**Optional Android steps**) Now we have tabs being displayed (1). Click on the Sync Photos button (2). This will download the photos from Azure and displayed it in a list (3).

> <img src="./media/image54.png" width="297" height="572" />

1.  Congratulations on completing the hands on lab.
