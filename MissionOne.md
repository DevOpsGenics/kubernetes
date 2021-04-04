# Environment Preparation (Centos 7) 

Prepare your k8s cluster infrastructure.

## Prerequisite

- Install VirtualBox [Download](https://www.virtualbox.org/wiki/Downloads)
- Download [CentOS-7-x86_64-Everything-2009.iso](http://mirrors.mit.edu/centos/7.9.2009/isos/x86_64/CentOS-7-x86_64-Everything-2009.iso)


## Prepare Master Node

1. Open VirtualBox → New → k8s-master.
   - Type: Linux, RedHat (64-bit)
   - CPU: 2 Cores
   - Memory: 4 GB
   - Disk: 30 GB

2. From File → HostNetwork Manager → Create → IPv4 address (192.168.164.1/24). This step will create a new NIC.
3. Right click “setting” on the box → Network → Adapter 2 → Enable Network Adapter & attached to “Host-only Adapter” and select the mew NIC you just created.
4. Go to storage → load the Centos ISO we just download 
5. Start the VM
6. Once it’s launch → Hit “Install Centos 7” → setup root user account
7. Login as root and run the following commands to update both NICs

  ```
  vi /etc/sysconfig/network-scripts/ifcfg-enp0s3

  # Update on boot setting to yes
  ONBOOT=yes
  ```


  ```
  vi /etc/sysconfig/network-scripts/ifcfg-enp0s8

  # Update on boot setting to yes and assinged a static IP address 
  BOOTPROTO=static
  ONBOOT=yes
  IPADDR=192.168.164.10
  PREFIX=24
  ```


8. Run the following command to set hostname to k8s-master

  ```
  hostnamectl set-hostname k8s-master
  ```

9. Install tools you may need (optional)

  ```
  yum install nano wget curl net-tools
  ```
10. Restart Network services
  ```
  systemctl restart network
  ```
## Prepare Worker Nodes

1. Full clone of master by shuting down the master node → Right click on it → "Clone" 
2. Update IP address on those two nodes respectively to the following:

	k8s-worker-1: 

    ```
    vi /etc/sysconfig/network-scripts/ifcfg-enp0s8
    
    # Update IP address from .10 to .11 since we did a full clone of master
    IPADDR=192.168.164.11
    ```
    
	k8s-worker-2:

    ```
    vi /etc/sysconfig/network-scripts/ifcfg-enp0s8
    
    # Update IP address from .10 to .12 since we did a full clone of master
    IPADDR=192.168.164.12
    ```

3. Update hostname on those two nodes respectively to the following:

	k8s-worker-1: 

    ```hostnamectl set-hostname k8s-worker-1```

	k8s-worker-2:

    ```hostnamectl set-hostname k8s-worker-2```

4. Reboot both worker nodes
