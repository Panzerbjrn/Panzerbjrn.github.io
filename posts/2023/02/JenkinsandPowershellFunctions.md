So, I recently had a problem with a PowerShell script I was deploying to Jenkins.
The error I got was this:
```
Read-DBData : Cannot process argument transformation on parameter 'ServerName'.
Cannot convert the "DBServer01" value of type "System.String" to type "System.Management.Automation.ParameterAttribute".
At D:\Jenkins\workspace\DailyFeedback.ps1:10 char:9
```

How odd I thought, *everything worked fine on my machine*...

The fuction being called looked like this:
```
Function Read-DBData{
[CmdletBinding()]
	param (
		[Parameter][string]$DBServer,
		[Parameter][string]$DBName,
		[Parameter][string]$TableName
	)

	$SQLQuery = "select * from dbo.$TableName"

	TRY{
		$InvokeSqlcmdSplat = @{
			ServerInstance = $DBServer
			Database = $DBName
			Query = $SQLQuery
			ErrorAction = "Stop"
			MaxCharLength = 44444444
		}
		$Data = Invoke-Sqlcmd @InvokeSqlcmdSplat
	}
	CATCH{
		THROW "Reserve request row failed. Server: '$DBServer' DB: '$DBName' Error: $($_.Exception.Message)"
	}
	return $Data
}
```
Fairly basic, and nothing strange about it. *However*...
