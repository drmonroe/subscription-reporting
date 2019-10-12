# Azure subscription-reporting

This repo will be used to collect information about Azure subscription reporting (using the billing API)
The technique will be:

1.) Collect a listing of all subscriptions using PowerShell

2.) Add all subscriptions to a management group (no policies - the management group is used for billing data collection purposes)

3.) Export a master report using either PowerShell or the Azure portal from the Cost Management interface

#

# Details
# To list all subscriptions:

Get-AzureRmSubscription 

# We want to pipe the subscription IDs into a CSV file for later use when we pipe all the subs into a management group for example:

Get-AzureRmSubscription | select-object SubscriptionId | export-csv subscriptionID.csv

# This PowerShell cmdlet is used to create a management group

New-AzManagementGroup -GroupName 'YourManagementGroupName' -DisplayName 'YourManagementGroup'

# For example:

New-AzManagementGroup -GroupName "BillingReporting" -DisplayName "BillingReportingGroup"

# 
