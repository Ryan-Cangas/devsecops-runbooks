# FortiGate Firewall Integration

Favorite: No
Archive: No
Notebook: Wazuh SIEM/SOC
Edited: July 2, 2026 11:40 AM
Created: July 1, 2026 6:57 PM

# Installing KVM qcow2 from Fortinet

1. Download the KVM ISO from the Support Page https://support.fortinet.com/support/#/downloads/vm. I’m going to install version 7.6.7, for familiarity as 8.0.0 has new upgrades. **Note:** Both versions are stable versions.

![image.png](/Images/image.png)

2. Extract the qcow file inside the ZIP which will uploaded to my Proxmox Host.

![image.png](/Images/image%201.png)

3. Inside the terminal of the Downloads directory (where the qcow2 file is), SCP (Secure Copy Protocol) to transfer this qcow2 file into my Proxmox host in the specified path.

```bash
PS C:\Users\user> scp fortios.qcow2 user@xxx.xxx.x.xxx:/var/lib/vz/template/iso/
user@xxx.xxx.x.xxx's password:
fortios.qcow2                                                                         100%  117MB  11.3MB/s   00:10
PS C:\Users\user>
```

4. Create an empty VM shell (that matches Fortinet’s evaluation limits so licensing has no errors).

```bash
user@pve-server:~# qm create 200 --name FortiGate-Firewall --memory 2048 --cores 1 --cpu host --net0 virtio,bridge=vmbr0
```

After this, it can be confirmed in the Web UI, the VM was created

![image.png](/Images/image%202.png)

5. Import the disk into the active storage pool. This command will convert the qcow2 file into a bootable virtual disk that will be assigned to the new VM that was created.

```bash
user@pve-server:~# qm importdisk 200 /var/lib/vz/template/iso/fortios.qcow2 local-lvm
importing disk '/var/lib/vz/template/iso/fortios.qcow2' to VM 200 ...
  Logical volume "vm-200-disk-0" created.
  Logical volume pve/vm-200-disk-0 changed.
transferred 0.0 B of 2.0 GiB (0.00%)
transferred 43.8 MiB of 2.0 GiB (2.14%)
transferred 79.1 MiB of 2.0 GiB (3.86%)
transferred 114.1 MiB of 2.0 GiB (5.57%)
transferred 149.3 MiB of 2.0 GiB (7.29%)
transferred 184.3 MiB of 2.0 GiB (9.00%)
transferred 219.5 MiB of 2.0 GiB (10.72%)
transferred 254.6 MiB of 2.0 GiB (12.43%)
transferred 289.8 MiB of 2.0 GiB (14.15%)
transferred 324.8 MiB of 2.0 GiB (15.86%)
transferred 360.0 MiB of 2.0 GiB (17.58%)
transferred 395.1 MiB of 2.0 GiB (19.29%)
transferred 430.3 MiB of 2.0 GiB (21.01%)
transferred 465.3 MiB of 2.0 GiB (22.72%)
transferred 500.5 MiB of 2.0 GiB (24.44%)
transferred 535.6 MiB of 2.0 GiB (26.15%)
transferred 570.8 MiB of 2.0 GiB (27.87%)
transferred 605.8 MiB of 2.0 GiB (29.58%)
transferred 641.0 MiB of 2.0 GiB (31.30%)
transferred 676.0 MiB of 2.0 GiB (33.01%)
transferred 711.3 MiB of 2.0 GiB (34.73%)
transferred 746.3 MiB of 2.0 GiB (36.44%)
transferred 781.5 MiB of 2.0 GiB (38.16%)
transferred 816.5 MiB of 2.0 GiB (39.87%)
transferred 851.8 MiB of 2.0 GiB (41.59%)
transferred 886.8 MiB of 2.0 GiB (43.30%)
transferred 922.0 MiB of 2.0 GiB (45.02%)
transferred 957.0 MiB of 2.0 GiB (46.73%)
transferred 992.3 MiB of 2.0 GiB (48.45%)
transferred 1.0 GiB of 2.0 GiB (50.16%)
transferred 1.0 GiB of 2.0 GiB (51.88%)
transferred 1.1 GiB of 2.0 GiB (53.59%)
transferred 1.1 GiB of 2.0 GiB (55.31%)
transferred 1.1 GiB of 2.0 GiB (57.02%)
transferred 1.2 GiB of 2.0 GiB (58.74%)
transferred 1.2 GiB of 2.0 GiB (60.45%)
transferred 1.2 GiB of 2.0 GiB (62.17%)
transferred 1.3 GiB of 2.0 GiB (63.88%)
transferred 1.3 GiB of 2.0 GiB (65.59%)
transferred 1.3 GiB of 2.0 GiB (67.31%)
transferred 1.4 GiB of 2.0 GiB (69.02%)
transferred 1.4 GiB of 2.0 GiB (70.74%)
transferred 1.4 GiB of 2.0 GiB (72.45%)
transferred 1.5 GiB of 2.0 GiB (74.17%)
transferred 1.5 GiB of 2.0 GiB (75.88%)
transferred 1.6 GiB of 2.0 GiB (77.60%)
transferred 1.6 GiB of 2.0 GiB (79.31%)
transferred 1.6 GiB of 2.0 GiB (81.03%)
transferred 1.7 GiB of 2.0 GiB (82.74%)
transferred 1.7 GiB of 2.0 GiB (84.46%)
transferred 1.7 GiB of 2.0 GiB (86.17%)
transferred 1.8 GiB of 2.0 GiB (87.89%)
transferred 1.8 GiB of 2.0 GiB (89.60%)
transferred 1.8 GiB of 2.0 GiB (91.32%)
transferred 1.9 GiB of 2.0 GiB (93.03%)
transferred 1.9 GiB of 2.0 GiB (94.80%)
transferred 1.9 GiB of 2.0 GiB (96.52%)
transferred 2.0 GiB of 2.0 GiB (98.23%)
transferred 2.0 GiB of 2.0 GiB (100.00%)
transferred 2.0 GiB of 2.0 GiB (100.00%)
unused0: successfully imported disk 'local-lvm:vm-200-disk-0'
```

6. Modify the VM virtual storage controller to virtio-scsi-pci, so it transfers data faster to the SSD, and attach the imported storage volume as primary hard drive on virtual slot scsi0.

```bash
user@pve-server:~# qm set 200 --scsihw virtio-scsi-pci --scsi0 local-lvm:vm-200-disk-0
update VM 200: -scsi0 local-lvm:vm-200-disk-0 -scsihw virtio-scsi-pci
```

7. Change the boot order, this is a must so it does not look for a bootable drive. We have already setup the boot, so it will look for it on scsi0 immediately when powered on.

```bash
user@pve-server:~# qm set 200 --boot order=scsi0
update VM 200: -boot order=scsi0
```

# FortiGate VM Setup

1. Power on the VM and setup the credentials. **Note**: When it initially asks for password, just press enter and it will prompt you to give new password. My credentials will be ####:####

![image.png](/Images/image%203.png)

2. Edit port 1 interface, so I can SSH into the VM; this makes it easier to edit settings and copy paste scripts into the shell.

![image.png](/Images/image%204.png)

3. I will make the IP address of this VM static, because I don’t want to setup DHCP Reservation on the home router settings. As per the router, the assigned IP for this VM is .xxx.

```powershell
PS C:\Users\user> ssh user@xxx.xxx.x.xxx
user@xxx.xxx.x.xxx's password:
FortiGate-VM64-KVM # config system interface //open system interface configuration

FortiGate-VM64-KVM (interface) # edit port1 //edit the primary virtual network interface that is connected to Proxmox bridge (vmbr0)

FortiGate-VM64-KVM (port1) # set mode static //disable DHCP and switch interface to static IP mode

FortiGate-VM64-KVM (port1) # set ip 192.168.1.195 255.255.255.0 //assign the static IP and the network subnet mask

FortiGate-VM64-KVM (port1) # set allowaccess ping ssh https http //configure access protocols allowed on this interface

FortiGate-VM64-KVM (port1) # next //close the editing window
1 admin session(s) are currently connected on this interface.
Are you sure you want to continue? (y/n)y

FortiGate-VM64-KVM (interface) # end //compile and write
```

4. Setup firewall to have default route to exit the local subnet. This default static route pointing out of port1 to home network gateway router.

```bash
config router static
    edit 1
        set dst 0.0.0.0 0.0.0.0
        set gateway 192.168.1.1
        set device "port1"
    next
end
```

5. To solve the dns resolve errors during account validation, I will point the the FW internal DNS settings to public Google DNS so it can find the Fortinet licensing network.

```bash
config system dns
    set primary 8.8.8.8
    set secondary 1.1.1.1
end
```

6. Setup the syslog setting so fortigate kernel instantly connects into UDP channel and sends logs to my Ubuntu network, into the Wazuh docker container.

```powershell
PS C:\Users\user> ssh user@xxx.xxx.x.xxx
user@xxx.xxx.x.xxx's password:
FortiGate-VM64-KVM # config log syslogd setting

FortiGate-VM64-KVM (setting) # set status enable

FortiGate-VM64-KVM (setting) # set server 192.168.1.xxx

FortiGate-VM64-KVM (setting) # set port 5140

FortiGate-VM64-KVM (setting) # set mode udp

FortiGate-VM64-KVM (setting) # set format default

FortiGate-VM64-KVM (setting) # end

FortiGate-VM64-KVM #
```

7. Enable the syslog setting

```bash
config log setting
    set status enable
end
```

# FortiGate Web UI

1. Enter the email and password that was used to sign up in the Fortinet Support website

![image.png](/Images/image%205.png)

2. It will verify from the licensing network of Fortinet and will reboot the FortiGate VM

![Screenshot 2026-07-02 105453.png](/Images/Screenshot_2026-07-02_105453.png)

3. After activating the free license (which is permanent), you can now setup preferred FortiGate configurations, these are mine.

![Screenshot 2026-07-02 105736.png](/Images/Screenshot_2026-07-02_105736.png)

![Screenshot 2026-07-02 105749.png](/Images/Screenshot_2026-07-02_105749.png)

![Screenshot 2026-07-02 112230.png](/Images/Screenshot_2026-07-02_112230.png)

4. You change the Hostname of the firewall through the CLI or the Web UI, I used the CLI as it’s my preference.

```bash
config system global
    set hostname <Your_New_Name>
end
```

# Docker Compose Setup (Wazuh)

1. Edit docker-compose.yml to map host port to container port

```bash
 ports:
      - "1514:1514"
      - "1515:1515"
      - "55000:55000"
      - "5140:514/udp"
```

2. Spin the Docker Compose again using docker compose up -d to deploy stack changes.
3. To edit the ossec.conf file of Wazuh, it is easier to do this by using the Web UI. Go to Settings > Server Management > Configuration. Here you can click edit configuration to edit the ossec.conf file easily.

![image.png](/Images/image%206.png)

4. Add the remote listener for docker to listen to the syslogs received from Fortinet FW. Save the new configuration and then restart the manager. In my case the IP is 0.0.0.0, this is ONLY for the homelab, because I had issues with IP setup, so I set it to listen to all network interfaces.

![image.png](/Images/image%207.png)

5. I used a diagnose log test in the Fortinet KVM for simulating logs because I don’t have access to the Fortinet Web UI (White screen issue; could be related to licensing or SSL).

```bash
FortiGate-VM64-KVM # diagnose log test
masks:
        Virus: 0x00001
        URL: 0x00002
        DLP: 0x00004
        IPS: 0x00008
        BOTNET: 0x00010
        ANOMALLY: 0x00020
        APP: 0x00040
        APP6: 0x00080
        Deep App: 0x00100
        Email: 0x00200
        CR Web: 0x00400
        SSH: 0x00800
        SSL: 0x01000
        FSW: 0x02000
        File Filter: 0x04000
        ICAP: 0x08000
        SCTP Filter: 0x10000
        Virtual Patch: 0x20000
        CASB: 0x40000
diag log test <repeat> [<sleep-duration(milliseconds)> <# of srcip> <# of dstip> <gen-utm-traffic-log> <seed> <masks>]
diag log test (repeat: 1)  (sleep-duration(milliseconds): 10) (# of srcip: 10) (# of dstip: 10) (gen-utm-traffic-log:1: true) (seed: 1782936689) (masks: 0xFFFFFFFF)
generating a system event message with level - warning
generating authentication event messages
1: generating an infected virus message with level - warning
1: generating a blocked virus message with level - warning
1: generating a URL block message with level - warning
1: generating a DLP message with level - warning
1: generating an IPS log message
1: generating an botnet log message
1: generating an anomaly log message
1: generating an application control IM message with level - information
1: generating an IPv6 application control IM message with level - information
1: generating deep application control logs with level - information
1: generating an antispam message with level - notification
1: generating a URL block message with level - warning
1: generating an ssh-command pass log with level - notification
1: generating an ssh-channel block with level - warning
1: generating an ssl-cert_blocklisted log with level - warning
1: generating FortiSwitch logs
1: generating a File Filter log with level - warning
1: generating a icap log with level - warning
1: generating a sctp filter log with level - warning
1: generating a virtual ot patch log with level - warning
1: generating a CASB monitor log with level - information
```

6. These logs can now be found in the Wazuh alerts index pattern under the decoder.name = fortigate-firewall-v6

![image.png](/Images/image%208.png)

# Creating Dashboard

1. Go to discover and select Dashboards, create new and then add the filter for only fortigate.

![image.png](/Images/Screenshot%202026-07-02%20123859.png)

2. Pick any type of visualization, I chose area and then configure the bucket by clicking add bucket. Confirm your fortigate logs in discover to see what specific fields it is using so that you can put it in the field of the dashboard.

![image.png](/Images/Screenshot%202026-07-02%20124557.png)

3. Save your visualization with short descriptive title and it will be added as a single panel to the overall dashboard where you can add more panels.

![image.png](/Images/Screenshot%202026-07-02%20124836.png)

4. Here’s a cluster of panels that I created to showcase the overall Dashboard for admins and SOC to use.

![image.png](/Images/Screenshot%202026-07-02%20125625.png)

# System Architecture

#### Minimum Hardware Requirements (as per Wazuh documentation):

- 4 vCPUs
- 8 GB Ram
- 50 GB Storage

![Wazuh with Fortigate.png](/Images/Wazuh_with_Fortigate.png)
