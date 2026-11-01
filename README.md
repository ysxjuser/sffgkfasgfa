Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
Set-Service sshd -StartupType Automatic
Start-Service sshd
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -PropertyType String -Force
New-NetFirewallRule -DisplayName "SSH-Claude" -Direction Inbound -Protocol TCP -LocalPort 22 -RemoteAddress 88.230.212.136 -Action Allow
New-NetFirewallRule -DisplayName "HTTPS-443" -Direction Inbound -Protocol TCP -LocalPort 443 -Action Allow

1. Gerçek boş alanı gör

Get-Volume -DriveLetter C | Format-List Size,SizeRemaining

2. Nefes aldıracak hızlı temizlik

Remove-Item "$env:TEMP\*" -Recurse -Force -ErrorAction SilentlyContinue; Remove-Item "C:\Windows\Temp\*" -Recurse -Force -ErrorAction SilentlyContinue; Clear-RecycleBin -Force -ErrorAction SilentlyContinue; powercfg -h off; "ok"

Stop-Service wuauserv -Force; Remove-Item "C:\Windows\SoftwareDistribution\Download\*" -Recurse -Force -ErrorAction SilentlyContinue; Start-Service wuauserv; "ok"

3. Asıl iş — WinSxS temizliği

Önce ne kadar kazanacağımıza bakalım:
Dism.exe /O

Sonra temizle (5-15 dakika sürer, sabret, kapatma):
Dism.exe /Online /Cleanup-Image /StartComponentCleanup /ResetBase

4. Windows.old var mı diye bak

Get-ChildItem C:\ -Directory -Force | ForEach-Object { $s=(Get-ChildItem $_.FullName -Recurse -File -Force -ErrorAction SilentlyContinue | Measure-Object Length -Sum).Sum; "{0,9:N0} MB  {1}" -f ($s/1MB), $_.Name } | Sort-Object -Descending

Varsa 5-10 GB daha kazanırız.

---

Hepsi bitince boş alanı tekrar ölç ve ekran görüntüsünü at. 3 GB+ olduğunda SSH kurulumunu tekrar deneriz, sonra domain + SSL işini ben bitiririm.

Not: WinSxS'in gerçek kapladığı yer, gösterilen 12.45 GB'den azdır (hardlink'ler iki kez sayılır) — ama DISM yine de ciddi yer açar.
