# How to import Office 365 PST files

Follow the step-by-step instructions below to import Office 365 PST files.

# Assign the Mailbox Import Export role to enable mailbox import/export actions.

Ensure you are assigned the Mailbox Import Export role in Exchange Online before creating import jobs in the Microsoft Purview compliance portal. Note that this role is not assigned to any role group by default.

Assign the Mailbox Import Export role in Exchange Online before creating import jobs or importing PST files using the Microsoft Purview compliance portal.

1. Sign in to the [Exchange Admin Center](https://admin.cloud.microsoft/exchange#/)
2. Navigate to Roles → Admin roles.
3. Select Organization Management.

<img width="734" height="516" alt="image" src="https://github.com/user-attachments/assets/d77f754c-b36b-49a7-9360-45f0756dc7d2" />

4. Go to Permissions.
5. Choose the Mailbox Import Export role.
6. Select Save to apply the changes.

> :information_source: Please note that the update can take up to 24 hours to propagate, but in most cases it becomes effective within 1 hour.

<img width="735" height="516" alt="image" src="https://github.com/user-attachments/assets/ff53cbdd-94e0-49be-a8e3-455f8d510965" />

If the following error appears after selecting Save:

> :no_entry: We couldn’t save your changes. Please try again.

A convenient alternative method to assign the Mailbox Import Export role is by using PowerShell.

Establish a connection to Exchange Online PowerShell and run the New-ManagementRoleAssignment cmdlet.

     Connect-ExchangeOnline
     New-ManagementRoleAssignment -Role "Mailbox Import Export" -SecurityGroup "Organization Management" -Name "Import Export Org Management"

# Create a PST import job.

To create a PST import job, follow these steps:

1. Sign in to the [Microsoft Purview compliance portal](https://compliance.microsoft.com/)
2. Expand Data lifecycle management and select Microsoft 365.
3. Click Import.
4. Choose New Import Job.

<img width="737" height="521" alt="image" src="https://github.com/user-attachments/assets/50004719-eed8-4ce2-a76d-56c0eb41caf5" />

If New Import Job is not visible, please wait for the previous changes to propagate.

<img width="739" height="521" alt="image" src="https://github.com/user-attachments/assets/d3ade812-7f18-465e-9d33-dbaa3495e454" />

5. Type a name for your import job and then click Next to continue.

<img width="736" height="520" alt="image" src="https://github.com/user-attachments/assets/0a427fff-4e31-4a6a-a585-6932cad26a3c" />

6. Click Upload your data, then Next.

<img width="738" height="520" alt="image" src="https://github.com/user-attachments/assets/6acdcabd-4eb7-47a7-a41b-8404ec81b9d1" />

7. Select Show network upload SAS URL.

<img width="736" height="522" alt="image" src="https://github.com/user-attachments/assets/4f5af50d-1d97-458d-afbf-2d5f5706de2a" />

8. Select Copy to clipboard to copy the SAS URL, which will be required in the following step.

<img width="735" height="518" alt="image" src="https://github.com/user-attachments/assets/1610a089-c3f1-4198-a283-c7185707c7ab" />

Next, you’ll upload your PST files to Office 365 with the AzCopy tool.

> :information_source: Make sure not to cancel the import job—you’ll need to come back to this window to finish it. Canceling means starting the process again.

# Upload PST files to Office 365.

1. Download [AzCopy (v10)](https://aka.ms/downloadazcopylatest) from Microsoft and place it in the C:\Temp directory. Since it is a standalone executable, installation is not necessary.

<img width="741" height="556" alt="image" src="https://github.com/user-attachments/assets/d9093477-7158-4a1a-89a9-3a671f8ad135" />

2. Verify where your PST files are stored.
  
You can use a network share (e.g., \\yourserver\PSTs) or a local folder (e.g., C:\PSTs). In this example, the PST files are located at C:\Temp\PSTs on the local drive.

<img width="743" height="556" alt="image" src="https://github.com/user-attachments/assets/43c81c84-0984-4ecc-95ab-8b5c3e803edb" />

3. Open Command Prompt as an administrator.
4. Change the directory to C:\Temp\.
5. Paste the SAS URL you copied earlier into the command.
6. Run the command to upload the PST files to Azure.

          cd C:\Temp\
          azcopy.exe copy "C:\Temp\PSTs" "PASTE_Your_SAS_URL_HERE" --recursive

7. After running the command, the output will look similar to the example below.

          INFO: Scanning...
          INFO: Any empty folders will not be processed, because source and/or destination doesn't have full folder support

          Job 37cf8d63-d431-e143-78c2-e3a9b85d0d7e has started
          Log file is located at: C:\Users\administrator.EXOIP\.azcopy\37cf8d63-d431-e143-78c2-e3a9b85d0d7e.log

          INFO: Could not read destination length. If the destination is write-only, use --check-length=false on the command line.
          100.0 %, 1 Done, 0 Failed, 0 Pending, 0 Skipped, 1 Total, 2-sec Throughput (Mb/s): 26.7263


          Job 37cf8d63-d431-e143-78c2-e3a9b85d0d7e summary
          Elapsed Time (Minutes): 20.6073
          Number of File Transfers: 1
          Number of Folder Property Transfers: 0
          Total Number of Transfers: 1
          Number of Transfers Completed: 1
          Number of Transfers Failed: 0
          Number of Transfers Skipped: 0
          TotalBytesTransferred: 4673750016
          Final Job Status: Completed

> :information_source: Azure Storage Explorer cannot be used to upload or modify PST files. The supported approach for importing PST files is through AzCopy. PST files uploaded to the Azure blob cannot be removed, and any attempt to delete them will trigger a permissions error.

# View uploaded PST files in Office 365.

Follow the steps below to install Azure Storage Explorer and connect to your Azure Storage account:

1. Download and install the [Microsoft Azure Storage Explorer application](https://github.com/microsoft/AzureStorageExplorer/releases)

<img width="741" height="519" alt="image" src="https://github.com/user-attachments/assets/94cb7b64-5268-40b2-ae80-4e1c87c581cb" />

2. Launch Microsoft Azure Storage Explorer.
3. Right-click Storage Accounts.
4. Click Connect to Azure Storage.

<img width="737" height="572" alt="image" src="https://github.com/user-attachments/assets/ebc64efe-4884-4226-a946-2b55370dd9c1" />

5. Click on Blob container.

<img width="739" height="558" alt="image" src="https://github.com/user-attachments/assets/b4ed0bb2-d7da-4ac7-a624-f362457f4343" />

6. Choose Shared access signature URL (SAS) and select Next.

<img width="737" height="561" alt="image" src="https://github.com/user-attachments/assets/d63a3ae3-2f31-44bd-b69b-46f10c6b0481" />

7. Provide a Display name.
8. Paste the Blob container SAS URL obtained earlier.
9. Select Next.

<img width="738" height="560" alt="image" src="https://github.com/user-attachments/assets/be3d83ac-e759-44e9-8a37-29c005c26e0c" />

10. Click Connect.

<img width="741" height="558" alt="image" src="https://github.com/user-attachments/assets/cc9d3fac-e45a-4ea5-8338-718975e982f4" />

11. The PSTs folder and the PST file will now be visible.

<img width="736" height="569" alt="image" src="https://github.com/user-attachments/assets/30c1f974-b6aa-4808-8bdd-2e514ecb5e19" />

> :information_source: PST files are automatically deleted from the Azure storage area. When there are no active import jobs, files within the ingestiondata container are purged 30 days after the most recent import job creation.

Confirm that the PST files are available in Azure storage, then proceed to the next step.

# Create PST import mapping file

1. Download the PST import mapping file from [Microsoft Direct](https://go.microsoft.com/fwlink/p/?LinkId=544717)
2. Launch Microsoft Excel.
3. Open Excel Advanced Options and update the settings listed below:
   >
     - Uncheck Use system separators.
     - Set Decimal separator to .
     - Set Thousands separator to ,

> :stop_sign:  Ensure the system separators are configured correctly; otherwise, uploading the CSV file in the next step will trigger a mapping file validation error.

<img width="737" height="687" alt="image" src="https://github.com/user-attachments/assets/7de95057-1cc2-4ffb-81d7-cf9bec3aa16e" />

4. Open the CSV file in Excel and modify the values.

When editing the CSV file, pay close attention to each value and its purpose:

- Workload: Specifies the service to which the data will be imported. For importing PST files to user mailboxes, set this to Exchange.
- FilePath: The folder path in Azure Storage where the PST files were uploaded.
- Name: The name of the PST file to import. This value is case-sensitive.
- Mailbox: The email address of the target mailbox for the PST import.
- IsArchive: Indicates whether the PST file should be imported into the user’s archive mailbox. Set to TRUE or FALSE. If you choose TRUE, ensure the user’s archive mailbox is enabled.
- TargetRootFolder: Specifies the destination folder in the mailbox. Leave blank: Creates a new folder named Imported at the root level of the mailbox (same level as Inbox and other default folders). /: Imports the PST file’s folders and items to the top of the mailbox folder structure. /foldername: Imports all items and folders from the PST into a folder named foldername.

All other fields in the CSV file are optional and may remain empty.

<img width="739" height="143" alt="image" src="https://github.com/user-attachments/assets/1ecd1917-ae29-4cfa-b1cb-fa5a205e62b0" />

5. You can open the CSV file with Notepad to change the values or just check that it’s saved as a comma-delimited file.

<img width="742" height="162" alt="image" src="https://github.com/user-attachments/assets/47eba2ca-3736-47ec-96d2-af887e782580" />

# Choose the PST import mapping file.

1. Navigate back to the PST import job, mark both checkboxes, and then click Next.

<img width="738" height="519" alt="image" src="https://github.com/user-attachments/assets/d66d3972-1aa4-4a23-ad04-500be6ec045a" />

2. Click Select mapping file.
3. Navigate to the location of the PstImportMappingFile.csv file.
4. Click Validate.

<img width="740" height="520" alt="image" src="https://github.com/user-attachments/assets/670f7f1d-7ad9-4b8f-82aa-a78c946541ed" />

5. If a green confirmation appears, the mapping file is valid. Proceed by clicking Next.

<img width="737" height="517" alt="image" src="https://github.com/user-attachments/assets/99861b62-4e14-4a65-afca-8888c4c6a8ec" />

6. Verify the PST import job and click Submit to start the import.

<img width="738" height="520" alt="image" src="https://github.com/user-attachments/assets/b7dccd63-693e-4a4b-aac5-1efe74ed4081" />

7. The PST files have been uploaded successfully. Just click Done to continue.

<img width="738" height="518" alt="image" src="https://github.com/user-attachments/assets/6c4bdb26-e8e9-45b6-adc9-6d58cf229d63" />

One last step remains: start the PST import to Office 365.

# Initiate the PST import process in Office 365.

You must wait for the analysis process to finish before importing the PST file to Office 365. When complete, the status will display Import completed, indicating it is ready to proceed.

1. Mark the checkbox for the import job.
2. Click Import to Office 365 to start the import.

<img width="738" height="522" alt="image" src="https://github.com/user-attachments/assets/e26c502b-d746-411b-88b4-3acbefd771d0" />

3. Choose No, I want to import everything and click Next to proceed.

<img width="733" height="519" alt="image" src="https://github.com/user-attachments/assets/236dd817-f8b6-40a1-bd0b-dbac2e3a1b34" />

4. Choose Submit

<img width="736" height="518" alt="image" src="https://github.com/user-attachments/assets/2f85c942-fdd5-4a5a-8ab5-f0c879d76294" />

5. The PST files have been successfully uploaded to the cloud. Click Done to finish.

<img width="739" height="518" alt="image" src="https://github.com/user-attachments/assets/e9f0afd3-1017-408d-a98a-32a5baeed8da" />

> :information_source: The PST import cannot be forced. You must wait for Microsoft to accept the request and begin processing.

# Verify PST import job status.

1. To check the PST import job status, click the import job in the list view and confirm that its status shows Completed.

<img width="728" height="515" alt="image" src="https://github.com/user-attachments/assets/31f4f6ff-7aee-4cc6-a219-6c6104226da2" />

Congratulations! You’ve successfully imported the PST file into Office 365.
