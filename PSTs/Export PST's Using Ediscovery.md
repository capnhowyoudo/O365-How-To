# Guide to Performing PST Exports for Office 365 Mailboxes via eDiscovery"

1. Login to [Microsoft 365 Defender Portal](https://sip.security.microsoft.com/homepage)
2. Select the eDiscovery Manager role group under Permissions > Email & collaboration roles > Roles. This ensures you have the required permissions to access eDiscovery tools."

<img width="826" height="854" alt="image" src="https://github.com/user-attachments/assets/3d12414c-552d-4733-b2b7-e4ae5f395506" />

3. Open the properties for the eDiscovery Manager role group and use the Edit button to add yourself as an eDiscovery Administrator. Verify that the Export role is listed; if it isn't, click Edit role group to add it, as this permission is vital for PST downloads.

<img width="562" height="758" alt="image" src="https://github.com/user-attachments/assets/20516e47-1a74-4911-9759-71ca204bbb26" />

4. Login to the [Microsoft Purview Compliance Portal](https://compliance.microsoft.com/) go to Content search, and click the New search button.

<img width="865" height="613" alt="image" src="https://github.com/user-attachments/assets/3bc14de1-868f-4bd9-a98c-b5a27c35c33a" />

> :information_source: If Content search is unavailable, wait for your eDiscovery permissions to sync (this can take up to 24 hours). Correct permissions are also required to enable the export and download features.

5. The content search wizard allows for granular configuration. Under Locations, you can specify the data source. For comprehensive mailbox coverage, enable Exchange mailboxes (1). Avoid selecting all locations unless SharePoint and Public Folder data are required, as this slows the search. Use the picker pane (2) to filter by specific users or teams.

<img width="785" height="655" alt="image" src="https://github.com/user-attachments/assets/81093c19-3125-44f3-ad78-c27ead6c34b8" />

