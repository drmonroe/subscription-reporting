# Azure subscription-reporting


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

### 4.) Use this exported data to produce reports (for example, using your computer and Excel or with PowerBI or other Azure analytics tooling)

#

# Details

To list all subscriptions:

Get-AzureRmSubscription 

# We want to pipe the subscription IDs into a CSV file for later use when we add the subs to a management group. For example:

Get-AzureRmSubscription | select-object SubscriptionId | export-csv subscriptionID.csv

# This PowerShell cmdlet is used to create a management group

New-AzManagementGroup -GroupName 'YourManagementGroupName' -DisplayName 'YourManagementGroup'

# For example:

New-AzManagementGroup -GroupName "BillingReporting" -DisplayName "BillingReportingGroup"

# 
