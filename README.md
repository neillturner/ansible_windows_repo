# ansible_windows_repo
ansible repo for windows support


to test accessing the windows severs from the linux box that runs ansible: 

```
python 
import winrm 
winrmsession = winrm.Session('10.0.2.15',auth=('vagrant','vagrant'))
winrmsession 
result = winrmsession.run_ps("(Get-WindowsFeature).Where{$PSItem.Installed}")
```

```
winrmsession = winrm.Session('http://10.0.2.15:5985',auth=('vagrant','vagrant'))
winrmsession = winrm.Session('https://10.0.2.15:5986',auth=('vagrant','vagrant'))
winrmsession = winrm.Session('10.0.2.15',auth=(None, None))
``````

http://pubs.vmware.com/orchestrator-plugins/index.jsp?topic=%2Fcom.vmware.using.powershell.plugin.doc_10%2FGUID-D4ACA4EF-D018-448A-866A-DECDDA5CC3C1.html

https://4sysops.com/archives/powershell-remoting-over-https-with-a-self-signed-ssl-certificate/

https://support.microsoft.com/en-us/kb/2019527

https://blogs.technet.microsoft.com/meamcs/2012/02/24/how-to-force-winrm-to-listen-interfaces-over-https/

http://www.hurryupandwait.io/blog/understanding-and-troubleshooting-winrm-connection-and-authentication-a-thrill-seekers-guide-to-adventure