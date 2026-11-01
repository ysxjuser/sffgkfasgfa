Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
Set-Service sshd -StartupType Automatic
Start-Service sshd
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
New-NetFirewallRule -DisplayName "SSH-Claude" -Direction Inbound -Protocol TCP -LocalPort 22 -RemoteAddress 88.230.212.136 -Action Allow
New-NetFirewallRule -DisplayName "HTTPS-443" -Direction Inbound -Protocol TCP -LocalPort 443 -Action Allow

Get-ChildItem C:\ -File -Force -ErrorAction SilentlyContinue | Sort-Object Length -Descending | Select-Object Name,@{n='MB';e={[math]::Round($_.Length/1MB)}} | Format-Table -AutoSize

vssadmin list shadowstorage

"WinSxS GB: " + [math]::Round((Get-ChildItem C:\Windows\WinSxS -Recurse -File -Force -ErrorAction SilentlyContinue | Measure-Object Length -Sum).Sum/1GB,2)
