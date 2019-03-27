
<!---
Copyright IBM Corp. 2018, 2018
--->
# Attention
This project is __use by myself to learn__ the process with **Github, IBM Cloud Automation Manager Templates, Atom (editor) and terraform**

This is a change in code...

# AIM / goal
The aim of this project is to create a cam-template to deploy a standard-installation of IBM Cloud Private in a vsphere environment with 1x Master-, 1x Proxy- and 3x Worker-Nodes. Most of the Variables should be prefilled, to make the process of deployment quicker.

# IBM Cloud Private Installer

This template will use terraform to provision 5 VMs in a vSphere environment.

This template will install and configure the IBM Cloud Private in an standard-topology.

The components of a IBM Cloud Private deployment include:

## VM and ICP-Roles

* VM1: Boot and Master-Node + NFS-Server for this ICP-Cluster
* VM2: Proxy-Node
* VM3: Worker-Node1
* VM4: Worker-Node2
* VM5: Worker-Node3

## Enabled/Disabled Services
* Management = disabled
* Vulnerability-Advisor (VA) = disabled
* Gluster-FS = disabled
*

## vSphere-VM-template
I created a ubuntu 16.04.6 LTS-VM and made some customizations like:
* NFS-Server/Client
* Socat
* extend root-filesystem to 500GB (this is thin-provisioned within VMware)
* Preinstalled IBM-Docker-18.03.1
* prepared for ssh-key-authorization

then I converted this VM into a template with these specs:
* 16 vCPU
* 32GB RAM
* single Disk with 500GB (thin)
* 1x vNIC

## IBM Cloud Private Version

* 3.1.2

<https://github.com/Patthecat249/icp-standard-deployment>

## System Requirements

### Hardware requirements

This template will setup the following hardware minimum requirements:

| VM | CPU Cores | Memory (mb) | Disk 1 |
|:------:|:-------------:|:----:|:-----:|
| VM1    | 16 | 32768 | 500 |
| VM2    | 16 | 32768 | 500 |
| VM3    | 16 | 32768 | 500 |
| VM4    | 16 | 32768 | 500 |
| VM5    | 16 | 32768 | 500 |

***Notes***


### Supported operating systems and platforms

The following operating systems and platforms are supported.

***Ubuntu 16.04 LTS***

- VMware Tools must be enabled in the image for VMWare template.
- Ubuntu Repos with correct configuration must be enabled in the images.
- Sudo User and password must exist and be allowed for use.
- Firewall (via iptables) must be disabled.
- SELinux must be disabled.
- The system umask value must be set to 0022.

### Network Requirements

The following network information is required:
Based on the Standard setup:

- 5x IP Addresses are needed
- Subnetmask in CIDR (eg. 192.168.1.1/24)
- Network Gateway
- Interface Name

## Template Variables

The following tables list the template variables.

### Cloud Input Variables

| Name | Description | Type | Default |
|------|-------------|:----:|:-----:|
| vsphere_datacenter |vSphere DataCenter Name| string |  |
| vsphere_resource_pool | vSphere Resource Pool | string |  |
| vm_network_interface_label | vSphere Port Group Name | string | `VM Network` |
| vm_folder | vSphere Folder Name | string |  |

### IBM Cloud Private Template Settings

| Name | Description | Type | Default |
|------|-------------|:----:|:-----:|
| vm_dns_servers | IBM Cloud Private DNS Servers | list | `<list>` |
| vm_dns_suffixes | IBM Cloud Private DNS Suffixes | list | `<list>` |
| vm_domain | IBM Cloud Private Domain Name | string | `ibm.com` |
| vm_os_user | Virtual Machine  Template User Name | string | `root` |
| vm_os_password | Virtual Machine Template User Password | string |  |
| vm_template | Virtual Machine Template Name | string |  |
| vm_disk1_datastore | Virtual Machine Datastore Name - Disk 1 | string |  |


### IBM Cloud Private Multi-Node Settings

| Name | Description | Type | Default |
|------|-------------|:----:|:-----:|
| enable_kibana | Enable IBM Cloud Private Kibana | string | `true` |
| enable_metering | Enable IBM Cloud Private Metering | string | `true` |
| worker_enable_glusterFS |  Enable IBM Cloud Private GlusterFS on worker Nodes| string | `true` |
| icp_cluster_name | IBM Cloud Private Cluster Name | string | `icpclustervip` |
| cluster_vip | IBM Cloud Private Cluster VIP | string |  |
| cluster_vip_iface | IBM Cloud Private Cluster Network Interface | string | `ens160` |
| proxy_vip | IBM Cloud Private Proxy VIP | string |  |
| proxy_vip_iface | IBM Cloud Private Proxy Network Interface | string | `ens160` |
| icp_admin_user |  IBM Cloud Private Admin Username| string | `admin` |
| icp_admin_password | IBM Cloud Private Admin Password | string | `admin` |

### IBM Cloud Private Download Settings

| Name | Description | Type | Default |
|------|-------------|:----:|:-----:|
| download_user | Repository User Name (Optional) | string |  |
| download_user_password | Repository User Password (Optional) | string |  |
| docker_binary_url | IBM Cloud Private Docker Download Location (http/https/ftp/file) | string |  |
| icp_binary_url |  IBM Cloud Private Download Location (http/https/ftp/file)| string | |
| icp_private_ssh_key | IBM Cloud Private - Private SSH Key | string | `` |
| icp_public_ssh_key | IBM Cloud Private - Public SSH Key | string | `` |
| icp_version | IBM Cloud Private Version | string | `3.1.2` |
| kub_version | Kubernetes Version| string | `1.11.0` |


### Master Nodes Input Settings

| Name | Description | Type | Default |
|------|-------------|:----:|:-----:|
| master_prefix_name | Master Node Hostname Prefix | string | `ICPMaster` |
| master_memory | Master Node Memory Allocation (mb) | string | `32768` |
| master_vcpu | Master Node vCPU Allocation | string | `16` |
| master_vm_disk1_size | Master Node Disk Size (GB)  | string | `500` |
| master_vm_ipv4_address | Master Nodes IP Address's | list | `<list>` |
| master_vm_ipv4_gateway | Master Node IP Gateway | string |  |
| master_vm_ipv4_prefix_length | Master Node IP Netmask (CIDR) | string | `26` |
| master_nfs_folders | Master Node NFS Directories | list | `<list>` |

### Proxy Nodes Input Settings

| Name | Description | Type | Default |
|------|-------------|:----:|:-----:|
| proxy_prefix_name | Proxy Node Hostname Prefix | string | `ICPProxy` |
| proxy_memory | Proxy Node Memory Allocation (mb) | string | `32768` |
| proxy_vcpu | Proxy Node vCPU Allocation | string | `16` |
| proxy_vm_disk1_size | Proxy Node Disk Size (GB) | string | `500` |
| proxy_vm_ipv4_address | Proxy Nodes IP Address's | list | `<list>` |
| proxy_vm_ipv4_gateway | Proxy Node IP Gateway | string |  |
| proxy_vm_ipv4_prefix_length | Proxy Node IP Netmask (CIDR)  | string | `26` |

### Worker Nodes Input Settings

| Name | Description | Type | Default |
|------|-------------|:----:|:-----:|
| worker_prefix_name | Worker Node Hostname Prefix | string | `ICPWorker` |
| worker_memory | Worker Node Memory Allocation (mb) | string | `32768` |
| worker_vcpu | Worker Node vCPU Allocation | string | `16` |
| worker_vm_disk1_size | Worker Node Disk Size (GB) | string | `500` |
| worker_vm_ipv4_address | Worker Nodes IP Address's | list | `<list>` |
| worker_vm_ipv4_gateway |Worker Node IP Gateway  | string |  |
| worker_vm_ipv4_prefix_length | Worker Node IP Netmask (CIDR) | string | `26` |

### NFS Server Node Input Settings

| Name | Description | Type | Default |
|------|-------------|:----:|:-----:|
| nfs_server_prefix_name | NFS Server Node Hostname Prefix | string | `ICPNFS` |
| nfs_server_memory | NFS Server Node Memory Allocation (mb) | string | `32768` |
| nfs_server_vcpu | NFS Server Node vCPU Allocation | string | `16` |
| nfs_server_vm_disk1_size | NFS Server Node Disk Size (GB)  | string | `500` |
| nfs_server_vm_ipv4_address | NFS Server Nodes IP Address | list | `<list>` |
| nfs_server_vm_ipv4_gateway | NFS Server Node IP Gateway | string |  |
| nfs_server_vm_ipv4_prefix_length | NFS Server Node IP Netmask (CIDR) | string | `26` |
| nfs_server_folder | NFS Server Node Server Folder | string | `/export` |
| nfs_server_mount_point | NFS Server Node Mount Point | string | `/mnt/nfs` |

## Template Output Variables

| Name | Description |
|------|-------------|
| ibm_cloud_private_admin_url | IBM Cloud Private Cluster URL |
| ibm_cloud_private_admin_user | IBM Cloud Private Admin Username |
| ibm_cloud_private_admin_password | IBM Cloud Private Admin Password |
