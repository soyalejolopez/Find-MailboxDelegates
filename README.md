# Find-MailboxDelegates
## Summary
Migrating to Exchange Online (EXO) using Exchange Hybrid? Make sure to migrate mailboxes with the same manager or delegate (associated delegates) in the same batch otherwise their access to each other's mailbox will be broken. This is because cross premises permissions are not supported with Exchange hybrid environments. You can read all about it here: https://technet.microsoft.com/en-us/library/jj906433(v=exchg.150).aspx

This script was developed to assist customers with their exchange hybrid migrations into Exchange Online. With this script you can export Exchange 2010/2013 on premises permissions, find their associated delegates (ie spider web batching), and produce a report of mailbox batches that can be used to migrate mailboxes without impacting users.

## Applies To
* Exchange 2010
* Exchange 2013

## Pre-requisites
* Powershell v3+

# How it works
## High level steps
1. Collect permissions 
1. Find batches based on the output permissions
1. Create migration schedule (this is built in the format required by the Microsoft FastTrack Mail Migration team)

## Available switches and examples
* PARAMETER InputMailboxesCSV  
Use this parameter to specify a list of users to collect permissions for, rather than all mailboxes.
Make sure that the CSV file provided has a header titled "PrimarySMTPAddress"

* PARAMETER ExcludeServiceAccts  
In cases where you have service accounts with permissions to a large number of mailboxes, e.g. Blackberry service accounts, you can use this to exclude those accounts as part of the batching mechanism. Otherwise you'll end up with huge batches. 
The parameter can be populated as following (use the service accounts primarysmtpaddress):
 
[string[]]$ExcludeServiceAccts = @("user1@contoso.com","user2@contoso.com")  
[string[]]$ExcludeServiceAccts = (get-content "C:\Users\administrator\Desktop\userlist.csv")  

*This will slow down processing. 

* PARAMETER FullAccess  
Collect Full Access permissions. Keep in mind that "Full Access" permissions are now supported in cross premises scenarios. Not including "Full Access" will speed up processing. 

* PARAMETER SendOnBehalfTo    
Collect SendOnBehalfTo permissions

* PARAMETER Calendar  
Collect calendar permissions

* PARAMETER SendAs  
Collect Send As permissions

* PARAMETER EnumerateGroups  
This will enumerate groups that have permissions to mailboxes and include in the batching logic.

*This will slow down processing.*

  * PARAMETER ExcludeGroups  
Use this to exclude groups that you don't want to enumerate. This can be used to speed up processing in environments where groups are heavily used. Use group name.   

[string[]]$ExcludeGroups = @("group1@contoso.com","group2@contoso.com")  
[string[]]$ExcludeGroups = (get-content "C:\Users\administrator\Desktop\groupList.csv")  

* PARAMETER ExchServerFQDN  
Connect to a specific Exchange Server

* PARAMETER Resume  
Use this to resume the script in case of a failure while running the script on a large number of users. This way you don't have to start all over.
The way this works is that it will look for the progress xml file where it keeps track of mailboxes that are pending processing.
Make sure not to use in conjunction with the InputMailboxesCSV switch.

* PARAMETER BatchUsers  
Use this if you want to skip collecting permissions and only run Step 2 and Step 3. 
Make sure you have the permissions output file in the same directory (Find-MailboxDelegates-Permissions.csv).

* EXAMPLE  
#Export only SendOnBehalfTo and Send As permissions and Enumerate Groups for all mailboxes.    
.\Find-MailboxDelegates.ps1 -SendOnBehalfTo -SendAs -EnumerateGroups

* EXAMPLE  
#Export only Full Access and Send As permissions and Enumerate Groups for the provided user list. Make sure to use "PrimarySMTPAddress" as header.   
.\Find-MailboxDelegates.ps1 -InputMailboxesCSV "C:\Users\administrator\Desktop\userlist.csv" -FullAccess -SendAs -EnumerateGroups

* EXAMPLE  
#Resume the script after a script interruption and failure to pick up on where it left off. Make sure to include the same switches as before EXCEPT the InputMailboxesCSV otherwise it'll yell at you  
.\Find-MailboxDelegates.ps1 -FullAccess -SendAs -EnumerateGroups -Resume

* EXAMPLE  
#Export all permissions and enumerate groups for all mailboxes  
.\Find-MailboxDelegates.ps1 -FullAccess -SendOnBehalfTo -SendAs -Calendar -EnumerateGroups 

* EXAMPLE  
#Export all permissions but don't enumerate groups for all mailboxes  
.\Find-MailboxDelegates.ps1 -FullAccess -SendOnBehalfTo -SendAs -Calendar

* EXAMPLE
#Export all permissions and exclude service accounts for all mailboxes  
.\Find-MailboxDelegates.ps1 -FullAccess -SendOnBehalfTo -SendAs -Calendar -ExcludeServiceAccts "user1@contoso.com","user2@contoso.com" 

* EXAMPLE  
#Export all permissions and exclude service accounts for all mailboxes  
.\Find-MailboxDelegates.ps1 -FullAccess -SendOnBehalfTo -SendAs -Calendar -ExcludeServiceAccts "user1@contoso.com","user2@contoso.com" -ExcludeGroups "group1@contoso.com","group2@contoso.com" 

* EXAMPLE  
#Skip collect permissions (assumes you already have a permissions output file) and only run Step 2 and 3 to batch users  
.\Find-MailboxDelegates.ps1 -BatchUsers

# Contributing
If you want to contribute and/or provide feedback, please reach out to:

Alejandro Lopez - Alejanl@microsoft.com
Sam Portelli - Sam.Portelli@microsoft.com
