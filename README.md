![MIKES DATA WORK GIT REPO](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_01.png "Mikes Data Work")        

# How To Configure AlwaysOn in SQL Server
**Post Date: March 25, 2015**        



## Contents    
- [About Process](##About-Process)  
- [Author](#Author)  
- [License](#License)       

## About-Process

<p>How to configure SQL Server 2012 AlwaysOn in a Windows Server Failover Cluster ( WSFC ). In this environment I have configured a basic Cluster using 2 Member Servers ( SQLC-01 & SQLc-02 ). The cluster was created using a Node and FileShare Majority without shared storage. You must have at least 2 servers that have already been configured in a cluster, and if you are using a multi-subnet environment you must have 2 available IP addresses one for each subnet. These must not be formerly assigned to any server or setup in DNS.</p>

1. Login to the Server where SQL Server has been installed and open 'SQL Server Configuration Manager'.

2. Right-click the database service and select 'Properties'.

![Click Properties]( https://mikesdatawork.files.wordpress.com/2015/03/image022.jpg "Click Properties")
 
3. Select the tab 'AlwaysOn High Availability', and check 'Enable AlwaysOn Availability Groups', and click 'OK'.
Note: This process must be repeated for both nodes in the cluster ( Primary and Secondary Servers ).

![Enable AlwaysOn]( https://mikesdatawork.files.wordpress.com/2015/03/image024.jpg "Enable AlwaysOn")
 
4. Next; you will need to open the 'SQL Server Management Studio'. Ensure you are running it under the Database Service Account. In this case we are using a database service account called "SQL_SVC".

![Run SQL Server Management Studio]( https://mikesdatawork.files.wordpress.com/2015/03/image025.jpg "Run SQL Server Management Studio SSMS")
 
5. Specify the appropriate credentials and click 'OK'.

![Add SQL Service Account]( https://mikesdatawork.files.wordpress.com/2015/03/image026.jpg "Add SQL Service Account")
 
6. Click 'Connect'.

![Authenticate To SQL Server]( https://mikesdatawork.files.wordpress.com/2015/03/image027.jpg "Authenticate To SQL Server")
 
7. Right-click 'AlwaysOn High Availability' and select 'New Availability Group Wizard'.

![Create Availability Group]( https://mikesdatawork.files.wordpress.com/2015/03/image028.jpg "Create Availability Group")
 
8. Click 'Next'.

![Click Next]( https://mikesdatawork.files.wordpress.com/2015/03/image029.jpg "Click Next")
 
9. Type the name of your Availability Group ( In this example we are using SQLCAG ) and click 'Next'.

![Create Availability Group Name]( https://mikesdatawork.files.wordpress.com/2015/03/image030.jpg "Specify AG Name")
 
10. Any database you have available will automatically be checked in the list. This is your opportunity to uncheck any databases you wish NOT to be configured for AlwaysOn. Simply Uncheck the databases you want to exclude from the configuration. In this configuration I have created a sample database called DBSYSMON. Click 'Next' to continue.

![Select Databases You Want To Mirror]( https://mikesdatawork.files.wordpress.com/2015/03/image031.jpg "Select Databases For AlwaysOn Configuration")
 
11. Click 'Add Replica'.

![Add Replica Database]( https://mikesdatawork.files.wordpress.com/2015/03/image032.jpg "Add Replica Database")
 
12. Click 'Connect' so you can authenticate to the other Database Server ( Secondary server in the Cluster ). In this example we are using a server called SQLC-02.

![Connect to Secondary Server]( https://mikesdatawork.files.wordpress.com/2015/03/image033.jpg "Connect To Secondary Replica Server")
 
13. Complete the following actions before going to the Endpoints tab.
1. Check the box for 'Automatic Failover'.
2. Check the box for 'Synchronous Commit'.
3. Check the box for 'Readable Secondary'.
4. Select the next tab 'Endpoints'.

![Configure Failover]( https://mikesdatawork.files.wordpress.com/2015/03/image034.jpg "Configure Failover")
 
14. Rename the 'Endpoint Name' by adding the suffix "_SQLC". This will allow you to differentiate the Endpoint names from any other Endpoint Configuration in the future.
1. Add suffix "_SQLC".
2. Click the tab 'Backup Preferences'.

![Configure Backup Preferences]( https://mikesdatawork.files.wordpress.com/2015/03/image035.jpg "Configure Backup Preferences")
 
15. Select the option for 'Any Replica' and click the tab 'Listener'.

![Configure Listener]( https://mikesdatawork.files.wordpress.com/2015/03/image036.jpg "Configure Listener")
 
16. Complete the steps for creating the Listener by performing the following actions.
a. 1. Select the option for 'Create an availability group listener'.
b. 2. Create the Listener DNS Name, and provide the Port and Network Mode.
a. Listener DNS Name: SQLC_Listener
b. Port: 1433
c. Network Mode: Static IP
c. 3. Select 'Add'.
d. 4. Provide the IP from the first subnet, then follow this process again by clicking the 'Add' and prove the IP for the second subnet.
e. 5. Click 'Next'.

![Provide IP Information For Listener]( https://mikesdatawork.files.wordpress.com/2015/03/image037.jpg "Provide IP Information For Listener")
 
17. Provide the Share name you have configured for the Synchronization process. In this example we are using: \MyServerSQLCReplica, and click 'Next'.
Note: The replica share has to have the correct permissions applied to it. You will need to add the following objects as both Full Control for the folder level, and for the Advanced Share Permissions:
a. SQL Database Service Account
b. SQL Agent Service Account
c. SQLC-01$ AD Server Object
d. SQLC-02$ AD Server Object

![Configure Data Synchronization Preference]( https://mikesdatawork.files.wordpress.com/2015/03/image038.jpg "Configure Data Synchronization Preference")
 
18. Click 'Next' for the Validation.

![Validate Click Next]( https://mikesdatawork.files.wordpress.com/2015/03/image039.jpg "Validate Click Next")
 
19. Click 'Finish' for the summary.

![Click Finish]( https://mikesdatawork.files.wordpress.com/2015/03/image040.jpg "Click Finish")
 
Monitor the progress if necessary, but the process is complete at this stage.

![Monitor Progress]( https://mikesdatawork.files.wordpress.com/2015/03/image041.jpg "Monitor Progress")
 
You may see an informational message stating "The current WSFC cluster quorum vote configuration is not recommended for this availability group". This is simply a recommendation and by no means is this configuration not supported. Click 'OK' to finish.

![Click OK On Quorum Recommendation]( https://mikesdatawork.files.wordpress.com/2015/03/image042.jpg "Click Ok On Quorum Recommendation")
 
Naturally; you should test your AlwaysOn configuration to ensure they are working properly. 


[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Author

[![Gist](https://img.shields.io/badge/Gist-MikesDataWork-<COLOR>.svg)](https://gist.github.com/mikesdatawork)
[![Twitter](https://img.shields.io/badge/Twitter-MikesDataWork-<COLOR>.svg)](https://twitter.com/mikesdatawork)
[![Wordpress](https://img.shields.io/badge/Wordpress-MikesDataWork-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

     
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Mikes Data Work](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_02.png "Mikes Data Work")

