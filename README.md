# find who is logged into remote computer via powershell


```powershell
$hostnames = (import-csv "c:\temp\hostnames.csv" -Header "hostname","username").hostname
clear
$hostnames

#test if they are online, and then add online hosts to $online_hosts
$Global:online_hosts = @()
$Global:offline_hosts = @()

foreach ($hostpc in $hostnames) {
    if (test-connection $hostpc -count 1 -erroraction silentlycontinue) {write-host $hostpc Online! -foregroundcolor green; $online_hosts += $hostpc} else {write-host $hostpc Offline! -foregroundcolor yellow; $offline_hosts += $hostpc}
}

write-host `nOffline Hosts:
$Global:offline_hosts

write-host `nOnline Hosts:
$Global:online_hosts


foreach ($online_host in $Global:online_hosts) {
    Get-WmiObject –ComputerName $online_host –Class Win32_ComputerSystem | Select-Object name,UserName
}

foreach ($offline_host in $Global:offline_hosts) {
}

```

## Alternate method:

```powershell
Get-CimInstance –ComputerName CLIENT1 –ClassName Win32_ComputerSystem | Select-Object UserName
```
