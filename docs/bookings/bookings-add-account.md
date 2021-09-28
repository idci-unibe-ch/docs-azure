# Unpersönlichen Bookings Account anlegen

Bei unpersönlichen Bookings Kalendern empfiehlt es sich einen Admin-Account für die Verwaltung und anschliessend die Staff Accounts einzuruchten.  
Auf diese Weise hat man eine zentrale Verwaltung und die Staff Accounts können nur die ihnen zugeordneten Termine bearbeiten.  

Am Beispiel unseres COVID-19 Testcenter Booking Calendars bedeutet dies, dass ein Admin-Account für die Verwaltung und anschliessend für jedes Testcenter ein Account eingerichtet wurde.  

## Bookings Calendar ##
Connect-ExchangeOnline
# Create Remote Mailbox (on Premises)
$Credentials = Get-Credential
New-RemoteMailbox -Name "Covid19-Testcenter-Admin" -Password $Credentials.Password -UserPrincipalName "covid19-tc-admin@unibe.ch" -Alias "covid19-tc-admin" -PrimarySmtpAddress "covid19-tc-admin@unibe.ch" -OnPremisesOrganizationalUnit "OU=Covid19_Testzentren,OU=CI,OU=ID-CloudSystems,OU=ID-Infrastructure,DC=campus,DC=unibe,DC=ch"
New-RemoteMailbox -Name "Covid19-Testcenter-Hochschulstrasse-4" -Password $Credentials.Password -UserPrincipalName "covid19-tc-h4@unibe.ch" -Alias "covid19-tc-h4" -PrimarySmtpAddress "covid19-tc-h4@unibe.ch" -OnPremisesOrganizationalUnit "OU=Covid19_Testzentren,OU=CI,OU=ID-CloudSystems,OU=ID-Infrastructure,DC=campus,DC=unibe,DC=ch"
New-RemoteMailbox -Name "Covid19-Testcenter-Bühlstrasse-20a" -Password $Credentials.Password -UserPrincipalName "covid19-tc-b20a@unibe.ch" -Alias "covid19-tc-b20a" -PrimarySmtpAddress "covid19-tc-b20a@unibe.ch" -OnPremisesOrganizationalUnit "OU=Covid19_Testzentren,OU=CI,OU=ID-CloudSystems,OU=ID-Infrastructure,DC=campus,DC=unibe,DC=ch"
New-RemoteMailbox -Name "Covid19-Testcenter-Bremgartenstrasse-107" -Password $Credentials.Password -UserPrincipalName "covid19-tc-b107@unibe.ch" -Alias "covid19-tc-b107" -PrimarySmtpAddress "covid19-tc-b107@unibe.ch" -OnPremisesOrganizationalUnit "OU=Covid19_Testzentren,OU=CI,OU=ID-CloudSystems,OU=ID-Infrastructure,DC=campus,DC=unibe,DC=ch"
# Create Remote Mailbox with Exchange Split Permissions (create User in AD first - UnibeApplSyncToCloud = True)
# Booking Admin Mailbox
Enable-RemoteMailbox -Identity "Covid19-Testcenter-Admin" -Alias "covid19-tc-admin" -PrimarySmtpAddress "covid19-tc-admin@unibe.ch" -RemoteRoutingAddress "covid19-tc-admin@unibe365.mail.onmicrosoft.com"
# Booking Staff Mailbox
Enable-RemoteMailbox -Identity "Covid19-Testcenter-Hochschulstrasse-4" -Alias "covid19-tc-h4" -PrimarySmtpAddress "covid19-tc-h4@unibe.ch" -RemoteRoutingAddress "covid19-tc-h4@unibe365.mail.onmicrosoft.com"
Enable-RemoteMailbox -Identity "Covid19-Testcenter-Bühlstrasse-20a" -Alias "covid19-tc-b20a" -PrimarySmtpAddress "covid19-tc-b20a@unibe.ch" -RemoteRoutingAddress "covid19-tc-b20a@unibe365.mail.onmicrosoft.com"
Enable-RemoteMailbox -Identity "Covid19-Testcenter-Bremgartenstrasse-107" -Alias "covid19-tc-b107" -PrimarySmtpAddress "covid19-tc-b107@unibe.ch" -RemoteRoutingAddress "covid19-tc-b107@unibe365.mail.onmicrosoft.com"
# AADSync
Start-ADSyncSyncCycle -PolicyType Delta
# To turn Bookings on or off for your organization (Exchange Online)
Get-OrganizationConfig | fl BookingsEnabled
# BookingsEnabled $true
# OWA Policy for Booking Admin (Exchange Online)
Get-OwaMailboxPolicy | ft name, *book*
Set-CASMailbox -Identity covid19-tc-admin -OwaMailboxPolicy "OwaMailboxPolicy-Default"
Set-CASMailbox -Identity covid19-tc-h4 -OwaMailboxPolicy "OwaMailboxPolicy-Default"
Set-CASMailbox -Identity covid19-tc-b20a -OwaMailboxPolicy "OwaMailboxPolicy-Default"
Set-CASMailbox -Identity covid19-tc-b107 -OwaMailboxPolicy "OwaMailboxPolicy-Default"
Get-CASMailbox -Identity Covid-Testcenter-H4 | fl *policy*
# Booking Staff, cant't create and Manage Booking Ressources
Get-CASMailbox -Identity Covid-Testcenter-H4-Box1 | fl *policy*
# Get Bookings-associated mailboxes in Exchange Online
Get-Mailbox -RecipientTypeDetails SchedulingMailbox -ResultSize:Unlimited | Get-MailboxPermission |Select-Object Identity,User,AccessRights | Where-Object {($_.user -like '*@*')}
# Links
# Microsoft Bookings -> https://docs.microsoft.com/en-us/microsoft-365/bookings/bookings-overview?view=o365-worldwide
Disconnect-ExchangeOnline