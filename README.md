
# Find-MailboxDelegates
## Summary
Migrating to Exchange Online (EXO) using Exchange Hybrid? Make sure to migrate mailboxes with the same manager or delegate (associated delegates) in the same batch otherwise their access to each other's mailbox will be broken. This is because Send-As, Receive-As, or Send on behalf cross premises permissions are not supported with Exchange hybrid environments. You can read all about it here: https://technet.microsoft.com/en-us/library/jj906433(v=exchg.150).aspx

This script was developed to assist customers with their exchange hybrid migrations into Exchange Online. With this script you can export Exchange 2010/2013 on premises permissions, find their associated delegates (ie spider web batching), and produce a report of mailbox batches that can be used to migrate mailboxes without impacting users.

Moved to location: https://github.com/Microsoft/FastTrack/tree/master/scripts/Find-MailboxDelegates

# Contributing
If you want to contribute and/or provide feedback, please reach out to:

Alejandro Lopez - Alejanl@microsoft.com  
