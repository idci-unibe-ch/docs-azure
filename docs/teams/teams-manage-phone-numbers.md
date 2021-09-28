# Telefonnummern in Teams verwalten

**Voraussetzungen**  
- Das aktuelle MicrosoftTeams Powershell Modul wurde installiert (>= 2.3.1).  
``` powershell
Install-Module MicrosoftTeams  
Update-Module MicrosoftTeams  
```
- Die Rolle ***Skype for Business Administrator*** wurde via PIM aktiviert.  

!!! warning
    Powershell Core wird nicht unterstützt.  

## Telefonnummer hinzufügen

1. Das MicrosoftTeams Powershell Modul laden und am Tenant anmelden.  
``` powershell
Import-Module MicrosoftTeams
Connect-MicrosoftTeams
```
2. Auf dem gewünschten Account die Telefonoption aktivieren und eine Nummer zuweisen.  
``` powershell
Get-CsOnlineUser -Identity "vorname.nachname@unibe.ch" | Set-CsUser -EnterpriseVoiceEnabled $true -HostedVoiceMail $true -OnPremLineURI "tel:+41316841234"
```
3. Die gewünschte Voice Routing Policy zuweisen[^1].  
``` powershell
Get-CsOnlineUser -Identity "vorname.nachname@unibe.ch" | Grant-CsOnlineVoiceRoutingPolicy -PolicyName "Direct Routing SBC"
```


## Telefonnummer aktualisieren

1. Das MicrosoftTeams Powershell Modul laden und am Tenant anmelden.  
``` powershell
Import-Module MicrosoftTeams
Connect-MicrosoftTeams
```
2. Auf dem gewünschten Account die Telefonnummer aktualisieren.  
``` powershell
Get-CsOnlineUser -Identity "vorname.nachname@unibe.ch" | Set-CsUser -OnPremLineURI "tel:+41316841234"
```

## Telefonnummer löschen

1. Das MicrosoftTeams Powershell Modul laden und am Tenant anmelden.  
``` powershell
Import-Module MicrosoftTeams
Connect-MicrosoftTeams
```
2. Auf dem gewünschten Account die Telefonnummer löschen.  
``` powershell
Get-CsOnlineUser -Identity "vorname.nachname@unibe.ch" | Set-CsUser -OnPremLineURI $null
```

[^1]: [Siehe: PSTN Gateway anbinden](teams-add-pstn-gateway.md)