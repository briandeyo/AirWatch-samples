# Workspace ONE Sensors

## Overview
- **Authors**:Bhavya Bandi, Varun Murthy, Josue Negron, Brooks Peppin, Aaron Black, Mike Nelson
- **Email**: bbandi@vmware.com, vmurthy@vmware.com, jnegron@vmware.com, bpeppin@vmware.com, aaronb@vmware.com, miken@vmware.com
- **Date Created**: 11/14/2018
- **Supported Platforms**: Workspace ONE 1811
- **Tested on**: Windows 10 Pro/Enterprise 1803+

## Purpose
These Workspace ONE Sensor samples contain PowerShell command lines or scripts that can be used in a **Provisioning > Custom Attributes > Sensors** payload to report back information about the Windows 10 device back to Workspace ONE.

## Description 
There are Sensor samples, templates, and a script `import_sensor_samples.ps1` to populate your environment with all of the samples.    

## Required Changes/Updates
You will want to leverage the `template_`  samples and modify any of the data, or leverage the existing samples. You can also leverage the `import_sensor_samples.ps1` script to upload the samples to your environment. Only the templates and the Sensor Importer require changes. Samples work as is, but can also be modified for your needs. 

### WMI Query Template
    $wmi=(Get-WmiObject WMI_Class_Name)
    write-output $wmi.Attribute_Name

### Registry Value Template 
    $reg=Get-ItemProperty "HKLM:\Key Folder\Key Name"
    write-output $reg.ValueName

## Workspace ONE Sensors Importer

### Synopsis 
This Powershell script allows you to automatically import PowerShell scripts (Sensor Samples) as Workspace ONE Sensors in the Workspace ONE UEM Console. MUST RUN AS ADMIN

### Description 
Place this PowerShell script in the same directory of all of your samples (.ps1 files) or use the `-SensorsDirectory` parameter to specify your directory. This script when run will parse the PowerShell sample scripts, check if they already exist, then upload to Workspace ONE UEM via the REST API.

### Examples 

- **Basic**: this command shows all required fields and will scan the default directory and upload the samples to Workspace ONE via the REST API using the credentials provided. 

    	.\import_sensor_samples.ps1 `
        -WorkspaceONEServer "https://as###.awmdm.com" `
        -WorkspaceONEAdmin "administrator" `
        -WorkspaceONEAdminPW "P@ssw0rd" `
        -WorkspaceONEAPIKey "YeJtOTx/v2EpXPIEEhFo1GfAWVCfiF6TzTMKAqhTWHc=" `
        -OrganizationGroupName "techzone"

- **Custom Directory**: using the `-SensorsDirectory` parameter tells the script where your samples exist. The directory provided must have .ps1 files which you want uploaded as Sensors. 

    	.\import_sensor_samples.ps1 `
        -WorkspaceONEServer "https://as###.awmdm.com" `
        -WorkspaceONEAdmin "administrator" `
        -WorkspaceONEAdminPW "P@ssw0rd" `
        -WorkspaceONEAPIKey "YeJtOTx/v2EpXPIEEhFo1GfAWVCfiF6TzTMKAqhTWHc=" `
        -OrganizationGroupName "techzone" `
		-SensorsDirectory "C:\Users\G.P.Burdell\Downloads\Sensors"

- **Assign to Smart Group**: using the `-SmartGroupID` parameter will assign ALL Sensors which were uploaded and that already exist to that chosen Smart Group. *Existing Smart Group memberships will be overwritten!* This command is used best in a test environment to quickly test Sensors before moving Sensors to production. Obtain the Smart Group ID via API or by hovering over the Smart Group name in the console and looking at the ID at the end of the URL. 

    	.\import_sensor_samples.ps1 `
        -WorkspaceONEServer "https://as###.awmdm.com" `
        -WorkspaceONEAdmin "administrator" `
        -WorkspaceONEAdminPW "P@ssw0rd" `
        -WorkspaceONEAPIKey "YeJtOTx/v2EpXPIEEhFo1GfAWVCfiF6TzTMKAqhTWHc=" `
        -OrganizationGroupName "techzone" `
		-SmartGroupID 14

### Parameters 
**WorkspaceONEServer**: Server URL for the Workspace ONE UEM API Server e.g. https://as###.awmdm.com without the ending /API. Navigate to **All Settings -> System -> Advanced -> Sites URLs**.
![](https://i.imgur.com/et8Tm5h.png)


**WorkspaceONEAdmin**: An Workspace ONE UEM admin account in the tenant that is being queried.  This admin must have the API role at a minimum.

**WorkspaceONEAdminPW**: The password that is used by the admin specified in the admin parameter

**WorkspaceONEAPIKey**: This is the REST API key that is generated in the Workspace ONE UEM Console.  You locate this key at **All Settings -> System -> Advanced -> API -> REST API**,
and you will find the key in the API Key field.  If it is not there you may need override the settings and Enable API Access. 
![](https://i.imgur.com/tQqsyzw.png)

**OrganizationGroupName**: The Group ID of the Organization Group. You can find this by hovering over your Organization's Name in the console.
![](https://i.imgur.com/lWjWBsF.png)

**SensorsDirectory**: (OPTIONAL) The directory your .ps1 sensors samples are located, default location is the current PowerShell directory of this script. 

**SmartGroupID**: (OPTIONAL) If provided, all sensors in your environment will be assigned to this Smart Group. Existing assignments will be overwritten. Navigate to **Groups & Settings > Groups > Assignment Groups**. Hover over the Smart Group, then look for the number at the end of the URL, this is your Smart Group ID. 
![](https://i.imgur.com/IjvkoGC.png)


## Change Log
- 12/6/2018 - Added more details on how to use import_sensor_samples.ps1
- 12/5/2018 - Added import_sensor_samples.ps1 and updated system_type, system_status, system_date, templates, system_family
- 12/3/2018 - added system_pc_type, system_status, system_family, system_type, system_thermal_state, system_wakeup_type, system_model, system_manufacturer, system_hypervisor_present, system_dns_hostname, system_username, system_name, system_domain_role, system_domain_membership, system_domain_name
- 11/30/2018 - added os_verison, os_build_version, os_build_number, os_architecture, os_edition, system_timezone, bitlocker_encryption_method, horizon_broker_url, horizon_protocol, template_get_wmi_object, template_get_registry_value
- 11/21/2018 - updated bios_secure_boot and bios_serial_number
- 11/15/2018 - changed echo to write-output in all samples
- 11/14/2018 - Uploaded README file

## Additional Resources
Coming Soon on [techzone.vmware.com](http://techzone.vmware.com)!
