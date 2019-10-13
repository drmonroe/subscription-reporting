# Azure subscription-reporting: A Work in Progress


### Synopsis

A common situation is the accumulation of many Azure subscriptions over time. 

This isn't a problem if these subs were made following a well-defined doctrine to meet clearly understood business and customer needs.

Often however, the subs are created in a disorganized way, by multiple teams, for different lines of business and departments without a central, organizing principle, leading to confusion.

In these cases, to get a handle on what's going on, you need to gather information about the overall subscription situation. 

The key questions to be answered are: 

### One.) How many subscriptions do you have?

### Two.) What's in those subscriptions?

### Three.) How much does it all cost?

One of the best methods for accomplishing this is by using *Azure Cost Management* as a data collection and reporting tool.



# The technique:

### 1.) Collect a listing of all subscriptions using PowerShell

### 2.) Add all subscriptions to a management group. No policies should be applied to this group since its purpose is to serve as an organizing container for the Cost Management tool

### 3.) Export a master report using either PowerShell or the Azure portal from the Cost Management interface

### 4.) Use this exported data to produce reports (for example, using Excel on your computer or, even better, with PowerBI or another Azure analytics tooling)

#

# Details

### To list all subscriptions:

Get-AzureRmSubscription 

### We want to pipe the subscription IDs into a CSV file for later use when we add the subs to a management group. For example:

Get-AzureRmSubscription | select-object SubscriptionId | export-csv subscriptionID.csv

### Note that this can be run using the Azure Cloud Shell, outputting the CSV file to your cloud drive.
### For example, by changing your directory to your cloud shell drive:

cd $HOME\clouddrive

### your outputted CSV can be written to an Azure Blob for use


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

### The PowerShell error still has be sorted and suggestions for tightening the script logic are welcome.  In the meantime, let's assume you've piped all of your subscriptions into the management group. The next step is to view aggregated subscription data via the Azure Cost Management, Cost Analysis interface (Azure Portal --> Cost Management --> Cost Analysis):

![Cost Analysis Accumulated View](https://mlabshare.blob.core.windows.net/malbshare/AzureCostAnalysisBillingGroupScope.png)


### The default view is *Accumulated Costs* which, although useful for cost control purposes, isn't the view we need for subscription reporting.  For reporting, let's switch to the *Costs by Resources* view:

![Cost Analysis Costs by Resource View](https://mlabshare.blob.core.windows.net/malbshare/AzureChangeView.png)


### The *Costs by Resource* view provides a detailed report of all the artifacts contained within the subscriptions that are part of the management group. You can use this via the Cost Analysis interface or, export as a file for ingestion by your preferred analysis tooling:


![Cost Analysis Costs by Resource View Choose Export](https://mlabshare.blob.core.windows.net/malbshare/AzureCostsbyResource.png)




