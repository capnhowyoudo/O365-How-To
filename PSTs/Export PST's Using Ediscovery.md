# Guide to Performing PST Exports for Office 365 Mailboxes via eDiscovery"

1. Login to [Microsoft 365 Defender Portal](https://sip.security.microsoft.com/homepage)
2. Select the eDiscovery Manager role group under Permissions > Email & collaboration roles > Roles. This ensures you have the required permissions to access eDiscovery tools."

<img width="577" height="593" alt="image" src="https://github.com/user-attachments/assets/67f34d55-9391-497a-a1a7-2ba6af2ac27e" />

3. Open the properties for the eDiscovery Manager role group and use the Edit button to add yourself as an eDiscovery Administrator. Verify that the Export role is listed; if it isn't, click Edit role group to add it, as this permission is vital for PST downloads.

<img width="568" height="756" alt="image" src="https://github.com/user-attachments/assets/36fbce78-947f-4b8b-a759-3fa969ae286b" />

4. Login to the [Microsoft Purview Compliance Portal](https://compliance.microsoft.com/) go to Content search, and click the New search button.

<img width="575" height="404" alt="image" src="https://github.com/user-attachments/assets/12ea5c16-2dc1-4373-afeb-ffaf4707e9b9" />

> :information_source: If Content search is unavailable, wait for your eDiscovery permissions to sync (this can take up to 24 hours). Correct permissions are also required to enable the export and download features.

5. The content search wizard allows for granular configuration. Under Locations, you can specify the data source. For comprehensive mailbox coverage, enable Exchange mailboxes (1). Avoid selecting all locations unless SharePoint and Public Folder data are required, as this slows the search. Use the picker pane (2) to filter by specific users or teams.

<img width="785" height="655" alt="image" src="https://github.com/user-attachments/assets/81093c19-3125-44f3-ad78-c27ead6c34b8" />

6. In addition to location filters, the Conditions page provides several ways to narrow down your results:

- Keywords & Phrases: Filter by specific terms using logical operators (e.g., AND, OR, NEAR, or NOT).

- Language & Region: Specify a query language or regional setting.

- Specific Criteria: Use the Add condition button to filter by timestamps, message fields, or metadata properties.

> :information_source: Note: If your goal is to export all mailbox data, leave the keywords and conditions sections completely blank.

<img width="582" height="610" alt="image" src="https://github.com/user-attachments/assets/c8a2a9ba-0c59-428a-95cd-d4cb35a167b2" />

7. Select Submit and Done in the final step to exit the wizard and start the content search.
 
8. Search processing occurs in the background; status updates are available by selecting the search within the Search tab.

> :information_source: If the status shows as "Starting" for a long time, you may need to refresh the page to see the updated progress.

<img width="574" height="475" alt="image" src="https://github.com/user-attachments/assets/130210c4-2cf0-4c47-8914-ef498694376a" />

9. Check the results pane to confirm the search is complete. When ready, go to Actions > Export results to begin the download process

<img width="573" height="571" alt="image" src="https://github.com/user-attachments/assets/2d22f48c-5454-46d4-9322-cc09960e4cba" />

10. Choose your encryption preferences and Exchange output settings. I recommend the defaults, which generate individual PSTs for each mailbox. Note: Only enable deduplication if you specifically want everything exported into one single PST. When ready, click Export.

<img width="556" height="844" alt="image" src="https://github.com/user-attachments/assets/91e8029f-0807-44df-a446-dc1387a6ae1c" />

11. Once the export starts, head over to the Export tab. Click on your search name to view the real-time progress of the data preparation.

<img width="577" height="532" alt="image" src="https://github.com/user-attachments/assets/820476f5-f2fa-4b5f-9981-2389fbf562d7" />

12. Select Download results now or wait until the export is 100% complete—the tools are designed to sync up regardless. Before leaving this screen, copy the Export key, as it is required to authenticate the download application.

<img width="561" height="702" alt="image" src="https://github.com/user-attachments/assets/51aefa6e-3094-473c-a67a-ee5576d10ad8" />

13. Once the eDiscovery PST Export Tool opens, paste your Export key into the first box. Then, click 'Browse' to choose the local folder where the PST files should be downloaded.

<img width="574" height="223" alt="image" src="https://github.com/user-attachments/assets/3ce174cd-50f0-4224-a2f9-6ae6ca9a07e5" />

Once you click Start, the application will begin downloading the data. When the process reaches 100%, you can safely close the tool.

To clean up, you can delete the eDiscovery search by returning to the Search tab, selecting your search, and choosing Delete from the Actions menu.







