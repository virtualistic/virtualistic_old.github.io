---
title: "How to Reinstall Single ESXi Host Running Vcenter"
date: 2023-04-10T16:47:34+02:00
draft: true
tags:
    - ESXi 7
    - reinstall
    - vCenter
---

Some quick notes on how I reinstalled my homeserver from 6.7 to 7 without losing any VM configuration

<!--more-->

## The Challenge

I was still running ESXi 6.7 from a usb-stick and I knew that going forward VMWare is no longer going to support this type of setup so how am I going to reinstall the host that also hosts my vCenter without losing all my VM configuration?  
<ol>
  <li>Shutdown all VMs except vCenter</li>
  <li>Make note if you have any VMs with manual mac addresses.If so collect that information</li>
  <li>Run the following one-liner from Powercli to save the location info of the VM's to a csv file</li>
  <li>```powershell
  Get-VM | Select Name, @{N="VMX";E={$_.Extensiondata.Summary.Config.VmPathName}} | Export-CSV -Path "c:\temp\vm-info.csv" -NoTypeInformation
    ```</li>
  <li>IMPORTANT: in vCenter disconnect the host (right-click, disconnect)</li>
  <li>Shutdown vCenter</li>
  <li>Shutdown the host </li>
  <li>Remove any PCIE passthrough & zwave USB devices from the VMs</li>
  <li>Remove the ESXi 6.7 USB thumbdrive</li>
  <li>Install ESXi 7 to a local SSD using the same hostname & IP</li>
  <li>After installation run this script to re-register all VMs again</li>
  <li>```powershell
  connect-viserver "esxi7-host.local"
  $csv = Import-Csv -Path 'C:\temp1\vm-info.csv' 
  foreach ($row in $csv){
  write-host "registering" $f.name -ForegroundColor Green
  $vm = $row.name
  $vmxpath = $row.VMX
  Write-Host "VM name is:" $vm
  Write-Host "vmx path name is:" $vmxpath
  New-VM -Name $vm -VMFilePath "$vmxpath"
  write-host ""
  }```</li>
  <li>select any needed devices for passthrough (a HBA in my case) </li>
  <li>attach the PCIe device back to the VM</li>
  <li>attach the zwave usb device to VM</li>
  <li>Start vCenter, login, select the host and click: "Connect". Accept the new certificate</li>
</ol>
Done! 
