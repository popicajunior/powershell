# Check is PS Remoting has been executed
#    1. Stop and disable the WinRM service.
#    2. Delete the listener that accepts requests on any IP address.
#    3. Disable the firewall exceptions for WS-Management communications.
#    4. Restore the value of the LocalAccountTokenFilterPolicy to 0, which restricts remote access to members of the
#Administrators group on the computer.
Enable-PSRemoting -SkipNetworkProfileCheck -Force

# Check if WInRM services is installed

# Enable WSMan Service\Auth\Basic = True
Get-NetConnectionProfile
Set-NetConnectionProfile -Name Network -NetworkCategory Private

Set-Item -Path 'WSMan:\localhost\Service\Auth\Basic' -Value $true -Force
Set-Item -Path 'WSMan:\localhost\Service\AllowRemoteAccess' -Value $true -Force
Restart-Service WinRM

# Enable WSMan Client to connect to non-domain members VMs
Set-Item -Path 'WSMan:\localhost\Client\AllowUnencrypted' -Value $true
Set-Item -Path 'WSMan:\localhost\Client\TrustedHosts' -Value * -Force