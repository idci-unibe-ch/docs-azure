# PSTN Gateway anbinden

**Voraussetzungen**  
- Das aktuelle MicrosoftTeams Powershell Modul wurde installiert (>= 2.3.1).  
``` powershell
Install-Module MicrosoftTeams  
Update-Module MicrosoftTeams  
```
- Die Rolle ***Skype for Business Administrator*** wurde via PIM aktiviert.  

!!! warning
    Powershell Core wird nicht unterstützt.  

## Erfassen eines PSTN Gateways

1. Das MicrosoftTeams Powershell Modul laden und am Tenant anmelden.  
``` powershell
Import-Module MicrosoftTeams
Connect-MicrosoftTeams
```
2. Das Gateway erfassen.  
``` powershell
New-CsOnlinePSTNGateway -Fqdn example-sbc.unibe.ch -SipSignallingPort 5061 -MaxConcurrentSessions 100 -Enabled $true -FailoverTimeSeconds 30 -ForwardCallHistory $true -SendSipOptions $true -MediaBypass $true

Set-CsOnlinePstnUsage  -Identity Global -Usage @{Add="sbcexample"}
```

## Erstellen einer Gateway Policy

1. Das MicrosoftTeams Powershell Modul laden und am Tenant anmelden.  
``` powershell
Import-Module MicrosoftTeams
Connect-MicrosoftTeams
```
2. Die Route erfassen.  
``` powershell
New-CsOnlineVoiceRoute -Identity "Direct Routing SBC Example" -NumberPattern ".*" -OnlinePstnGatewayList example-sbc.unibe.ch -Priority 100 -OnlinePstnUsages "sbcexample"
```

3. Die Policy erfassen.  
``` powershell
New-CsOnlineVoiceRoutingPolicy "Direct Routing SBC Example" -OnlinePstnUsages "sbcexample"
```