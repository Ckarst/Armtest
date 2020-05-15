# Deploy Azure Covid Pipeline to your environment
[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FCkarst%2FArmtest%2Fmaster%2Fazuredeploy.json)  
This template deploys an Azure Data Factory (with all linked services, datasets, and pipelines), a Azure Storage account, and an Azure SQL DB. 

After deployment is complete, you will need to: 
- Setup Storage Account Key in ADF
- Verify the pipeline runs by running the pipeline in debug mode
- Activate the trigger deployed to ADF & Publish changes 
- SetUp SQL DB Firewall rules

##Setup Storage Account Key in ADF
At deployment time, the ADF pipeline uses an arbitrary string as a placeholder for your storage account key. To successfully run the ADF pipeline, you will need to correct this.
- First, navigate to the resource group that you deployed the solution to.
- Next, click on the storage account (name "storage<uniqueId>")
  - With the storage account open in the Azure Portal, look to the left navigation bar and click "Access Keys" under Settings.
  - Copy the text for Key value under Key1. 
- Now that you have the Key, go back to your Resource Group and open your Azure Data Factory in the Azure Portal.
- Click Author & Monitor in the center of the page. This will open the Azure Data Factory Development experience.
- Once the ADF editor has rendered, click the "Author"  icon (shape of a pencil writing) on the left nav.
- A left Nav will pop out. At the bottom of the tab, click the Connections button. 
- Now there should be three Linked services on the screen. Select the Covid_Storage Linked service.
- Find the Storage Account Key field and paste the key from the storage account. 
- Verify connectivity by pressing "Test Connection" on the bottom right.
- After connectivity is confirmed, press "Apply"

##Verify the pipeline runs
Now that we have all of the components working, it is time to test the pipeline and initialize the data in the database. 
- Click the Author button on the left nav.
- Expand the Pipelines folder and teh Loading DB subfolder. 
- Click the Load_DB_Driver Pipeline to see the pipeline. 
- Click the Debug button above the pipline components. This will confirm that the pipeline runs as expected and will load the latest data into your SQL DB. 

## Activate the trigger deployed to ADF & Publish
After you verify the pipeline works correctly, it's time to wrap up ADF by Activating the trigger and publishing your changes.
- Next to the Debug button, click the Trigger button and then click New/Edit
- On the right Sidebar, select the Load_DB_Driver_Trigger.
- At the bottom of the sidebar, toggle yes under Activated. This will turn on a trigger which will load fresh data everyday at 8am UTC.
- Click OK on sidebar.
- Finally, Click Publish All on the top left of the editor.

## SetUp SQL DB Firewall rules
Now that we have data in the SQL DB, let's setup the firewall rules so you can access the data.
- Go back to your resource group and select the Server that was created (SQLServer-<>). 
- On the left blade, scroll down to Security and select "Firewalls and virtual networks"
- Select "Add client IP" on the top of the page and then click "Save"
- To verify your changes, click overview and scroll down to select your database.
- Now that you are on the database page, select "Query Editor" in the lect nav. 
- Login to your database with the login and password you specified in the Deployment.

Congratulations! You now have the latest Azure Covid Data.



