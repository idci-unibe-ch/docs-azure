Install-Module AIPService
Import-Module aipservice
Connect-AipService

Policy (Nur lizenzierte user in der Gruppe XY)
Set-AipServiceOnboardingControlPolicy -UseRmsUserLicense $True -SecurityGroupObjectId "f9cdef11-f0de-4fd8-b147-040681a0a8b7" -Scope All

Enable-AipService

Mehr Informationen:
https://docs.microsoft.com/en-us/azure/information-protection/activate-service