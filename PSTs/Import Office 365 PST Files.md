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

7.
