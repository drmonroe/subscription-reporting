# Azure subscription-reporting

### Synopsis

A common situation is the accumulation of many Azure subscriptions over time. 

If these subs were made to meet business needs, and according to well-defined deployment doctrine, this isn't a problem. 

Often however, the subs are created in a disorganized way, by multiple teams, for different lines of business and departments without a central, organizing principle, leading to confusion.

In these cases, you need to gather information about the overall subscription situation. 

The key questions to be answered are: 

# One.) How many subs 

# Two.) What's in them

# Three.) How much does it cost?

One of the best methods for accomplishing this is by using *Azure Cost Management* as a data collection and reporting tool.



The technique:

# 1.) Collect a listing of all subscriptions using PowerShell

# 2.) Add all subscriptions to a management group (no policies - the management group is used for billing data collection purposes)

# 3.) Export a master report using either PowerShell or the Azure portal from the Cost Management interface

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
