
###### If you like it, please consider buy me a beer :beer:
###### [![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=6NKR7XQH5E2P2&source=url)


# SnsPsScriptsMonitoring


## Requirements

* .NET Framework 4.5
* PowerShell 4


## Important

The Monitoring DataBase schema is different stating from version 1.0.0.6. If you upgrade from previous versions please make
sure that you will `Import-Module` in elevated PowerShell for the first time. The existing database will be backed up and
kept for 61 days. The backup file will be in the same folder as the production database. You can use any SQLite tools to
extract the information from the backup. The information which already exist in the Monitoring DataBase will be not
migrated into the new database.


## Instructions

To install the module from PowerShell Gallery simply run
```powershell
Install-Module "SnsPsScriptsMonitoring" -Scope "AllUsers";
```
OR
1. Download SnsPsScriptsMonitoring.zip.
2. Don't forget to check the .ZIP file for viruses and etc.
3. File MD5 hash: `3990C2F07C3EE5E80D1E61D0C78EFFAE`
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

The SnsPsScriptsMonitoring module was made to monitor PowerShell scripts operation. It can be used to monitor only
scripts running on the same machine when no MS Orchestrator server is available.

To monitor a script, it must make a transcript file or log file and to insert updates in those files regularly. In the
begging, the monitored script must load this module and run the command let Enable-SnsScriptMonitoring and
provide:
- the full absolute path to the transcript or log file
- time threshold in minutes used to decide whether the monitored script is stuck
- script’s own Windows Scheduled Task if the script is started via task on a schedule.

Prepare in advance and run on a schedule a monitoring script which runs the command let Get-SnsMonitoringEntry
with the needed parameters according to the preferences. You can use the output for whatever you name, send report
with the failed scripts on email, upload the report on SharePoint, insert it on DataBase etc. You must write that in your
script. Get-SnsMonitoringEntry can restart the failed scripts if it is specified to its parameters and the Monitoring Script
is running in Elevated mode and all the needed information is provided during the enabling. In All the rest scenarios it
only reports.

For failed scripts are considered the ones which Windows process is no longer running, or scripts which did not update
their transcript or log files for the specified with the Threshold parameter amount of time.

At the end of the monitored script, it must run Disable-SnsScriptMonitoring command let, to disable its monitoring
entry and to prevent successfully completed scripts to be reported as failed or started again.

The module has no network capabilities other than verification for newer versions which can be disabled. It cannot
monitor scripts that are not running on the same machine. The transcript or log files must be located on the same
machine, or to be presented in a way like they are on the same machine, for example via mapping of network drive. If
you need to have more complex monitoring solution that monitors scripts running on multiple machines, you can
make the monitoring scripts as agents running locally that send the output on central location. In all the cases it is
your responsibility to make a monitoring script which uses the Get-SnsMonitoringEntry.

The SnsPsScriptsMonitoring is using SQLite V3 Database called Monitoring DataBase, in order the module CmdLets to
exchange information about the Monitored Scripts. The SQLite DataBase is Serverless Open-Source DataBase solution,
where the DataBase is a file stored locally. On the first import of the module, the Monitoring DataBase will be created.
In Order this to happen smoothly the first import of the module must happen in Elevated PowerShell window.

For additional information, please use the CmdLets built-in help.
```powershell
Get-Help Enable-SnsScriptMonitoring -Full;
Get-Help Disable-SnsScriptMonitoring -Full;
Get-Help Get-SnsScriptMonitoringEntry -Full;
```


## External Links

- svesavov on GitHub: [https://github.com/svesavov](https://github.com/svesavov)
- svesavov on PowerShell Gallery: [https://www.powershellgallery.com/packages/SnsPsScriptsMonitoring/](https://www.powershellgallery.com/packages/SnsPsScriptsMonitoring/)
- Svetoslav Savov on LinkedIn [https://www.linkedin.com/in/svetoslavsavov](https://www.linkedin.com/in/svetoslavsavov)
- SQLite V3: [https://sqlite.org/index.html](https://sqlite.org/index.html)
