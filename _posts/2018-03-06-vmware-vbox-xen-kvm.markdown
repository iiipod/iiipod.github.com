---
title: 虚拟化随笔
layout: post
author:
  name: macint0sh
---
## VirtualBox 修改硬盘型号/序列号
	IDE:   
	VBoxManage setextradata xp "VBoxInternal/Devices/piix3ide/0/Config/PrimaryMaster/ModelNumber" "string:VMware Virtual IDE"
	VBoxManage setextradata xp "VBoxInternal/Devices/piix3ide/0/Config/PrimaryMaster/SerialNumber" "string:00000000000000000001" 
           
	SATA:  
	VBoxManage setextradata xp "VBoxInternal/Devices/ahci/0/Config/Port0/ModelNumber" "string:VMware Virtual IDE"
	VBoxManage setextradata xp "VBoxInternal/Devices/ahci/0/Config/Port0/SerialNumber" "string:00000000000000000001"

## VirtualBox 修改 BIOS
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiBIOSVendor "coreboot"
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiBIOSVersion "4.0"
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiBIOSReleaseDate "09/04/2014"
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiSystemVendor "Hewlett-Packard"
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiSystemProduct "Falco"
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiSystemVersion "SVersion"
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiSystemSerial "SSerial"
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiSystemFamily "SFamily"
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiSystemUuid "9852bf98-b83c-49db-a8de-182c42c7226b"
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiBoardVendor       "GA-945GCM-S2L"
	VBoxManage setextradata "xp"  VBoxInternal/Devices/pcbios/0/Config/DmiBoardProduct      "HP"
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiBoardVersion      "2.22"
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiBoardSerial       ""
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiBoardAssetTag     "bTag"
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiBoardLocInChass   "bLocation"
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiBoardBoardType    10
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiChassisVendor     "CVendor"
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiChassisType       3
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiChassisVersion    "CVersion"
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiChassisSerial     "CSerial"
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiChassisAssetTag   "CTag"
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiProcManufacturer  "GenuineIntel"
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiProcVersion       "Pentium(R) III"
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiOEMVBoxVer        "vbox5.1"
	VBoxManage setextradata "xp" VBoxInternal/Devices/pcbios/0/Config/DmiOEMVBoxRev        "vbox5.1"

## img 转vdi     
	$ VBoxManage convertfromraw --format VDI lede.img lede.vdi    

## Vmware 创建固定大小的硬盘          
	.\vmware-vdiskmanager.exe -c -s 60M -a ide -t 0 60m.vmdk    



