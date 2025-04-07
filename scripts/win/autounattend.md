# Windows Autounattend Scripts Technical Reference

This document provides technical details about the Windows autounattend scripts used for automated Windows installations. These scripts are XML-based answer files that control the Windows installation process.

## Script Specifications

### autounattend_1.xml
Core automation script with the following XML components:

- `<OOBE>` section:
  - Disables all user interaction prompts
  - Configures regional settings
  - Sets up network configuration

- `<UserAccounts>` section:
  - Creates local administrator account "admin"
  - Sets empty password configuration
  - Configures account permissions

- `<RunSynchronous>` section:
  - Custom commands to bypass hardware checks
  - Disables TPM requirements
  - Disables Secure Boot validation
  - Modifies registry keys for hardware compatibility

- `<DiskConfiguration>` section:
  - Currently commented out
  - No disk partitioning performed
  - Preserves existing disk layout

- `<ImageInstall>` section:
  - Currently commented out
  - No automatic image selection
  - Manual image selection required

### autounattend_2.xml
Extended version of `autounattend_1.xml` with additional features:

- `<DiskConfiguration>` section:
  - Active disk partitioning
  - Creates GPT partition scheme
  - Formats partitions with appropriate file systems
  - WARNING: Will erase existing disk data

- `<ImageInstall>` section:
  - Automatic Windows image selection
  - Configures installation drive
  - Sets up partition layout

## XML Structure Details

Both scripts use the following key XML elements:

```xml
<?xml version="1.0" encoding="utf-8"?>
<unattend xmlns="urn:schemas-microsoft-com:unattend">
    <settings pass="windowsPE">
        <!-- Windows PE configuration -->
    </settings>
    <settings pass="offlineServicing">
        <!-- Offline servicing configuration -->
    </settings>
    <settings pass="generalize">
        <!-- Generalization configuration -->
    </settings>
    <settings pass="specialize">
        <!-- Specialization configuration -->
    </settings>
    <settings pass="oobeSystem">
        <!-- OOBE configuration -->
    </settings>
</unattend>
```

## Registry Modifications

The scripts modify these key registry locations:

- `HKLM\SYSTEM\CurrentControlSet\Control\SecureBoot\State`
  - Disables Secure Boot requirements

- `HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard`
  - Modifies TPM and virtualization settings

- `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\OOBE`
  - Configures OOBE behavior

## Implementation Notes

1. **Disk Operations**
   - `autounattend_1.xml`: Safe for existing systems
   - `autounattend_2.xml`: Destructive operations, backup required

2. **Image Selection**
   - `autounattend_1.xml`: Manual selection in Windows Setup
   - `autounattend_2.xml`: Automatic selection based on XML configuration

3. **Hardware Compatibility**
   - Both scripts use `RunSynchronous` commands to modify hardware requirements
   - Registry modifications bypass standard Windows 11 requirements

## Development Resources

For script modification:

1. **XML Schema Reference**
   - [Microsoft Components Reference](https://learn.microsoft.com/en-us/windows-hardware/customize/desktop/unattend/components-b-unattend)
   - [Windows Setup Automation Overview](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/windows-setup-automation-overview?view=windows-11)

2. **Generation Tools**
   - [Windows Answer File Generator](http://www.windowsafg.com)
     - XML-based configuration
     - Direct registry modifications
   - [schneegans.de Unattend Generator](https://schneegans.de/windows/unattend-generator)
     - PowerShell integration
     - Advanced customization options

3. **Product Keys**
   - [KMS Client Activation Keys](https://learn.microsoft.com/en-us/windows-server/get-started/kms-client-activation-keys)
   - Used in `<ProductKey>` section of XML

