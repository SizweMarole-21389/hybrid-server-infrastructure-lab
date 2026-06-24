# Hybrid Server Infrastructure Deployment and Security Hardening

![Windows Server](https://img.shields.io/badge/Windows%20Server-2022-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![Hyper-V](https://img.shields.io/badge/Virtualisation-Hyper--V-0078D6?style=for-the-badge)
![Active Directory](https://img.shields.io/badge/Identity-Active%20Directory-1BA0D7?style=for-the-badge)
![Storage Spaces](https://img.shields.io/badge/Storage-Storage%20Spaces%20%2F%20Dedup-orange?style=for-the-badge)
![Hardening](https://img.shields.io/badge/Focus-Security%20Hardening-red?style=for-the-badge)

> A fully screenshotted, step-by-step lab deploying and hardening a hybrid Windows Server
> environment - Hyper-V virtualisation, Active Directory, Storage Spaces, FSRM quotas, data
> deduplication, and Windows Server Backup.

## Overview
This hands-on lab project documents the full deployment and security hardening of a hybrid server environment using Microsoft Hyper-V, Active Directory, and Windows Server Storage Spaces. The project is conducted on a virtualised domain environment hosted at the Central University of Technology (CUT), using the domain CUTHealth.ac.za. All configurations are captured with supporting screenshots across two major modules.

## Technologies and Tools
- Microsoft Hyper-V - virtualisation platform for the lab environment
- Active Directory Domain Services (AD DS) - domain management and Group Policy enforcement
- Windows Server Storage Spaces - software-defined storage with resiliency options
- File Server Resource Manager (FSRM) - quota management, file screening, and reporting
- Windows Server Backup - scheduled full server backup and recovery
- Print Management Console - centralised print server access control

## Skills Demonstrated
- Windows Server role installation and configuration
- Storage architecture design using virtual and software-defined disks
- File server quota enforcement and threshold alerting
- Role-based access control applied to print management
- Backup scheduling, destination configuration, and task automation

---

## Module 1 - File Server Resource Manager (FSRM) and Print Management Security

### Section I - FSRM Role Installation

**Screenshot I-1**
The Add Roles and Features Wizard is open on the Server Roles selection step. The File Server Resource Manager role is highlighted under File and Storage Services and File and iSCSI Services. The destination server is CUTHealthDC.CUTHealth.ac.za. The description panel confirms that FSRM enables administrators to manage and classify files on file servers, configure storage reports, define file screening policies, and enforce folder quotas.

![Screenshot I-1](File%20Server%20Resource%20Manager%20%28FSRM%29%20and%20Print%20Management%20Security/I/1.png)

**Screenshot I-2**
The Installation Progress screen confirms that FSRM is being installed on CUTHealthDC.CUTHealth.ac.za. The components listed for installation are File Server Resource Manager under File and iSCSI Services, as well as the accompanying File Server Resource Manager Tools under Remote Server Administration Tools. The progress bar indicates the installation is underway.

![Screenshot I-2](File%20Server%20Resource%20Manager%20%28FSRM%29%20and%20Print%20Management%20Security/I/2.png)

---

### Section III - Quota Configuration on C:\SharedData

**Screenshot III-1**
The Quota Properties dialog is open for the path C:\SharedData. A storage quota of 500 MB is being configured with the Hard quota option selected, which prevents users from writing data once the limit is reached. No notification thresholds have been added yet and no quota template has been applied.

![Screenshot III-1](File%20Server%20Resource%20Manager%20%28FSRM%29%20and%20Print%20Management%20Security/III/1.png)

**Screenshot III-2**
The Add Threshold dialog is open within the Quota Management console. A warning notification threshold is being configured to trigger at 80 percent of quota usage. The Email Message tab is active, showing the default notification template which would alert both administrators and the user who exceeded the threshold. The subject line reads the quota threshold percentage followed by the text "quota threshold exceeded."

![Screenshot III-2](File%20Server%20Resource%20Manager%20%28FSRM%29%20and%20Print%20Management%20Security/III/2.png)

**Screenshot III-3**
The Quota Properties dialog for C:\SharedData now reflects the completed threshold configuration. The Notification Thresholds list shows a single entry labelled "Warning (80%)", confirming the alert will fire when storage usage reaches 80 percent of the 500 MB hard quota limit.

![Screenshot III-3](File%20Server%20Resource%20Manager%20%28FSRM%29%20and%20Print%20Management%20Security/III/3.png)

---

### Section V - Print Management Security on CUTHealthRODC

**Screenshot V-1**
The Windows Settings Printers and Scanners panel is open on the CUTHealthRODC server, which is the Read-Only Domain Controller in the lab environment. The Add Printer wizard is at the Choose a Printer Port step, with the option to use an existing port LPT1 (Printer Port) selected. This is the first step in manually adding a printer to the server for centralised management.

![Screenshot V-1](File%20Server%20Resource%20Manager%20%28FSRM%29%20and%20Print%20Management%20Security/V/1.png)

**Screenshot V-2**
The Add Printer wizard is at the printer naming step. The printer has been named AdminPrinter. The wizard indicates the printer will be installed using the Generic/Text Only driver, which is standard for administratively managed print queues where physical driver compatibility is not required.

![Screenshot V-2](File%20Server%20Resource%20Manager%20%28FSRM%29%20and%20Print%20Management%20Security/V/2.png)

**Screenshot V-3**
The Advanced Security Settings dialog for AdminPrinter is displayed, showing the full access control list for the printer. Permission entries are listed for multiple principals including Everyone (Print), ALL APPLICATION PACKAGES (Print and Manage documents), CREATOR OWNER (Manage documents), Administrator (Print and Manage documents), Administrators (Read permissions and Print), and the IT-Support group (CUTHEALTH\IT-Support), which has been granted the Manage this printer permission. This confirms that role-based access control has been applied to the printer, restricting management capabilities to the designated support group.

![Screenshot V-3](File%20Server%20Resource%20Manager%20%28FSRM%29%20and%20Print%20Management%20Security/V/3.png)

**Screenshot V-4**
The AdminPrinter Properties security tab shows a simplified permissions view for the printer. The IT-Support group is selected and the Allow checkboxes for both Print and Manage this printer are ticked. This confirms that the IT-Support group has been granted the appropriate administrative permissions over the printer without elevating their broader domain privileges.

![Screenshot V-4](File%20Server%20Resource%20Manager%20%28FSRM%29%20and%20Print%20Management%20Security/V/4.png)

---

## Module 2 - Storage Spaces, Data Deduplication, and Server Backup Management

### Section I - Virtual Hard Disk Provisioning in Hyper-V

**Screenshot I-1**
The New Virtual Hard Disk Wizard in Hyper-V Manager is at the Choose Disk Format step. The VHDX format is selected. VHDX supports virtual disks up to 64 TB and provides resilience to consistency issues caused by unexpected power failures, making it the recommended format for production environments running Windows Server 2012 and later.

![Screenshot I-1](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/I/1.png)

**Screenshot I-2**
The wizard is at the Choose Disk Type step. The Dynamically expanding type is selected, meaning the virtual hard disk file starts small and grows on disk only as data is written to it. This option optimises physical storage consumption and is recommended for servers that are not disk-intensive.

![Screenshot I-2](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/I/2.png)

**Screenshot I-3**
The Specify Name and Location step shows the first virtual hard disk being named Storage1, saved to the default Hyper-V virtual hard disk path. This disk will be used as one of the physical disks in the Windows Storage Spaces pool.

![Screenshot I-3](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/I/3.png)

**Screenshot I-4**
The Configure Disk step shows Storage1 being created as a new blank virtual hard disk with a size of 25 GB. The host system exposes two physical drives, PHYSICALDRIVE0 at 476 GB and PHYSICALDRIVE1 at 29 GB, as alternative source options, but a new blank disk is being created for the lab environment.

![Screenshot I-4](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/I/4.png)

**Screenshot I-5**
The Specify Name and Location step for the second virtual hard disk shows it named Storage2, stored in the same default directory. This is the second of three disks to be added to the CUTHealthRODC virtual machine via the SCSI controller.

![Screenshot I-5](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/I/5.png)

**Screenshot I-6**
The Configure Disk step for Storage2 sets the disk size to 35 GB as a new blank virtual hard disk, providing a larger capacity than Storage1 to allow for an asymmetric storage pool configuration.

![Screenshot I-6](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/I/6.png)

**Screenshot I-7**
The Specify Name and Location step for the third virtual hard disk shows it named Storage3, saved to the same default location. This completes the provisioning of three separate virtual disks for the Storage Spaces configuration.

![Screenshot I-7](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/I/7.png)

**Screenshot I-8**
The Hyper-V Settings panel for CUTHealthRODC displays the completed hardware configuration. Under the SCSI Controller section, three virtual hard drives are now attached: Storage1.vhdx, Storage2.vhdx, and Storage3.vhdx. Storage3 is currently selected and its full file path is confirmed, demonstrating that all three disks have been successfully attached to the virtual machine.

![Screenshot I-8](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/I/8.png)

**Screenshot I-9**
The Disk Management console on CUTHealthRODC confirms that the three newly attached virtual disks are visible to the operating system. Disk 0 is the existing OS disk at 127 GB containing the C: drive. Disk 1 at 25 GB, Disk 2 at 35 GB, and Disk 3 at 45 GB appear as fully unallocated, confirming they are ready to be consumed by Windows Storage Spaces.

![Screenshot I-9](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/I/9.png)

---

### Section II - Storage Pool Creation

**Screenshot II-1**
The New Storage Pool Wizard is at the Specify a Storage Pool Name and Subsystem step. The pool is named StoragePool and the Windows Storage subsystem on CUTHealthRODC is selected as the target. The Primordial pool, which represents all available unassigned physical disks, is listed in the selection panel.

![Screenshot II-1](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/II/1.png)

**Screenshot II-2**
The Select Physical Disks step shows all three Microsoft Virtual Disks selected for the pool: 25 GB, 45 GB, and 35 GB. The combined total selected capacity is 105 GB. All disks are assigned Automatic allocation and use the SAS bus type. The wizard confirms that selecting these disks will create a local storage pool.

![Screenshot II-2](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/II/2.png)

**Screenshot II-3**
The Confirm Selections screen summarises the storage pool configuration before creation. The server is CUTHealthRODC, the pool name is StoragePool, total capacity is 105 GB, and all three Microsoft Virtual Disks are listed with Automatic allocation. The cluster role is Not Clustered and the subsystem is Windows Storage.

![Screenshot II-3](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/II/3.png)

**Screenshot II-4**
The View Results screen confirms the New Storage Pool Wizard completed successfully. All three tasks, Gather information, Create storage pool, and Update cache, show a Completed status. The storage pool with 103 GB usable capacity is now visible in the Server Manager background panel.

![Screenshot II-4](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/II/4.png)

---

### Section II-A - Virtual Disk Creation (DataVol - Simple Layout)

**Screenshot II-A-1**
The New Virtual Disk Wizard is at the Specify the Virtual Disk Name step. The virtual disk is named DataVol. The StoragePool on CUTHealthRODC with 103 GB total capacity is shown as the parent pool. Storage tiering is not available as the pool does not contain mixed media types.

![Screenshot II-A-1](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/II/A/1.png)

**Screenshot II-A-2**
The Select the Storage Layout step shows the Simple layout selected for DataVol. The description confirms that Simple striping maximises capacity and read/write throughput by distributing data across all physical disks, but does not provide protection against disk failure. This layout is chosen for the primary data volume.

![Screenshot II-A-2](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/II/A/22.png)

**Screenshot II-A-3**
The Specify the Provisioning Type step shows Thin provisioning selected. With thin provisioning, the virtual disk claims space from the storage pool only as data is actually written, up to the maximum specified size. This optimises overall pool utilisation when volumes are not immediately filled.

![Screenshot II-A-3](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/II/A/3.png)

**Screenshot II-A-4**
The size configuration step is shown at a reduced zoom level alongside the Server Manager background interface. The DataVol virtual disk is being allocated a total requested size of 70 GB from the StoragePool, representing the majority of the pool capacity reserved for primary data storage.

![Screenshot II-A-4](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/II/A/4.png)

**Screenshot II-A-5**
The Confirm Selections screen for DataVol displays the final configuration summary before creation: StoragePool as the parent pool, Simple storage layout, Thin provisioning type, and a total requested size of 70 GB. The configuration is ready for creation.

![Screenshot II-A-5](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/II/A/5.png)

**Screenshot II-A-6**
The New Volume Wizard is at the Select File System Settings step for the DataVol volume. The file system is set to ReFS (Resilient File System), which provides enhanced data integrity features, corruption detection, and optimised compatibility with Storage Spaces virtual disks. The allocation unit size is set to Default.

![Screenshot II-A-6](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/II/A/6.png)

**Screenshot II-A-7**
The Confirm Selections step for the DataVol volume displays the final volume configuration: Virtual disk = DataVol, residing on Disk 4, Volume size = 70.5 GB, Drive letter assigned = E:\, File system = ReFS, Short file name creation = Disabled, and Allocation unit size = Default.

![Screenshot II-A-7](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/II/A/7.png)

---

### Section II-B - Virtual Disk Creation (LogsVol - Mirror Layout)

**Screenshot II-B-1**
The New Virtual Disk Wizard for the second virtual disk shows the name LogsVol being entered. The StoragePool background panel confirms 99 GB of free space remains available. This disk uses a different storage layout from DataVol, providing redundancy for log data storage.

![Screenshot II-B-1](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/II/B/1.png)

**Screenshot II-B-2**
The Select the Storage Layout step for LogsVol shows the Mirror layout selected. The description explains that mirroring writes two or three copies of data across physical disks, increasing reliability at the cost of reduced usable capacity. A minimum of two disks is required to protect against a single disk failure.

![Screenshot II-B-2](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/II/B/2.png)

**Screenshot II-B-3**
The Specify the Provisioning Type step for LogsVol shows Fixed provisioning selected. With fixed provisioning, the entire allocated size is immediately claimed from the storage pool at creation time. This provides predictable and reserved space, which is appropriate for log volumes where consistent availability is required.

![Screenshot II-B-3](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/II/B/3.png)

**Screenshot II-B-5**
The New Volume Wizard for the LogsVol volume is at the Select File System Settings step. The file system is configured as NTFS, which is suitable for log file workloads that benefit from journaling and fine-grained access control. The StoragePool background panel now reflects 58.5 GB of remaining free space, confirming the LogsVol disk has been successfully provisioned and claimed space from the pool.

![Screenshot II-B-5](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/II/B/5.png)

---

### Section III - Data Deduplication Configuration

**Screenshot III-1**
The Add Roles and Features Wizard is shown at the Server Roles selection step at a reduced zoom level to display the full interface. The Data Deduplication role is being selected for installation under File and Storage Services. This role enables the operating system to identify redundant data blocks across storage volumes and store only single copies, reducing consumed disk space.

![Screenshot III-1](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/III/1.png)

**Screenshot III-2**
The Volumes panel in Server Manager shows 5 volumes on CUTHealthRODC. The E: drive (New Volume, 70.4 GB, Thin provisioned, backed by the DataVol virtual disk) is selected. The Deduplication Settings dialog for the E: volume is open, showing data deduplication currently set to Disabled with the default file exclusions of .edb and .jrs file types.

![Screenshot III-2](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/III/2.png)

**Screenshot III-3**
The Deduplication Settings dialog for the E: drive now shows data deduplication configured in General Purpose File Server mode. Files older than 3 days will be eligible for deduplication processing. The Set Deduplication Schedule button is available to define when background deduplication jobs will execute to minimise impact on server performance.

![Screenshot III-3](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/III/3.png)

**Screenshot III-4**
A full interface screenshot of the Volumes panel confirms the completed deduplication configuration. The E: volume now populates the Deduplication Savings and Deduplication Status columns, confirming that data deduplication has been successfully enabled on the DataVol storage volume and is ready to begin processing.

![Screenshot III-4](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/III/4.png)

---

### Section IV - Windows Server Backup Configuration

**Screenshot IV-1**
The Add Roles and Features Wizard is at the Features selection step. The Windows Server Backup feature is selected for installation. This feature provides the native backup and recovery capability for Windows Server, supporting scheduled full server backups, incremental backups, and bare-metal recovery.

![Screenshot IV-1](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/IV/1.png)

**Screenshot IV-2**
The Windows Server Backup console (wbadmin) is open on CUTHealthRODC. The Backup Schedule Wizard is at the Select Backup Configuration step. The Full server (recommended) option is selected, which backs up all server data, applications, and system state in a single operation. The estimated backup size is 14.80 GB.

![Screenshot IV-2](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/IV/2.png)

**Screenshot IV-3**
The Specify Backup Time step shows the primary backup schedule configured to run once a day at 2:00 AM. An additional scheduled run at 9:00 PM is listed under the More than once a day option, providing two recovery points per day for improved recovery time objectives.

![Screenshot IV-3](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/IV/3.png)

**Screenshot IV-4**
The Specify Destination Type step shows the backup storage destination being configured. The recommended option, Back up to a hard disk that is dedicated for backups, is selected. This option formats an entire disk exclusively for backup storage, preventing backup operations from competing with active server workloads and providing the highest level of backup reliability.

![Screenshot IV-4](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/IV/4.png)

**Screenshot IV-5**
The Select Destination Disk step shows Disk 5, a Microsoft Virtual Disk of 20 GB mounted as drive F:\, selected as the dedicated backup destination. The disk contains only 74.25 MB of used space, confirming it is essentially empty and ready to be formatted and fully dedicated to backup storage.

![Screenshot IV-5](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/IV/5.png)

**Screenshot IV-6**
The Task Scheduler Edit Trigger dialog displays the backup schedule configuration in detail. The trigger is set to run Weekly every Sunday at 2:00 AM, starting from 11 November 2025 with an expiry of 11 November 2026. The task is enabled and configured to synchronise across time zones, ensuring consistent execution in multi-timezone environments.

![Screenshot IV-6](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/IV/6.png)

**Screenshot IV-7**
The Task Scheduler shows the Microsoft-Windows-WindowsBackup scheduled task listed under the Windows Backup task library folder. The task status is Ready, the trigger confirms it fires at 2:00 AM every Sunday, and the next scheduled run is 16 November 2025 at 2:00 AM. The task runs under the SYSTEM account and is configured to execute whether or not a user is logged in, ensuring unattended backup execution.

![Screenshot IV-7](Storage%20Spaces,%20Data%20Deduplication,%20and%20Server%20Backup%20Management/IV/7.png)
