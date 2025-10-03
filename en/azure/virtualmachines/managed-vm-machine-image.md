# AZURE / Virtual Machines / Managed VM Machine Image

## Quick Info

| | |
|-|-|
| **Plugin Title** | Managed VM Machine Image |
| **Cloud** | AZURE |
| **Category** | Virtual Machines |
| **Description** | Ensures that VM is launched from a managed VM image. |
| **More Info** | A managed VM image contains the information necessary to create a VM, including the OS and data disks. Virtual Machines should be launched using managed images to ensure security practices and consistency across all the instances. |
| **AZURE Link** | https://docs.microsoft.com/en-us/azure/virtual-machines/windows/create-vm-generalized-managed |
| **Recommended Action** | Ensure that VM is launched using managed VM image. |

## Detailed Remediation Steps

1. Log into the Microsoft Azure Management Console.
2. At the top of the page, under "Azure services" look for the "Virtual machines" icon. If you cannot find that option, click on "More services" and filter for the "Virtual machines" option.
3. Select the applicable Virtual Machine from the list of Virtual Machines by clicking on the "Name" link.
4. On the left side menu for the selected Virtual Machine, expand "Settings" and click on the "Disks" option.
5. At the top of the Disks section, click on the "Migrate to managed disks" button.
6. After clicking the button, you will receive one of two screens:
   1. If your VM is in an availability set, there will be a warning on the Migrate to managed disks blade that you need to convert the availability set first. The warning should have an info box you can click to convert the availability set. Click on the "Convert" button. Once the availability set is converted, click on the "Migrate" button to start the process of migrating your disks to managed disks.
   2. If your VM is not in an availability set, you will be presented with a screen notifying you that after the migration is complete, the source unmanaged disks will not be deleted. At the bottom of the screen, click on the "Migrate" button to start the process of migrating your disks to managed disks.
7. The VM will be stopped and restarted after migration is complete.
8. Repeat steps 3-5 for all other applicable VMs.