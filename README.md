# pswrkcsvdatapowershell
## 3.Exploring Information to CSV
### 2 The Basics of
```
Get-Process | Export-Csv C:\a.csv
```

```
Get-Process | Get-Member
```

#### 04:25
```
Get-Process | Select Name,Path,Company | Export-Csv | c:\a.csv
```
delete type information
```
Get-Process | Select Name,Path,Company | Export-Csv | c:\a.csv -NoTypeInformation
```

```
Get-Process | Select Name,Path,Company | Export-Csv | c:\a.csv -NoTypeInformation -Delimiter ';'
```

#### 09:58
Data -> Text to Columns


### 3 Exporting Hard Drive Information
```
Get-WmiObject win32_logicaldisk -ComputerName LocalHost
```
more complex
```
Get-WmiObject Win32_logicaldisk -ComputerName Localhost `
Select `
@{Name="Letter";Expression={($_.DeviceID)}},`
@{Name="Size(GB)";Expression={[decimal]("{0:N2}" -f($_.size/1gb))}},`
|Export-CSV C:\a.csv -NoTypeInformation
```

### 4 Exporting Active Directory Users
```
get-aduser -filter * -properties department,name | where {$_.department -eq "math"} | Select Name, Title, @{N='Manager';Expression={(Get-ADUser $_.Manager).Name}} | Export-csv C:\file.csv
```
### 5 Exporting Office 365 Licensed
```
Get-MsolUser -All | Where {$_.isLicensed -eq $true} | select userPrincipalName, DisplayName,Department， @{N='License';Expression={$_.Licenses.AccountSkuId}} | Export-CSV c:\scripts\m2\licenses.csv
```

## 4. Importing Information from CSV
### 2 The Basics of Importing from CSV
```
$users = import csv C:\scripts\m3.csv
foreach( $user in $users)
$username = $user.name
```
