# Self-hosted-Portal
VERSION : 1.1
RELEASED : June 2022
Introduction ....................................................................3
Deployment process...............................................................3
Prerequisites....................................................................3
System requirements..............................................................4
Developer Workstation............................................................4
Hardware Recommendations.........................................................5
Register Dynamics 365 SDK cmdlets................................................6
Run Portals import script........................................................7
Publish Web Application to Azure ................................................12
Introduction
We are introducing an open-source version of the portal to allow you to customize the portal as per 
your requirement. 
Deployment process
If you deploy Dynamics 365, you can:
• Customize your portal with asp.net code.
• Deploy portals in your own Azure subscription.
• Install portals using Dynamics 365 on-premise.
• Minimize support obligations of portals.
• Move back to an out-of-box non-customized version of portals, and then upgrade to the SaaS 
version.
Dynamics 365 can be deployed from PowerShell using the OAuth method. The deployment process 
consists of the following steps: 
1. Install portal package into Dynamics 365.
2. Publish web application.
Prerequisites
• Dynamics 365 SDK.
• Visual Studio 2019 (tested).
• Azure SDK for .NET 2.9 or later.
• Dynamics 365 (online).
System requirements
The following requirements are the minimum needed to install Portals:
Developer Workstation
• Visual Studio
• Microsoft .NET Framework version 4.6 or later.
• Dynamics 365 Software Development Kit. 
• Windows Features:
Hardware Recommendations
Please note that the hardware recommendations given here are based on the needs of a single website 
with basic resource needs. Given that we do not know the specific load or traffic patterns of your 
application, please use this as a baseline and scale up as you see fit. If you have thousands of users, 
multiple websites and/or hundreds of pages, those will have a heavy performance impact on your server 
and you will want to increase the resources allocated to the server. We do strongly recommend the use 
of virtual machines as their resources can easily be scaled up if the application needs additional 
resources.
• We recommend the use of virtual machines hosted on any current-gen hardware (Dell, Acer, 
Lenovo, HP, etc.) or cloud services provider (Azure, Amazon, etc.).
• We recommend - as a minimum - two (2) CPU cores.
• We recommend - as a minimum - four (4) gigabytes of RAM.
Import portal package into Dynamics 365
A portal package consists of the Dynamics 365 solution schema and data. The name of the files 
containing portal source are:
• MicrosoftDynamics365PortalsSource.exe: Use this file to deploy Portals to IIS or Azure.
• MicrosoftDynamics365PortalsSolutions.exe: Use this file to deploy Portals to Dynamics 365.
You must also download Dynamics 365 SDK to get the Package Deployer tool to import portal packages
into the Dynamics 365 environment.
Register Dynamics 365 SDK cmdlets
Note: Registering the Dynamics 365 SDK cmdlets in one-time activity that must be performed on a 
computer.
1. Download the Dynamics 365 SDK package from Microsoft Download Center.
2. Run the executable file (.msi) to extract the content of the package. Let us assume you extracted 
the package to C:\Dynamics365 folder on your computer. The Package Deployer tool and the 
other required files become available at the following location:
C:\Dynamics365\SDK\Tools\PackageDeployer
3. Follow the below steps to register the SDK’s PowerShell cmdlets:
3.1. Start Windows PowerShell with elevated privileges (run as administrator).
3.2.Navigate to the PowerShell folder under the PackageDeployer folder.
cd C:\Dynamics365\SDK\Tools\PackageDeployer\PowerShell
3.3.Run the RegisterXRMTooling.ps1 script to register the Package Deployer Windows 
PowerShell assembly (.dll), and install the Windows PowerShell snap-in for the Package 
Deployer tool.
.\RegisterXRMTooling.ps1 
Note: If you are unable to run the script, check your script execution policy by running the 
following command: get-executionpolicy If the script execution policy is set to restricted, elevate 
it by running the following command: set-executionpolicy unrestricted 
Run Portals import script
The portals solution distribution includes the following PowerShell scripts in the 
Portals\PackageDeployerPackages folder:
• Import.ps1: Prompts you to select a package to import, specify Dynamics 365 environment 
details, and then runs the ImportPackage.ps1 script to import the portal package into Dynamics 
365.
• ImportPackage.ps1: Takes the selected package and Dynamics 365 parameters, and then 
executes the SDK’s Import-CrmPackage PowerShell cmdlet to import the package into Dynamics 
365
Caution: Please be aware that this package contains a specific version of the portal solution files that 
were current at the time of release. The portal source code has only been tested to function on this 
specific version of the solution files. Please ensure that you do not upgrade your portal solutions 
through the Dynamics 365 Administration Center or have them upgraded by installing a new portal via 
the Dynamics 365 Administration Center as that will upgrade your solutions to a version that has not 
been tested with this version of your portal host. There is no guarantee that future versions of the portal 
solutions will be backwards compatible with older portal source code. There is no mechanism to roll 
back solution versions outside of completely uninstalling them and reinstalling them which will result in 
data deletion. No support will be given to customers to restore operation of their portals deployed from 
this package resulting from an upgrade of the portal solutions to newer versions. The only remedy 
would be for customers to make appropriate modifications to the portal source code to be compatible 
with the new solution files and to redeploy the portal. There is no plan for us to provide an update to 
the portal source code or the technical information required to be compatible with future versions.
1. Download the MicrosoftDynamics365 file and extract the content. Let us assume you extracted 
the content to C:\MicrosoftDynamics365
2. Navigate to the PackageDeployerPackages folder under the Portals folder. 
cd C:\MicrosoftDynamics365\Portals\PackageDeployerPackages
3. Run the following commands: 
Install-Module -Name Microsoft.Xrm.Tooling.CrmConnector.PowerShell
Install-Module -Name Microsoft.Xrm.Tooling.PackageDeployment.PowerShell
If you installed the module in the past, you can update it with the following command:
Update-Module -Name Microsoft.Xrm.Tooling.CrmConnector.PowerShell
Update-Module -Name Microsoft.Xrm.Tooling.PackageDeployment.PowerShell
4. You are now ready to use the Microsoft.Xrm.Tooling.CrmConnector.PowerShell and
Microsoft.Xrm.Tooling.PackageDeployment.PowerShell cmdlet. To list the functions that you 
registered, run the following command in the PowerShell window:
Get-Help “Crm”
5. Run the Import.ps1 script to start the package deployment .\Import.ps1
The CRM connection is displayed (press Enter to continue)
6. DeploymentRegion is displayed choose where your Dynamics 365 is hosted CRM URL regions
7. Specify values for the following and press Enter after each value: 
7.1. Specify Organization Name: Unique organization name of Dynamics 365.
7.1.1. Go to your Dynamics 365 (Example: 
https://org2d4532b8crm5.dynamics.com)
7.1.2. Settings -> Customization.
7.1.3. Developer Resources. 
7.1.4. Unique organization name.
7.2. Specify Language Locale ID (LCID): Locale ID to be used or leave it blank to use 1033 for 
English by default.
7.3. Select a package to import: Press the appropriate number to import the package.
8. Enter your Dynamics 365 administrator credentials.
8.1.image
8.2.On successful authentication, the package is imported into Dynamics 365. (it takes 
around 25 minutes)
9. Then an error message is displayed
10. Download The D365 SDK V9.7
11. Unzip the downloaded file then open it, go to D365 SDK V9 -> ConfigMigration
12. Run DataMigrationUtility.exe
13.image
Choose Import data, press Continue. 
14.image
Select Office 365, Display list of available organizations, Show Advanced. Add your CRM Online 
Region, User Name and Password. Press login.
15. Choose the Organization Name
16. When the connection is completed, you can choose the data file to import it to CRM.
16.1. Go to 
C:\MicrosoftDynamics365\Portals\PackageDeployerPackages\CustomerPortal\Adxstudi
o.CustomerPortal (use the file that you’ve selected before on PowerShell on Step 7.3)
16.2. This zip is used for English language data_1033.zip.
17.
18. The Data import to CRM is completed. 
Publish Web Application to Azure
follow the steps below to publish the web application to Azure: 
1. Download MicrosoftDynamics365PortalsSource.exe and run the file to extract the content. Let’s 
assume you extracted the content to C:\MicrosoftDynamics365. 
2. Open the C:\ MicrosoftDynamics365\Portals\Solutions\Portals\Portals.sln file in Visual Studio
3. Go to Solution Explorer-> Web.config
4.image
5. Go to line 66 and 127. To targetFramwork
6. change it to 4.6 targetFramework="4.6".
6.1.image
6.2.image
7. Right-click the MasterPortal web application project in the Solution Explorer and select Publish
8. images
9. The Publish Web window is displayed. Select Azure and Press Next.
10. Select Microsoft Azure App Service as the publish target, press Next.
11. Provide the appropriate account and subscription details for Azure and click OK.
12. The Visual Studio output window will log the status of the publishing task. When the publication 
is completed, your Azure website is launched in a new browser window. The Portal 
Configuration page is displayed.
13.image
14.image
15. Select the desired website and click Apply.
16. Congrats! the portal is published and connected to Dynamics 365.
Note: the above is the updated version V1.1 of the Self-Hosted Portal Guide V1.0 
