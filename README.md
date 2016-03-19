# vagrant-yaml-ansible
### This is customized the [Vagrantfile by HashiCorp](https://www.vagrantup.com/) as to read the GuestVM's settings from the YAML file.

## Features
 - You can define Vagrant's VM settings on the YAML format.
 - You can use multiple VM and multiple OS mixed (Linux and Windows) on Virtualbox with Vagrant.
 - You can use Ansible provisioner of Vagrant.

## Verification Environment
 - Host
   - Mac OSX yosemite
   - VirtualBox 5.0.16
   - Vagrant 1.8.1
   - Ansible 2.1.0
   - ruby 2.0.0p481
 - GuestVM
   - CentOS 6
   - WinowsServer2012R2 

## Usage
 - You can define VM setting on the `conf-vbox-guestvm.yml`
 - The commands are as well as the [Vagrant](https://www.vagrantup.com/docs/cli/).

## Sample YAML

``` conf-vbox-guestvm.yml 
# Define the set value of guestvm in YAML format.
# You can either create a yml file in the current directory, Or "str" described variable.
# (*):The fields are required.
#---
#guestvm:
#  - name:                    * Set the guest VM's name . It is use hostname and Vagrant nodename
#    box:                     * Set the guest VM's box name'.
#    box_url:                   Set the guest VM's box url or path.
#    ipaddress:                 Set the guest VM's ipaddress. If not set ignore template's used.
#    os_type:                 * Set the guest VM's OS of the guest VM in 'windows' or 'linux'.
#    os_username:               Set the guest VM's OS username for ssh or WinRM. If not set ignore.
#    os_password:               Set the guest VM's OS password for ssh or WinRM. If not set ignore.
#    ansible_playbook:          Set the playbook name of Ansible to use provisioning. If not set ignore.
#    ansible_inventory_path:    Set the inventory file path of Ansible to use provisioning. If not set ignore.
#
#
---
win_base: &WIN_BASE
  box:                       win-2012r2-standard-amd64-nocm-mod
  os_type:                   windows
  os_username:               vagrant
  os_password:               vagrant

centos_base: &CENT_BASE
  box:                       geerlingguy/centos6
  os_type:                   linux

guestvm:
  - name:                    win2012-iis01
    ipaddress:               192.168.33.51
    ansible_playbook:        win2012_feature_iis_install.yml
    ansible_inventory_path:  hosts
    << : *WIN_BASE
  - name:                    centos6-httpd01
    ipaddress:               192.168.33.41
    ansible_playbook:        centos6_httpd_install_noproxy.yml
    << : *CENT_BASE
  - name:                    centos6-httpd02
    ipaddress:               192.168.33.42
    ansible_playbook:        centos6_httpd_install_noproxy.yml
    << : *CENT_BASE
```

## `vagrant up` Results

```
/vagrant-yaml-ansible $ vagrant up
Bringing machine 'win2012-iis01' up with 'virtualbox' provider...
Bringing machine 'centos6-httpd01' up with 'virtualbox' provider...
Bringing machine 'centos6-httpd02' up with 'virtualbox' provider...
==> win2012-iis01: Importing base box 'win-2012r2-standard-amd64-nocm-mod'...
==> win2012-iis01: Matching MAC address for NAT networking...
==> win2012-iis01: Setting the name of the VM: vagrant-yaml-ansible_win2012-iis01_1458379134800_68871
==> win2012-iis01: Clearing any previously set network interfaces...
==> win2012-iis01: Preparing network interfaces based on configuration...
    win2012-iis01: Adapter 1: nat
    win2012-iis01: Adapter 2: hostonly
==> win2012-iis01: Forwarding ports...
    win2012-iis01: 5985 (guest) => 55985 (host) (adapter 1)
    win2012-iis01: 5986 (guest) => 55986 (host) (adapter 1)
==> win2012-iis01: Running 'pre-boot' VM customizations...
==> win2012-iis01: Booting VM...
==> win2012-iis01: Waiting for machine to boot. This may take a few minutes...
    win2012-iis01: WinRM address: 127.0.0.1:55985
    win2012-iis01: WinRM username: vagrant
    win2012-iis01: WinRM execution_time_limit: PT2H
    win2012-iis01: WinRM transport: plaintext

==> win2012-iis01: Machine booted and ready!
Sorry, don't know how to check guest version of Virtualbox Guest Additions on this platform. Stopping installation.
==> win2012-iis01: Checking for guest additions in VM...
==> win2012-iis01: Setting hostname...
==> win2012-iis01: Configuring and enabling network interfaces...
==> win2012-iis01: Mounting shared folders...
    win2012-iis01: /vagrant => /Users/foo/vagrant-yaml-ansible
==> win2012-iis01: Running provisioner: ansible...
    win2012-iis01: Running ansible-playbook...

PLAY [all] *********************************************************************

TASK [setup] *******************************************************************
ok: [win2012-iis01]

TASK [win2012-iis01: copy html file] *******************************************
changed: [win2012-iis01]

TASK [win2012-iis01: shuld be Installed IIS] ***********************************
ok: [win2012-iis01]

PLAY RECAP *********************************************************************
win2012-iis01              : ok=3    changed=1    unreachable=0    failed=0   

==> centos6-httpd01: Importing base box 'geerlingguy/centos6'...
==> centos6-httpd01: Matching MAC address for NAT networking...
==> centos6-httpd01: Checking if box 'geerlingguy/centos6' is up to date...
==> centos6-httpd01: Setting the name of the VM: vagrant-yaml-ansible_centos6-httpd01_1458380691988_27772
==> centos6-httpd01: Clearing any previously set network interfaces...
==> centos6-httpd01: Preparing network interfaces based on configuration...
    centos6-httpd01: Adapter 1: nat
    centos6-httpd01: Adapter 2: hostonly
==> centos6-httpd01: Forwarding ports...
    centos6-httpd01: 22 (guest) => 2222 (host) (adapter 1)
==> centos6-httpd01: Running 'pre-boot' VM customizations...
==> centos6-httpd01: Booting VM...
==> centos6-httpd01: Waiting for machine to boot. This may take a few minutes...
    centos6-httpd01: SSH address: 127.0.0.1:2222
    centos6-httpd01: SSH username: vagrant
    centos6-httpd01: SSH auth method: private key
    centos6-httpd01: Warning: Remote connection disconnect. Retrying...
    centos6-httpd01: 
    centos6-httpd01: Vagrant insecure key detected. Vagrant will automatically replace
    centos6-httpd01: this with a newly generated keypair for better security.
    centos6-httpd01: 
    centos6-httpd01: Inserting generated public key within guest...
    centos6-httpd01: Removing insecure key from the guest if it's present...
    centos6-httpd01: Key inserted! Disconnecting and reconnecting using new SSH key...
==> centos6-httpd01: Machine booted and ready!
GuestAdditions 5.0.16 running --- OK.
==> centos6-httpd01: Checking for guest additions in VM...
==> centos6-httpd01: Setting hostname...
==> centos6-httpd01: Configuring and enabling network interfaces...
==> centos6-httpd01: Mounting shared folders...
    centos6-httpd01: /vagrant => /Users/foo/vagrant-yaml-ansible
==> centos6-httpd01: Running provisioner: ansible...
    centos6-httpd01: Running ansible-playbook...

PLAY [all] *********************************************************************

TASK [centos6-httpd01: libselinux-python should have been installed] ***********
ok: [centos6-httpd01]

TASK [centos6-httpd01: httpd should have been installed] ***********************
changed: [centos6-httpd01]

TASK [centos6-httpd01: copy html file] *****************************************
changed: [centos6-httpd01]

TASK [centos6-httpd01: httpd should have been running] *************************
changed: [centos6-httpd01]

TASK [centos6-httpd01: lokkit -p 80:tcp] ***************************************
changed: [centos6-httpd01]

PLAY RECAP *********************************************************************
centos6-httpd01            : ok=5    changed=4    unreachable=0    failed=0   

==> centos6-httpd02: Importing base box 'geerlingguy/centos6'...
==> centos6-httpd02: Matching MAC address for NAT networking...
==> centos6-httpd02: Checking if box 'geerlingguy/centos6' is up to date...
==> centos6-httpd02: Setting the name of the VM: vagrant-yaml-ansible_centos6-httpd02_1458380895063_96616
==> centos6-httpd02: Fixed port collision for 22 => 2222. Now on port 2200.
==> centos6-httpd02: Clearing any previously set network interfaces...
==> centos6-httpd02: Preparing network interfaces based on configuration...
    centos6-httpd02: Adapter 1: nat
    centos6-httpd02: Adapter 2: hostonly
==> centos6-httpd02: Forwarding ports...
    centos6-httpd02: 22 (guest) => 2200 (host) (adapter 1)
==> centos6-httpd02: Running 'pre-boot' VM customizations...
==> centos6-httpd02: Booting VM...
==> centos6-httpd02: Waiting for machine to boot. This may take a few minutes...
    centos6-httpd02: SSH address: 127.0.0.1:2200
    centos6-httpd02: SSH username: vagrant
    centos6-httpd02: SSH auth method: private key
    centos6-httpd02: Warning: Remote connection disconnect. Retrying...
    centos6-httpd02: 
    centos6-httpd02: Vagrant insecure key detected. Vagrant will automatically replace
    centos6-httpd02: this with a newly generated keypair for better security.
    centos6-httpd02: 
    centos6-httpd02: Inserting generated public key within guest...
    centos6-httpd02: Removing insecure key from the guest if it's present...
    centos6-httpd02: Key inserted! Disconnecting and reconnecting using new SSH key...
==> centos6-httpd02: Machine booted and ready!
GuestAdditions 5.0.16 running --- OK.
==> centos6-httpd02: Checking for guest additions in VM...
==> centos6-httpd02: Setting hostname...
==> centos6-httpd02: Configuring and enabling network interfaces...
==> centos6-httpd02: Mounting shared folders...
    centos6-httpd02: /vagrant => /Users/foo/vagrant-yaml-ansible
==> centos6-httpd02: Running provisioner: ansible...
    centos6-httpd02: Running ansible-playbook...

PLAY [all] *********************************************************************

TASK [centos6-httpd02: libselinux-python should have been installed] ***********
ok: [centos6-httpd02]

TASK [centos6-httpd02: httpd should have been installed] ***********************
changed: [centos6-httpd02]

TASK [centos6-httpd02: copy html file] *****************************************
changed: [centos6-httpd02]

TASK [centos6-httpd02: httpd should have been running] *************************
changed: [centos6-httpd02]

TASK [centos6-httpd02: lokkit -p 80:tcp] ***************************************
changed: [centos6-httpd02]

PLAY RECAP *********************************************************************
centos6-httpd02            : ok=5    changed=4    unreachable=0    failed=0   

~/vagrant-yaml-ansible $
```

## Other
### Ansible provisioner
 - Please refer to here as a [Vagrant reference](https://www.vagrantup.com/docs/provisioning/ansible.html).

### Sample Windows box
`win-2012r2-standard-amd64-nocm-mod` box is based [`opentable/win-2012r2-standard-amd64-nocm`](https://atlas.hashicorp.com/opentable/boxes/win-2012r2-standard-amd64-nocm)
and add the settings below for use Ansible provisoner.

+ Vagrant Connect Network `192.168.33.0/24` chaged from Public to Private and Private Network Firewall changed off.

```
Windows PowerShell
Copyright (C) 2013 Microsoft Corporation. All rights reserved.

PS C:\Users\Administrator> Get-NetConnectionProfile

Name             : Network
InterfaceAlias   : Ethernet
InterfaceIndex   : 12
NetworkCategory  : Public
IPv4Connectivity : Internet
IPv6Connectivity : NoTraffic

Name             : Unidentified network
InterfaceAlias   : Ethernet 2
InterfaceIndex   : 15
NetworkCategory  : Public
IPv4Connectivity : LocalNetwork
IPv6Connectivity : NoTraffic

PS C:\Users\Administrator> Get-NetConnectionProfile |  where Name -eq 'Unidentified network'

Name             : Unidentified network
InterfaceAlias   : Ethernet 2
InterfaceIndex   : 15
NetworkCategory  : Public
IPv4Connectivity : LocalNetwork
IPv6Connectivity : NoTraffic

PS C:\Users\Administrator> Get-NetConnectionProfile |  where Name -eq 'Unidentified network' | Set-NetconnectionProfile
-NetworkCategory Private
PS C:\Users\Administrator> Get-NetConnectionProfile |  where Name -eq 'Unidentified network'

Name             : Unidentified network
InterfaceAlias   : Ethernet 2
InterfaceIndex   : 15
NetworkCategory  : Private
IPv4Connectivity : LocalNetwork
IPv6Connectivity : NoTraffic
```

+ Add WinRM setting.

```
Microsoft Windows [Version 6.3.9600]
(c) 2013 Microsoft Corporation. All rights reserved.

C:\Users\Administrator>   winrm quickconfig -q
WinRM service is already running on this machine.
WinRM is already set up for remote management on this computer.

C:\Users\Administrator>   winrm set winrm/config/winrs @{MaxMemoryPerShellMB="512"}
Winrs
    AllowRemoteShellAccess = true
    IdleTimeout = 7200000
    MaxConcurrentUsers = 10
    MaxShellRunTime = 2147483647
    MaxProcessesPerShell = 25
    MaxMemoryPerShellMB = 512
    MaxShellsPerUser = 30


C:\Users\Administrator>   winrm set winrm/config @{MaxTimeoutms="1800000"}
Config
    MaxEnvelopeSizekb = 500
    MaxTimeoutms = 1800000
    MaxBatchItems = 32000
    MaxProviderRequests = 4294967295
    Client
        NetworkDelayms = 5000
        URLPrefix = wsman
        AllowUnencrypted = false
        Auth
            Basic = true
            Digest = true
            Kerberos = true
            Negotiate = true
            Certificate = true
            CredSSP = false
        DefaultPorts
            HTTP = 5985
            HTTPS = 5986
        TrustedHosts
    Service
        RootSDDL = O:NSG:BAD:P(A;;GA;;;BA)(A;;GR;;;IU)S:P(AU;FA;GA;;;WD)(AU;SA;GXGW;;;WD)
        MaxConcurrentOperations = 4294967295
        MaxConcurrentOperationsPerUser = 1500
        EnumerationTimeoutms = 240000
        MaxConnections = 300
        MaxPacketRetrievalTimeSeconds = 120
        AllowUnencrypted = true
        Auth
            Basic = true
            Kerberos = true
            Negotiate = true
            Certificate = false
            CredSSP = false
            CbtHardeningLevel = Relaxed
        DefaultPorts
            HTTP = 5985
            HTTPS = 5986
        IPv4Filter = *
        IPv6Filter = *
        EnableCompatibilityHttpListener = false
        EnableCompatibilityHttpsListener = false
        CertificateThumbprint
        AllowRemoteAccess = true
    Winrs
        AllowRemoteShellAccess = true
        IdleTimeout = 7200000
        MaxConcurrentUsers = 10
        MaxShellRunTime = 2147483647
        MaxProcessesPerShell = 25
        MaxMemoryPerShellMB = 512
        MaxShellsPerUser = 30


C:\Users\Administrator>   winrm set winrm/config/service @{AllowUnencrypted="true"}
Service
    RootSDDL = O:NSG:BAD:P(A;;GA;;;BA)(A;;GR;;;IU)S:P(AU;FA;GA;;;WD)(AU;SA;GXGW;;;WD)
    MaxConcurrentOperations = 4294967295
    MaxConcurrentOperationsPerUser = 1500
    EnumerationTimeoutms = 240000
    MaxConnections = 300
    MaxPacketRetrievalTimeSeconds = 120
    AllowUnencrypted = true
    Auth
        Basic = true
        Kerberos = true
        Negotiate = true
        Certificate = false
        CredSSP = false
        CbtHardeningLevel = Relaxed
    DefaultPorts
        HTTP = 5985
        HTTPS = 5986
    IPv4Filter = *
    IPv6Filter = *
    EnableCompatibilityHttpListener = false
    EnableCompatibilityHttpsListener = false
    CertificateThumbprint
    AllowRemoteAccess = true


C:\Users\Administrator>   winrm set winrm/config/service/auth @{Basic="true"}
Auth
    Basic = true
    Kerberos = true
    Negotiate = true
    Certificate = false
    CredSSP = false
    CbtHardeningLevel = Relaxed


C:\Users\Administrator>   sc config WinRM start= auto
[SC] ChangeServiceConfig SUCCESS

C:\Users\Administrator>
```
