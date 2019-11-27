# Specialized Image in Shared Image Gallery demo with azure-chroot

## Summary of Lab
In this exercise you will create a virtual machine using the shared image that you create. Initially, we will start by creating a managed disk. Then, we will create a snapshot from this managed disk. Finally, shared image gallery and create shared image into this gallery. For verification, we will create a virtual machine and a VMSS using the shared image to review that the credential has been kept intact, and has no issue with name resolution.  

## Pre-Requisites
This Lab Exercise will assume that:
    You have added your Resource Group ID, Location, Source VM Name, Image Name, Shared Gallery Name, Shared Image Name, Shared Image Version and VM Name to the vars.yml file prior to executing this lab.

## Updated modules
Following files have been updated to support azure snapshot from managed disk.
- /modules/library/azure_rm_snapshot.py
- /module_utils/azure_rm_common_ext.py

## Procedure
1. Create a VM from terraform

Following git is private. please contact for access.
```sh
https://github.com/bedro96/terraform/blob/master/ubuntuvmpacker/ubuntusing_packer_vm.tf
```

2. SSH into the VM and git clone

```sh
git clone https://github.com/bedro96/chroot.git
sudo apt install unzip
unzip packer_1.4.5_linux_amd64.zip
sudo cp packer /usr/bin
```
vi init_chroot.sh and update ARM_SOURCE_DISK_RESOURCE_ID
3. Update init_chroot.sh, sudo su, source the init_chroot.sh

```sh
sudo su
source init_chroot.sh
packer build exampleNetPlanSan_Daniel_Sol.json
```
4. Locate PackerTemp disk and take a snapshot

Create target resource group with vnet, named ansiblerg.
```sh
ansible-playbook 00c-prerequisites.yml
```
Locate PackerTemp disk and identify resource id
```sh
ansible-playbook 01c-create-snapshot.yml 
```
5. Create a shared image gallery

```sh
ansible-playbook 02-create-shared-image-gallery.yml
```
6. Create a shared image gallery image and image version

```sh
ansible-playbook 03b-create-shared-image-specialized.yml
```
7. Create a VM from specialized image and check if you can ssh into the machine

```sh
ansible-playbook 04b-create-vm-using-specialized-image.yml
```
8. Create a VMSS from specialized image and check if you can ssh into the machine

```sh
ansible-playbook 05b-create-vmss-using-specialized-image.yml
```
