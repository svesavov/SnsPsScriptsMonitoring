
###### If you like it, please consider buy me a beer :beer:
###### [![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=6NKR7XQH5E2P2&source=url)


# SnsPsScriptsMonitoring


## Requirements

* .NET Framework 4.5.2
* PowerShell 4


## Important

The Monitoring DataBase schema is modified in version 1.0.0.8. The existing database will be backed up and kept for 61 days. The backup file will be in the same folder as the production database. You can use any SQLite tools to extract the information from the backup. The information which already exist in the Monitoring DataBase will be not migrated into the new database. Please make sure there are no running monitored scripts at the time. In case there are, their corresponding monitoring entries will remain in the backup, and they will be no longer monitored, until their next run. This will not stop or restart the running monitored scripts.


## Instructions

To install the module from PowerShell Gallery simply run
```powershell
Install-Module "SnsPsScriptsMonitoring" -Scope "AllUsers";
```
OR
1. Download SnsPsScriptsMonitoring.zip.
2. Don't forget to check the .ZIP file for viruses and etc.
3. File MD5 hash: `B4AFD823C5BAF0AAC382AA6939D9FCC5`
4. Unzip in one of the following folders depending of your preference:
* `C:\Users\UserName\Documents\WindowsPowerShell\Modules` - Replace "UserName" with the actual username, If you want the module to be available for specific user.
* `C:\Program Files\WindowsPowerShell\Modules` - If you want the module to be available for all users on the machine.
* Or any other location present in `$env:PSModulePath`
5. Run the following command replacing "PathWhereModuleIsInstalled" with the actual path where the module files were unzipped.
```powershell
Get-ChildItem -Path "PathWhereModuleIsInstalled" -Recurse | Unblock-File
```
6. Enjoy!


## Functionality

The SnsPsScriptsMonitoring module was made to monitor PowerShell scripts operation. It is intended to monitor scripts running locally on the same machine whenever no MS Orchestrator server is available.

For failed scripts are considered the ones which Windows process is no longer running. For stuck are considered scripts which are stopped somewhere, without being able to continue further. In that case their Windows process is running and doing nothing. To be possible the enumeration of stuck scripts, the module requires the monitored scripts to regularly write updates in a transcript or log file. The module monitors whether the transcript or log file LastWriteTime is within a threshold, specified during the enabling for monitoring.

In the beginning, a monitored script must load this module, run Enable-SnsScriptMonitoring CmdLet and specify:
- The full absolute path to the transcript or log file.
- Script’s own Windows Scheduled Task if the script is started via task on a schedule. When omitted the CmdLet will try to enumerate the scheduled task which starts the script. This enumeration might not succeed to identify the correct scheduled task. When missing the monitoring CmdLet Get-SnsScriptMonitoringEntry will ignore the users input in RestartFailed parameter and will not restart the failed script Windows Scheduled Task.
- A time threshold in minutes used to evaluate whether the monitored script is stuck. When omitted will be used the default value of 15 minutes.

Prepare in advance and execute on a schedule a monitoring script which uses Get-SnsScriptMonitoringEntry CmdLet with the needed parameters according to the preferences. You can use the output for whatever you name, send report with the failed scripts on email, upload the report on SharePoint, insert it on DataBase and etc. Get-SnsScriptMonitoringEntry can restart the failed scripts when:
- It is requested via parameter RestartFailed.
- The Monitoring Script is running in elevated mode.
- All the needed information is provided during the enabling.
In the rest scenarios Get-SnsScriptMonitoringEntry only reports.

At the end of the monitored script, it must run Disable-SnsScriptMonitoring CmdLet, to disable its monitoring entry and to prevent successfully completed scripts to be reported as failed or started again. In case this step is omitted, and the monitoring entry contains the scheduled task for the monitored script, Get-SnsScriptMonitoringEntry CmdLet will evaluate the task, and will automatically disable the monitoring entry when the task has status "Ready”, and the last run result is "The operation completed successfully. (0x0)". Since this have too many prerequisites it is always preferable the monitored scripts to disable themselves from monitoring upon successful completion.

The module has no network capabilities other than verification for newer versions which could be disabled. It cannot monitor scripts that are not running on the same machine. The transcript or log files must be located on the same machine, or to be presented in a way like they are on the same machine, for example via mapping of network drive. If you need to have more complex monitoring solution that monitors scripts running on multiple machines, you can make the monitoring scripts to function as agents running locally and send the output on central location.

The module's CmdLets exchange information about the monitored scripts via SQLite V3 Database called Monitoring DataBase. The SQLite DataBase is Serverless Open-Source DataBase solution, where the DataBase is a file stored locally. The Monitoring DataBase will be automatically created On the first import of the module and verified on each consequent module import. This requires the first module import to happen in Elevated PowerShell window. The monitoring DataBase is located at `%ProgramData%\SnsPsScriptsMonitoring\SnsScrMon.db`.

For additional information, please use the CmdLets built-in help.
```powershell
Get-Help Enable-SnsScriptMonitoring -Full;
Get-Help Disable-SnsScriptMonitoring -Full;
Get-Help Get-SnsScriptMonitoringEntry -Full;
```


## External Links

- svesavov on GitHub: [https://github.com/svesavov](https://github.com/svesavov)
- svesavov on PowerShell Gallery: [https://www.powershellgallery.com/packages/SnsPsScriptsMonitoring/](https://www.powershellgallery.com/packages/SnsPsScriptsMonitoring/)
- Svetoslav Savov on LinkedIn: [https://www.linkedin.com/in/svetoslavsavov](https://www.linkedin.com/in/svetoslavsavov)
- SQLite V3: [https://sqlite.org/index.html](https://sqlite.org/index.html)
