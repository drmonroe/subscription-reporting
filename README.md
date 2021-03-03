# Azure subscription-reporting: A Work in Progress


### Synopsis

A common situation is the accumulation of many Azure subscriptions over time. 

This isn't necessarily a problem if these subs were made following a well-defined doctrine (for example, the Azure Cloud Adoption Framework - https://bit.ly/2OJNVzU) to meet clearly understood business and customer needs and proper tracking and tagging methods were consistently used.

Often however, the subs are created in a disorganized way, by multiple teams, for different lines of business and departments without a central, organizing principle, leading to confusion.

In these cases, to get a handle on what's going on, you need to gather information about the overall subscription situation. 

The key questions to be answered are: 

### One.) How many subscriptions do you have?

### Two.) What's in those subscriptions?

### Three.) How much does it all cost?

One of the best methods for answering these questions is by using *Azure Cost Management* as a data collection and reporting tool.



# The technique:

### 1.) Collect a listing of all subscriptions using PowerShell

### 2.) Add all subscriptions to a management group. No policies should be applied to this group since its purpose is to serve as an organizing container for the Cost Management tool's data collection

### 3.) Export a master report using either PowerShell or the Azure portal from the Cost Management interface

### 4.) Use this exported data to produce reports (for example, using Excel on your computer or, even better, with PowerBI or another Azure or non-Azure analytics tooling)

#

# Details

### To list all subscriptions:

Get-AzureRmSubscription 

### We want to pipe the subscription IDs into a CSV or TXT file for later use when we add the subs to a management group. For example:

CSV File: Get-AzureRmSubscription | select-object SubscriptionId | export-csv subscriptionID.csv

TXT File: Get-AzureRmSubscription | select-object SubscriptionId | Out-File subscriptionID-text.txt

### Note that this can be run using the Azure Cloud Shell, outputting the CSV or TXT file to your cloud drive.
### For example, by changing your directory to your cloud shell drive:

cd $HOME\clouddrive

![Azure Cloud Shell](https://mlabstoragewest.blob.core.windows.net/mlabstorcontainerwest/cloudshell.png)

If you're unfamiliar with Azure Cloud Shell, check out this overview:

https://docs.microsoft.com/en-us/azure/cloud-shell/overview


### Using Cloud Shell, your outputted CSV or TXT can be directly written to an Azure File Share (hosting your clouddrive) for use.

You can learn more about Azure File Shares here: 

https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-create-file-share


### This PowerShell cmdlet is used to create a management group

New-AzManagementGroup -GroupName 'YourManagementGroupName' -DisplayName 'YourManagementGroup'

### For example:

New-AzManagementGroup -GroupName "BillingReporting" -DisplayName "BillingReportingGroup"

### Ideally, you should be able to use the Subscription IDs you gathered in the previous step and pipe those values into the cmdlet which adds subscriptions to a Management Group.  For example, here's the cmdlet to add a single sub to a management group:

New-AzManagementGroupSubscription -GroupName 'YourManagementGroupName' -SubscriptionId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

### Obviously, if you have hundreds of subscriptions to consider, you don't want to manually run the cmdlet against each, one by one.  We can try to apply the cmdlet against a CSV listing of sub IDs.  Here's one attempt:

GC subscriptionID.csv | % {New-AzManagementGroupSubscription -GroupName BillingReporting -SubscriptionId $_}

### This loop doesn't work, producing the following error:

New-AzManagementGroupSubscription : Cannot bind parameter 'SubscriptionId'. Cannot convert value ""123456-1a23-56ab-99ed-vb2345567"" to type "System.Guid". Error: "Guid should contain 32 digits with 4 dashes (xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)."

# Using the Azure Cost Management Analysis Interface

### The PowerShell error still has to be sorted and suggestions for tightening the script logic are welcome (I'm continuing to try ideas).  In the meantime, let's assume you've piped all of your subscriptions into the management group. The next step is to view aggregated subscription data via the Azure Cost Management, Cost Analysis interface (Azure Portal main page --> Cost Management --> Cost Analysis). Note the *scope* which is the *General Resources* management group:

![Cost Analysis Accumulated View](https://acmpstor.blob.core.windows.net/acmpblob1/Azure-Cost-Management-Accumulated-View.png)


### The default view is *Accumulated Costs* which, although useful for cost control purposes, isn't the view we need for subscription reporting.  For reporting, let's switch to the *Costs by Resources* view:

![Cost Analysis Costs by Resource View](https://acmpstor.blob.core.windows.net/acmpblob1/Azure-Cost-by-Resources-View.png)


### With all of the subscription data accessible via a management group and cost analysis, you can export a detailed listing for analysis:

![Cost Analysis Costs by Resource View Choose Export](https://acmpstor.blob.core.windows.net/acmpblob1/Azure-Cost-Management-Export.png)

### Here's an example of an exported Excel spreadsheet:

![Cost Analysis Costs by Resource View Choose Export](https://acmpstor.blob.core.windows.net/acmpblob1/Azure-Cost-Export.png)

### The results can be parsed using any of the included columns which are:

* ResourceId	
* ResourceType	
* ResourceLocation	
* ResourceGroupName	
* SubscriptionName	
* EnrollmentAccountName	
* DepartmentName	
* Tags	
* PreTaxCost	
* Currency

### Depending on the size of the output (if, for example, you have hundreds of subscriptions and maybe thousands of deployed artifacts that make working with a spreadsheet difficult) you may want to import the data to PowerBI (or your preferred data analytics tool) for further insight and report creation (screenshot from PowerBI data selection):

![PowerBI Input](https://acmpstor.blob.core.windows.net/acmpblob1/PowerBI-Excerpt.png)

PowerBI: https://app.powerbi.com


# Azure Cost Management for Busy People

### If you're interested in learning more about Azure Cost Management and what it can do for your organization, check out my Kindle book, "Azure Cost Management for Busy People" -

https://www.azurefinops.com/

And follow me on Twitter for Azure Cost analysis -

https://twitter.com/AFinops


Thanks!


