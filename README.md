# USB Booter: Automated Windows Installation Guide

This repository provides a comprehensive guide for setting up a bootable USB drive using Ventoy, enabling automated Windows installations without manual intervention.

## Ventoy Setup

1. **Download Ventoy**
   - Get the latest version from [Ventoy's official website](https://www.ventoy.net/en/index.html)

2. **Configure Ventoy Web UI**
   - Follow the [Linux WebUI setup instructions](https://www.ventoy.net/en/doc_linux_webui.html)
   - Switch partition style from MBR to GPT:
     - Navigate to Option -> Partition Style -> GPT

3. **USB Drive Structure**
   The USB drive will be partitioned into two sections:
   - `Ventoy` partition (exFAT): Stores ISOs and scripts
   - `VTOYEFI` partition (FAT): System files

4. **Organize Your Files**
   Create the following directory structure in the `Ventoy` partition:
   ```
   Ventoy/
   ├── isos/
   │   ├── win/    # Windows ISO images
   │   └── lin/    # Linux ISO images
   └── scripts/
       └── win/    # Windows unattended install scripts
   ```

   Note: The `ventoy/` directory and `ventoy.json` configuration file are automatically created by VentoyPlugson.

## Download Windows ISOs

Visit [Microsoft's official Windows 11 download page](https://www.microsoft.com/en-us/software-download/windows11) for a Win 11 ISO.  Save the file (e.g., `Win11_24H2_English_x64.iso`) to your `isos/win/` directory.

## Ventoy Configuration

1. **Use VentoyPlugson**
   - Follow the [plugin configuration guide](https://www.ventoy.net/en/plugin_plugson.html)
   - This will create the necessary `ventoy/` directory in your Ventoy partition

2. **Configure Auto Installation**
   - Launch VentoyPlugson: `sudo bash VentoyPlugson.sh /dev/sdb`
   - Select "Auto Install Plugin" from the left menu
   - Click "Add" in the "auto_install" tab
   - Choose "[parent] Set the same auto install template for all the files under a directory"
   - Configure:
     - Directory Path: Full path to your `isos/win` directory
     - Template: Select your preferred `autounattend.xml` script
     - Add additional scripts using the "Add" button in the "Operation" column

   Example configuration (`ventoy/ventoy.json`):
   ```json
   {
       "auto_install":[
           {
               "parent": "/isos/win",
               "template":[
                   "/scripts/autounattend_1.xml",
                   "/scripts/autounattend_2.xml"
               ]
           }
       ]
   }
   ```

## BIOS Configuration

Some systems require specific BIOS settings for successful USB booting. Below are manufacturer-specific settings:

### Dell Systems

1. **Storage Configuration**
   - Navigate to Storage -> SATA/NVMe Operation
   - Change from RAID ON to AHCI/NVMe
   - Note: RAID drivers may be needed; see [TechNewsToday guide](https://www.technewstoday.com/raid-drivers/)

2. **Security Settings**
   - Disable TPM 2.0 Security
   - Disable Intel Total Memory Encryption
   - Permanently Disable Absolute 