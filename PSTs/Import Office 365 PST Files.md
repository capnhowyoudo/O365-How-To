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
