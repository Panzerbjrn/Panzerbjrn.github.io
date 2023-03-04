---
title: my title
author: my name
date: today
---

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
I discovered something new, at leat new to me.
In the line `[Parameter][string]$....` not having something after the parameter, *can* cause PowerShell to not understand the parameter you're trying to use, and this was causing the error.
I tried RDPing to the Jenkins server, and sure enough, running this in a console on that server, caused the same error.

Very odd, and something I'd never experienced before, but hey ho.
So a quick edit later, and the new function works flawlessly:
```
Function Read-DBData{
[CmdletBinding()]
	param (
		[string]$DBServer,
		[string]$DBName,
		[string]$TableName
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

At least I learned something new.