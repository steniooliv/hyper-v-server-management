# Enable Hyper-V Remote Management

- Check the index of ethernet interface
  - `Get-DnsClient`
  
- Put the servers and clients on same Workgroud and set a DNS Suffix, in sever core run the command line above changing the InterfaceIndex and DNS Suffix, after reboot the system
  - `Set-DnsClient -InterfaceIndex 4 -ConnectionSpecificSuffix "cartorio.local"`

- Install the Hyper-V Management on Features of Windows 10

- Edit host files on Windows 10 and the Hyper-V Server
  - `Get-Content -Path "C:\Windows\System32\drivers\etc\hosts" #Verify host files entries`
  - `Add-Content -Path "C:\Windows\System32\drivers\etc\hosts" -Value "192.168.0.11 SR1.cartorio.local" #Add host entries`
  - `Add-Content -Path "C:\Windows\System32\drivers\etc\hosts" -Value "192.168.0.20 W10.cartorio.local" #Add host entries`

- Allow Firewall rules, on Windows 10 may be change on Windows Firewall with Advanced Security, on Hyper-V Server run Powershell command lines for us-en or pt-br
  - `Enable-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" #English`
  - `Enable-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv6-In)" #English`
  
  - `Enable-NetFirewallRule -DisplayName "Compartilhamento de Arquivo e Impressora (Solicitação de Eco - ICMPv4-In)" #Portuguese`
  - `Enable-NetFirewallRule -DisplayName "Compartilhamento de Arquivo e Impressora (Solicitação de Eco - ICMPv6-In)" #Portuguese`
  
- Change Network Profile to Private, make same on Windows 10
  - `Get-NetConnectionProfile #Verify Network Profile`
  - `Set-NetConnectionProfile -InterfaceAlias "Ethernet" -NetworkCategory Private #Change the interface Ethernet to Private Category`

- Now in Windows 10 enable the Remote Management
  - `Enable-PSRemoting`

- On Windows 10, enable domain delegation
  - `Get-WSManCredSSP #Verify`
  - `Enable-WSManCredSSP -Role Client -DelegateComputer "*.cartorio.local" #Add delegation, change the domain`

- On Windows 10, add the domain to the Trusted Host
  - `Get-Item -Path WSMan:\localhost\Client\TrustedHosts #Verify trusted hosts`
  - `Set-Item -Path WSMan:\localhost\Client\TrustedHosts -Value "*.cartorio.local" #Add domain on trusted hosts, change the domain`

- On Windows 10, add credential
  - `cmdkey /list #List credentials`
  - `cmdkey /add:SR1.cartorio.local /user:Administrador /pass:abc123. #Add the Hyper-V Server credentials, change to your hostname, user, and pass`

- On Windows 10, verify the connection with the Hyper-V Server, change you hostname and suffix
  - `Enter-PSSession -ComputerName SR1.cartorio.local`

- Now, open the Hyper-V Management on Windows 10 and connect with the Hyper-V Server

