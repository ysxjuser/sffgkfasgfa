5. Başlat:

Set-Service sshd -StartupType Automatic; Start-Service sshd; Get-Service sshd

6. Kabuk + firewall:

New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force

New-NetFirewallRule -DisplayName "SSH-Claude" -Direction Inbound -Protocol TCP -LocalPort 22 -RemoteAddress 88.230.212.136 -Action Allow

New-NetFirewallRule -DisplayName "HTTPS-443" -Direction Inbound -Protocol TCP -LocalPort 443 -Action Allow
