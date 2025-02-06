
### Regla en powershell para permitir acceso a ssh:
New-NetFirewallRule -Name "PermitirSSH" -DisplayName "Permitir Conexiones SSH" -Direction Inbound -Protocol TCP -LocalPort 22 -Action Allow