# Connecting Industrial OPC-UA PLCs to azure iot hub using Moxa UC-8112A IIoT Gateway running Microsoft OPCpublisher Container (Azure IoT Edge + ThingsPro Edge)

## This document lists the following steps:
   - Basic Device Setup
   - Cloud Setup
   - Device Setup Provisioning Tool
   - Data Setup
   - Monitor OPC-UA D2C Telemetry data on Azure IoT Hub

# Basic Device Setup

## Default credential and IP address
  - username/password:  moxa/moxa
  - IP LAN1 192.168.3.127 
  - IP LAN2 192.168.4.127

## Set to Default

Reset to factory default

If the unit has been installed prior, we should reset it back to default before installing ThingsPro Edge. The commands listed below is for `armhf` and UC-8100A series gateway

```sh
mx-set-def
```

> Note: Make sure to have a console cable conected to the device or login via ssh

Remove docker folder
```sh
rm -rf /overlayfs/docker /overlayfs/working/docker
```

> Note: This will wipe out all the data on the device!

## Configure Network
```sh
dhclient eth0
```
> Note: Make sure there is a dhcp server on LAN1

## Download and Install ThingsPro Edge V2.0.0 on UC-8112A with Internet 
- armhf:
    ```sh
    wget https://thingspro.blob.core.windows.net/software/edge/V2.0.0/update_2.0.0-1511-uc-8112a-me-iotedge_armhf.deb
    dpkg -i ./update_2.0.0-1511-uc-8112a-me-iotedge_armhf.deb
    ```
## Download and Install ThingsPro Edge V2.0.0 on UC-8112A without Internet
Click on the url below or copy url it on your web browser which starts downloading thingspro software, it takes 10~12 minutes depending the network speed of your internet connection. 
 ```sh
https://thingspro.blob.core.windows.net/software/edge/V2.0.0/update_2.0.0-1511-uc-8112a-me-iotedge_armhf.deb
 ```
 

After the download is completed, copy .deb package file on the UC gateway via FTP

> Hint: On Windows10 use WinSCP tool. It is free tool available on the url below. 
 ```sh
https://winscp.net/eng/download.php
 ```
 - armhf:
     ```sh
     dpkg -i ./update_2.0.0-1511-uc-8112a-me-iotedge_armhf.deb
     ```

## Update the Installation Process
```sh
journalctl -u update -f
```

## Track Installation Progress
   ![](Media/track-installation-progress.png)

> Note: The entire process will take about ~11 minutes depends on the hardware/model. When it shows "Stopped MOXA ThingsPro Updater", you can reboot the device by command listed below. The SSH service will be disabled after TPE is installed. 

## Reboot Device
```sh
sudo reboot
```

# Cloud Setup

## Setup a Device Provisioning Service (DPS) using Azure portal

Please use the url below to set up a DPS on azure portal. The url includes three steps. 
 1.  Create an Iot hub instance
 2.  Create a new IoT hub DPS
 3.  Link Iot hub and your DPS

```sh
https://docs.microsoft.com/en-us/azure/iot-dps/quick-setup-auto-provision
```

## Setup a Auto IotEdge Deploment using Azure portal

