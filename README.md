1. Konsolda Ctrl + C bas, iptal et.

2. İndir (tek blok):

[Net.ServicePointManager]::SecurityProtocol='Tls12'; $r=Invoke-RestMethod "https://api.github.com/repos/PowerShell/Win32-OpenSSH/releases/latest" -UseBasicParsing; $u=($r.assets | Where-Object { $_.name -like "OpenSSH-Win64*.zip" } | Select-Object -First 1).browser_download_url; Write-Host $u; Invoke-WebRequest $u -OutFile "$env:TEMP\OpenSSH.zip" -UseBasicParsing; "INDIRILDI"

3. Aç:

Expand-Archive "$env:TEMP\OpenSSH.zip" -DestinationPath "C:\Program Files" -Force; Get-ChildItem "C:\Program Files\OpenSSH*" | Select-Object Name



& "C:\Program Files\OpenSSH-Win64\install-sshd.ps1"
ndowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force

New-NetFirewallRule -DisplayName "SSH-Claude" -Direction Inbound -Protocol TCP -LocalPort 22 -RemoteAddress 88.230.212.136 -Action Allow

New-NetFirewallRule -DisplayName "HTTPS-443" -Direction Inbound -Protocol TCP -LocalPort 443 -Action Allow
---

Bu da olmazsa Plan B var

※ recap: unojoin.com'u VDS'ine bağlıyoruz; domain tamam, site http://unojoin.com'da çalışıyor, sadece SSL kaldı. Sunucuda disk doluydu, 4.4 GB açıldı. Sıradaum komutlarını tek tek çalıştırıp "Running"
  yazınca bana haber vermen.

> yarım saat olcdu hala running ooooooooo da kaldı
UserPromptSubmit hook error
Failed with non-blocking status code: No

2. İndir (tek blok):

[Net.ServicePointManager]::SecurityProtocol='Tls12'; $r=Invoke-RestMethod "https://api.github.com/repos/PowerShell/Win32-OpenSSH/releases/latest" -UseBasicParsing; $u=($r.assets | Where-Object { $_.name -like "OpenSSH-Win64*.zip" } | Select-Object -First 1).browser_download_url; Write-Host $u; Invoke-WebRequest $u -OutFile "$env:TEMP\OpenSSH.zip" -UseBasicParsing; "INDIRILDI"

3. Aç:

Expand-Archive "$env:TEMP\OpenSSH.zip" -DestinationPath "C:\Program Files" -Force; Get-ChildItem "C:\Program Files\OpenSSH*" | Select-Object Name

4. Kur:

Set-Service sshd -StartupType Automatic; Start-Service sshd; Get-Service sshd

6. Kabuk + firewall:

New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force

New-NetFirewallRule -DisplayName "SSH-Claudel TCP -LocalPort 22 -RemoteAddress88.230.212.136 -Action Allow

New-NetFirewallRule -DisplayName "HTTPS-443" -Direction Inbound -Protocol TCP -LocalPort 443 -Action Allow
