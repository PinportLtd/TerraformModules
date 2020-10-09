# My new created Terraform module

This module will create a Virtual Machine or multiple Virutal machines. You will be able to set a key vault for the username and password of the virtual machines, or if you would rather just store the password in the key vault, you can do that instead.
It will use a username and password which has been stored in a key vault. or you can specifiy them yourself. Or you can store the password only in the key vault and manually specify the admin username. You can import an existing key vault or create a new one on the fly.
It will autmatically setup the WinRM (Https) access to the Virtual Machine,
It will automatically setup the.)
It will automatically setup the system managed ID to access the key vault.
It will create an extra Data Disk (you will be able to increase the amount of data disk up to 9)
It will allow you to add extensions on.??
by default the server names will be randon names, this can be overwritten and you can use whatever names you want.


## Usage

Provide the sample code to use your module.

## Scenarios

If you would like to 

## Examples

Paste the links to your sample code in `examples` folder.

## Inputs
``` Terraform

// Required Variables

location            = "uksouth"
resourcegroupname   = "Deathrace"
admin_password      = "ASimplePassword"

name_counts = {
    "foo" = 2
 /* "bar" = 4 */
} 


// Optional Variables

// ##########################
// Start of General variables

taglist = {
    "Environment" = "Staging"
    "Created by"  = "Russell Maycock"
    "Company"     = "Pinport Ltd"
    "project"     = "DeathRace"
    "website"     = "www.realworldit.net"
}


// End of General variables
// ########################



// ##################
// Key vault Settings

key_vault = {
    key_vault = {
      key_vault_resource_group  = "kv502857KeyVault"
      key_vault_name            = "kv502857"
      key_vault_secret_password = "Default-Admin-VM-Password"
    }
}

// end of key vault variables
// ##########################


// ##########################
// Start of Network variables

network_subnet_Frontend_name = "Frontend"

// End of Network variables
// ########################


// #################################
// Start of Virtua Machine variables

enable_automatic_updates    = true
eviction_policy             = "Deallocate"
priority                    = "Regular"
vmsize                      = "Standard_B2ms"
timezone                    = "GMT Standard Time"

// Virtual Machine identity block variables

identity_type = "SystemAssigned"

// Virtual Machine identity block variables end

// Virtual Machine os_disk block variables

caching              = "ReadWrite"
storage_account_type = "Standard_LRS"

// Virtual machine os_disk block variable ends

// Virtual machine secret block variable starts

secrets = {
  secrets1 = {
    store = "My"
    url   = "https://kv502857.vault.azure.net/secrets/foomachines/f......3"
  } 
}

// Virtual machine secret block variable ends

// Virtual machine source_image_reference block variables

imagepublishername  = "MicrosoftWindowsServer"
imageoffer          = "WindowsServer"
imagesku            = "2019-Datacenter"
imageskuversion     = "latest"

// Virtual machine source_image_reference block variables End

// Virtual machine Win_rm block variables

enable_winrm = {
  "Https" = "https://kv502857.vault.azure.net/secrets/foomachines/f......3"
}

// Virtual machine Win_rm block variables end

// Virtual machine Boot Diags Block variables start

boot_diagnostics_storage_account_uri = {
   "uri" = "https://te.....w3re.blob.core.windows.net/?sv=2........4=https&sig=L.....D"
}

// Virtual machine Boot Diags Block variables end


// Virtual Machine Variables End
// #############################


// ###############################
// start of Managed Disk variables

disks_per_vm    = 1
disksize        = 32

// end of Managed Disk variables
// #############################


// ############################################
// Start of IaaSAntiMalware Extension variables

enableIaaSAntiMalwareExtension                      = true
IaaSAntimalwareExt_AntimalwareEnabled               = "true"
IaaSAntimalwareExt_RealtimeProtectionEnabled        = "true"
IaaSAntimalwareExt_ScheduledScanSettings_isEnabled  = "true"
IaaSAntimalwareExt_ScheduledScanSettings_day        = "1"
IaaSAntimalwareExt_ScheduledScanSettings_time       = "120"
IaaSAntimalwareExt_ScheduledScanSettings_scanType   = "Quick"
IaaSAntimalwareExt_Exclusions_Extensions            = ""
IaaSAntimalwareExt_Exclusions_Paths                 = ""
IaaSAntimalwareExt_Exclusions_Processes             = ""

// End of IaaSAntimalware Extension variables
// ##########################################

// #########################################
// Start of CustomScript Extension variables

enableCustomScriptextension         = true
CustomScriptExt_commandToExecute    = "powershell.exe -Command \"./chocolatey.ps1; exit 0;\""
CustomScriptExt_filesUris = [
    "https://gist.githubusercontent.com/mcasperson/c815ac880df481418ff2e199ea1d0a46/raw/5d4fc583b28ecb27807d8ba90ec5f636387b00a3/chocolatey.ps1"
  ]

// End of CustomScript Extensions variables
// ########################################

// ######################################
// Start of DSCScript Extension variables

enableDSCScriptextension                = true
DSCScriptExt_configurationUrlSasToken   = "?sv=......Q%3D"
DSCScriptExt_Url                        = "https://te.....w3re.blob.core.windows.net/dsccontainer/PowershellDSC.ps1.zip"
DSCScriptExt_Name                       = "PowershellDSC.ps1"
DSCScriptExt_Function                   = "PowershellDSC"
DSCScriptExt_dataCollection             = "enable"
DSCScriptExt_forcePullAndApply          = false

// End of DSCScript Extensions variables
// #####################################

// #######################################
// Start of DSCService Extension variables

enableDSCServiceextension                       = false
DSCServiceExt_RegistrationKeyPassword           = "gRHraf.......2igb5="
DSCServiceExt_RegistrationUrl                   = "https://7d.....a21.agentsvc.uks.azure-automation.net/accounts/6e3....5a31"
DSCServiceExt_NodeConfigurationName             = "PowerShellDSC.localhost"
DSCServiceExt_ConfigurationMode                 = "ApplyAndAutoCorrect"
DSCServiceExt_ConfigurationModeFrequencyMins    = 15
DSCServiceExt_RefreshFrequencyMins              = 30
DSCServiceExt_RebootNodeIfNeeded                = true
DSCServiceExt_ActionAfterReboot                 = "continueConfiguration"
DSCServiceExt_AllowModuleOverwrite              = true

// End of DSCService Extensions variables
// ######################################


```





## Outputs

List all output variables of your module.




``` terraform
// variables.tf file

// ##########################
// Start of General variables

// Required Variables

variable "location" {
  description = "The Azure location of the virtual machine."
}

variable "resourcegroupname" {
  description = "The Resource Group Name"
}

variable "admin_password" {
  description = "This is the Administrator's Password. Use If you are not storing the password in a key vault."
}




// Optional Variables

variable "taglist" {
  type        = map(string)
  description = "This is the tags for the Virtual Machine."
  default = {

  }
}

variable "name_counts" {
        type = map(number)
        description = "The name and number of machines"
        default     = {
            "foo" = 1
        // "bar" = 1
        }
}



// End of General variables
// ########################



// ##################
// Key vault Settings

variable "key_vault" {
  type = map(object({
      key_vault_resource_group  = string
      key_vault_name            = string
      key_vault_secret_password = string
  }))
    description = "Existing Key Vault Settings"
    default     = {
  
    }
}


// end of key vault variables
// ##########################


// ##########################
// Start of Network variables
variable "network_subnet_Frontend_name" {
    description = "Description"
    default     = "Frontend"
}

// End of Network variables
// ########################


// #################################
// Start of Virtua Machine variables
variable "admin_username" {
        description = "The name of the administrator"
        default     = "azureadmin"
}
variable "enable_automatic_updates" {
    description = "Enables auto updates on the VM"
    default     = true
}
variable "eviction_policy" {
    description = "Sets the evition policy if the VM is set to use azure Spot. The only available option is to deallocate at the moment"
    default     = "Deallocate"
}
variable "priority" {
    description = "Set to either Regulare or Spot. To use azure spot set to Spot"
    default     = "Regular"
}
variable "vmsize" {
    description = "The size of the VM"
    default     = "Standard_B2ms"
}
variable "timezone" {
    description = "The timezone for the VMs"
    default     ="GMT Standard Time"
}


// Virtual Machine identity block variables
variable "identity_type" {
    description = "The identity type can be set to systemassigned or userassigned"
    default     = "SystemAssigned"
}

// Virtual Machine identity block variables end

// Virtual Machine os_disk block variables

variable "caching" {
    description = "The caching of the OS Disk"
    default     = "ReadWrite"
}
variable "storage_account_type" {
    description = "The Storage Account type can be one of three options"
    default     = "Standard_LRS"
}

// Virtual machine os_disk block variable ends

// Virtual machine secret block variable starts
variable "secrets" {
  type = map(object({
    store = string
    url = string
  }))
    description = "The Secrets "
    default     = {
    
      }
    }

// Virtual machine secret block variable ends

// Virtual machine source_image_reference block variables
variable "imagepublishername" {
    description = "The Publishers name of the image e.g. MicrosoftWindowsServer"
    default     = "MicrosoftWindowsServer"
}
variable "imageoffer" {
    description = "The Office of the Image e.g. WindowsServer"
    default     = "WindowsServer"
}
variable "imagesku" {
    description = "The SKU of the Image e.g. 2019-DataCenter"
    default     = "2019-DataCenter"
}
variable "imageskuversion" {
    description = "The version of the SKU of the Image e.g. latest"
    default     = "latest"
}

// Virtual machine source_image_reference block variables End

// Virtual machine Win_rm block variables

variable "enable_winrm" {
    description = "Enables WinRM"
    default     = {
       
        }
    } 


// Virtual machine Win_rm block variables end

// Virtual machine Boot Diags Block variables start

variable "boot_diagnostics_storage_account_uri" {
        description = "Description"
        default     = {
         
        }
}

// Virtual machine Boot Diags Block variables end


// Virtual Machine Variables End
// #############################


// ###############################
// start of Managed Disk variables
variable "disks_per_vm" {
    description = "Number of disk per VM"
    default     = 0
}
variable "disksize" {
    description = "The size of the disk in GB. This will apply to all Data Disks"
    default     = 32
}

// end of Managed Disk variables
// #############################


// ############################################
// Start of IaaSAntiMalware Extension variables
variable "enableIaaSAntiMalwareExtension" {
    description = "Enable the IaaSAntiMalware Extension"
    default     = false
}
variable "IaaSAntimalwareExt_AntimalwareEnabled" {
    description = "Enable the Antimalware"
    default     = "true"
}
variable "IaaSAntimalwareExt_RealtimeProtectionEnabled" {
    description = "Enable the Antimalware Realtime Protection"
    default     = "true"
}
variable "IaaSAntimalwareExt_ScheduledScanSettings_isEnabled" {
    description = "Enable the Scheduled Scan"
    default     = "true"
}
variable "IaaSAntimalwareExt_ScheduledScanSettings_day" {
    description = "Set the Day settings"
    default     = "1"
}
variable "IaaSAntimalwareExt_ScheduledScanSettings_time" {
    description = "Set the Scan time"
    default     = "120"
}
variable "IaaSAntimalwareExt_ScheduledScanSettings_scanType" {
    description = "The the Scan Type"
    default     = "Quick"
}
variable "IaaSAntimalwareExt_Exclusions_Extensions" {
    description = "Description"
    default     = ""
}
variable "IaaSAntimalwareExt_Exclusions_Paths" {
    description = "Description"
    default     = ""
}
variable "IaaSAntimalwareExt_Exclusions_Processes" {
    description = "Description"
    default     = ""
}

// End of IaaSAntimalware Extension variables
// ##########################################

// #########################################
// Start of CustomScript Extension variables

variable "enableCustomScriptextension" {
    description = "Enable the Custom Script Extension"
    default     = false
}
variable "CustomScriptExt_commandToExecute" {
    description = "The Command to run e.g. powershell.exe -Command ./chocolatey.ps1; exit 0; ."
    default     = ""
}
variable "CustomScriptExt_filesUris" {
    description = "The location URL of the script to run e.g. https://gist.githubusercontent.com/mcasperson/c815ac880df481418ff2e199ea1d0a46/raw/5d4fc583b28ecb27807d8ba90ec5f636387b00a3/chocolatey.ps1"
    default     = [
    
  ]
} 

// End of CustomScript Extensions variables
// ########################################

// ######################################
// Start of DSCScript Extension variables

variable "enableDSCScriptextension" {
    description = "Enable the DSC Script Extension"
    default     = false
}
variable "DSCScriptExt_configurationUrlSasToken" {
    description = "The Configuration URL Sas Token, from the Storage account"
    default     = ""
}
variable "DSCScriptExt_Url" {
    description = "The DSC Script URL of the zip file. e.g. https://temp429574.blob.core.windows.net/dsccontainer/PowershellDSC.ps1.zip"
    default     = ""
}
variable "DSCScriptExt_Name" {
    description = "The Name of the Script you want to run. e.g. PowerShellDSC.ps1"
    default     = ""
}
variable "DSCScriptExt_Function" {
    description = "The Function you want to run e.g. PowershellDSC"
    default     = ""
}
variable "DSCScriptExt_dataCollection" {
    description = "Description"
    default     = "enable"
}
variable "DSCScriptExt_forcePullAndApply" {
    description = "Description"
    default     = false
}

// End of DSCScript Extensions variables
// #####################################

// #######################################
// Start of DSCService Extension variables

variable "enableDSCServiceextension" {
    description = "Enable the DSC Service Extension."
    default     = false
}
variable "DSCServiceExt_RegistrationKeyPassword" {
    description = "The Registration Key Password. This can be found using the following command:"
    default     = ""
}
variable "DSCServiceExt_RegistrationUrl" {
    description = "The Registration URL. This can be found using the following command."
    default     = ""
}
variable "DSCServiceExt_NodeConfigurationName" {
    description = "The Node Configuration Name e.g. PowerShellDSC.localhost"
    default     = ""
}
variable "DSCServiceExt_ConfigurationMode" {
    description = "The Configuration Mode can be set to one of three possibilities ApplyAndAutoCorrect, ."
    default     = "ApplyAndAutoCorrect"
}
variable "DSCServiceExt_ConfigurationModeFrequencyMins" {
    description = ""
    default     = 15
}
variable "DSCServiceExt_RefreshFrequencyMins" {
    description = "Description"
    default     = 30
}
variable "DSCServiceExt_RebootNodeIfNeeded" {
    description = "Description"
    default     = true
}
variable "DSCServiceExt_ActionAfterReboot" {
    description = "Description"
    default     = "continueConfiguration"
}
variable "DSCServiceExt_AllowModuleOverwrite" {
    description = "Description"
    default     = true
}

// End of DSCService Extensions variables
// ######################################




locals {
  
  
    lcidr    = ["10.0.0.0/16"]
  
    counting = flatten(values(local.expanded_names))

    expanded_names = {
        for name, count in var.name_counts : name => [
        for i in range(count) : format("%s%02d", name, i)
        ]
    }
    
    datadisk_count_map = { for k in local.counting : k => var.disks_per_vm}


    datadisk_lun_map = flatten([
        for vm_name, count in local.datadisk_count_map : [
        for i in range(count) : {
            datadisk_name = format("%s_datadisk%02d", vm_name, i)
            lun           = i
            storage_account_type = "Standard_LRS"
            create_option        = "Empty"
            disk_size_gb         = var.disksize
        }
        ]
    ])

}

```


``` terraform
//terraform.tfvars file

// Required Variables

location            = "uksouth"
resourcegroupname   = "Deathrace"
admin_password      = "ASimplePassword"

name_counts = {
    "foo" = 2
 /* "bar" = 4 */
} 


// Optional Variables

// ##########################
// Start of General variables

taglist = {
    "Environment" = "Staging"
    "Created by"  = "Russell Maycock"
    "Company"     = "Pinport Ltd"
    "project"     = "DeathRace"
    "website"     = "www.realworldit.net"
}


// End of General variables
// ########################



// ##################
// Key vault Settings

key_vault = {
    key_vault = {
      key_vault_resource_group  = "kv502857KeyVault"
      key_vault_name            = "kv502857"
      key_vault_secret_password = "Default-Admin-VM-Password"
    }
}

// end of key vault variables
// ##########################


// ##########################
// Start of Network variables

network_subnet_Frontend_name = "Frontend"

// End of Network variables
// ########################


// #################################
// Start of Virtua Machine variables

enable_automatic_updates    = true
eviction_policy             = "Deallocate"
priority                    = "Regular"
vmsize                      = "Standard_B2ms"
timezone                    = "GMT Standard Time"

// Virtual Machine identity block variables

identity_type = "SystemAssigned"

// Virtual Machine identity block variables end

// Virtual Machine os_disk block variables

caching              = "ReadWrite"
storage_account_type = "Standard_LRS"

// Virtual machine os_disk block variable ends

// Virtual machine secret block variable starts

secrets = {
  secrets1 = {
    store = "My"
    url   = "https://kv502857.vault.azure.net/secrets/foomachines/f......3"
  } 
}

// Virtual machine secret block variable ends

// Virtual machine source_image_reference block variables

imagepublishername  = "MicrosoftWindowsServer"
imageoffer          = "WindowsServer"
imagesku            = "2019-Datacenter"
imageskuversion     = "latest"

// Virtual machine source_image_reference block variables End

// Virtual machine Win_rm block variables

enable_winrm = {
  "Https" = "https://kv502857.vault.azure.net/secrets/foomachines/f......3"
}

// Virtual machine Win_rm block variables end

// Virtual machine Boot Diags Block variables start

boot_diagnostics_storage_account_uri = {
   "uri" = "https://te.....w3re.blob.core.windows.net/?sv=2........4=https&sig=L.....D"
}

// Virtual machine Boot Diags Block variables end


// Virtual Machine Variables End
// #############################


// ###############################
// start of Managed Disk variables

disks_per_vm    = 1
disksize        = 32

// end of Managed Disk variables
// #############################


// ############################################
// Start of IaaSAntiMalware Extension variables

enableIaaSAntiMalwareExtension                      = true
IaaSAntimalwareExt_AntimalwareEnabled               = "true"
IaaSAntimalwareExt_RealtimeProtectionEnabled        = "true"
IaaSAntimalwareExt_ScheduledScanSettings_isEnabled  = "true"
IaaSAntimalwareExt_ScheduledScanSettings_day        = "1"
IaaSAntimalwareExt_ScheduledScanSettings_time       = "120"
IaaSAntimalwareExt_ScheduledScanSettings_scanType   = "Quick"
IaaSAntimalwareExt_Exclusions_Extensions            = ""
IaaSAntimalwareExt_Exclusions_Paths                 = ""
IaaSAntimalwareExt_Exclusions_Processes             = ""

// End of IaaSAntimalware Extension variables
// ##########################################

// #########################################
// Start of CustomScript Extension variables

enableCustomScriptextension         = true
CustomScriptExt_commandToExecute    = "powershell.exe -Command \"./chocolatey.ps1; exit 0;\""
CustomScriptExt_filesUris = [
    "https://gist.githubusercontent.com/mcasperson/c815ac880df481418ff2e199ea1d0a46/raw/5d4fc583b28ecb27807d8ba90ec5f636387b00a3/chocolatey.ps1"
  ]

// End of CustomScript Extensions variables
// ########################################

// ######################################
// Start of DSCScript Extension variables

enableDSCScriptextension                = true
DSCScriptExt_configurationUrlSasToken   = "?sv=......Q%3D"
DSCScriptExt_Url                        = "https://te.....w3re.blob.core.windows.net/dsccontainer/PowershellDSC.ps1.zip"
DSCScriptExt_Name                       = "PowershellDSC.ps1"
DSCScriptExt_Function                   = "PowershellDSC"
DSCScriptExt_dataCollection             = "enable"
DSCScriptExt_forcePullAndApply          = false

// End of DSCScript Extensions variables
// #####################################

// #######################################
// Start of DSCService Extension variables

enableDSCServiceextension                       = false
DSCServiceExt_RegistrationKeyPassword           = "gRHraf.......2igb5="
DSCServiceExt_RegistrationUrl                   = "https://7d.....a21.agentsvc.uks.azure-automation.net/accounts/6e3....5a31"
DSCServiceExt_NodeConfigurationName             = "PowerShellDSC.localhost"
DSCServiceExt_ConfigurationMode                 = "ApplyAndAutoCorrect"
DSCServiceExt_ConfigurationModeFrequencyMins    = 15
DSCServiceExt_RefreshFrequencyMins              = 30
DSCServiceExt_RebootNodeIfNeeded                = true
DSCServiceExt_ActionAfterReboot                 = "continueConfiguration"
DSCServiceExt_AllowModuleOverwrite              = true

// End of DSCService Extensions variables
// ######################################



```