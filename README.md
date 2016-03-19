# ansible_windows_repo

Ansible Repo for windows support


## Using AWS or any cloud where servers can be access via IP addresses.

1. Create a windows 2012 R2 server and a linux server using a keypair using say AWS Cloud Formation.
2. In ansible_windows_repo update the inventory/hosts_ssh with IP address and Administrator password of windows server.
3. In the .kitchen.yml.ssh file
   * Set the ssh_key  to the aws keypair for linux server e.g. spec/test.pem
   * Set the hostname to ipadress of linux server  e.g.'54.229.103.38'
4. In the group_var/windows-server.xml
   * set the ansible_ssh_pass to the Windows administrator password
5. To configure Windows Server logon using say RDP and:
   * check winmrm enable by running: `winrm quickconfig`
   * Run the powershell script [ConfigureRemotingForAnsible.ps1](https://github.com/ansible/ansible/blob/devel/examples/scripts/ConfigureRemotingForAnsible.ps1) to configure the server for remote access from ansible.
   * if you get error "running scripts is disabled on your system" then in powershell run `Set-ExecutionPolicy RemoteSigned`
   * Make sure the firewall is open for winrm requests by running `netsh advfirewall firewall add rule name="WinRM-HTTPS" dir=in localport=5986 protocol=TCP action=allow`

##  Using Vagrant.

1. Typically the user is `vangrant` and password `vagrant`.
2. use the test-kitchen file `.kitchen.yml.vagrant`.
3. Same issues apply for configuring the Windows Server.

## Test winrm access

To test accessing the windows severs from the linux box that runs ansible (where xxxxxxxx is the Administrator password):

```
python
import winrm
winrmsession = winrm.Session('10.0.2.15',auth=('Administrator','xxxxxxxx'))
winrmsession
result = winrmsession.run_ps("(Get-WindowsFeature).Where{$PSItem.Installed}")
```

## References:

http://www.hurryupandwait.io/blog/understanding-and-troubleshooting-winrm-connection-and-authentication-a-thrill-seekers-guide-to-adventure

https://www.mail-archive.com/ansible-project@googlegroups.com/msg19494.html

http://pubs.vmware.com/orchestrator-plugins/index.jsp?topic=%2Fcom.vmware.using.powershell.plugin.doc_10%2FGUID-D4ACA4EF-D018-448A-866A-DECDDA5CC3C1.html

https://4sysops.com/archives/powershell-remoting-over-https-with-a-self-signed-ssl-certificate/

https://support.microsoft.com/en-us/kb/2019527

https://blogs.technet.microsoft.com/meamcs/2012/02/24/how-to-force-winrm-to-listen-interfaces-over-https/

