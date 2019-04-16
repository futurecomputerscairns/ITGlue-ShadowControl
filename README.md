# Sync important backup and replication information from ShadowControl with ITGlue

This script was the first written with 'automated documentation' in mind, with the goal of not ever having to document a site manually in ITGlue ever again. Along with the awesome work by [GCITS](https://github.com/GCITS/knowledge-base/tree/master/ITGlue), this is slowly becoming a reality.

![Screenshot of ITGlue](https://i.imgur.com/wC52wea.png)
![Screenshot of ShadowProtect](https://i.imgur.com/jdyIDFK.png) ![Screenshot of ImageManager](https://i.imgur.com/wgsXEcP.png)

This script compliments the information that is available from ShadowControl via API, and formats it in a way that it can be sent to ITGlue. Then, it will poll through customer you have in ITGlue, grab the Configuration items for that customer, and perform a match on the information gathered from ShadowControl. The matched data is either created if it does not already exist, or updated if it does.

# How to run the script

1. First you need to create the ShadowControl flexible asset type in ITGlue. This is done through Account > Flexible Asset Types > + New

The fields you will require are as follows, other fields (such as Icon) can be changed as you wish:
```
1.
name                   : Protected Server
kind                   : Tag
tag-type               : Configurations
show-in-list           : True

2.
name                   : StorageCraft Product
kind                   : Select
options                : ShadowProtect
                         ShadowProtect SPX
                         ShadowProtect SPX (Backup Agent)
                         ImageManager
show-in-list           : True

3.
name                   : Shadowprotect Version
kind                   : Text
show-in-list           : True

4.
name                   : Backup Name
kind                   : Text
show-in-list           : True

5.
name                   : Backup Destination
kind                   : Text
show-in-list           : True

6.
name                   : Backup Mode
kind                   : Text
show-in-list           : True

7.
name                   : Backup Schedule
kind                   : Text
show-in-list           : True

8.
name                   : ImageManager Version
kind                   : Text
show-in-list           : True

9.
name                   : ImageManager Managed Folders
kind                   : Textbox
show-in-list           : True

10.
name                   : ImageManager Replication
kind                   : Text
show-in-list           : True

11.
name                   : ImageManager Replication Jobs
kind                   : Textbox
show-in-list           : True
```

2. Now you need to edit line #41 of the script in a text editor and replace the ID with your own. To find this, the new flexible asset type needs to be added to your sidebar (Account > Settings > Customize Sidebar - Drag your new asset type from 'Flexible Assets to the 'Apps & Services' bar under 'Sidebar Sections'). Dont forget to hit Save right down the bottom. Now, return to a customer and click on your new asset in the sidebar. The URL will now show the asset type ID, as shown below:

![assettypeID](https://i.imgur.com/A3s67d2.png)

3. You will now need to create an API key in ITGlue, for the script to be able to interact with ITGlue. You will not need Password Access enabled for this API key.

   To generate a new API key:
```
1. Users with an Administrator role can generate the API key by visiting their Account > Settings in IT Glue.
2. Under the API Keys > Custom API Keys, give the key a name and then generate it. Note that you can't view a key again after it has been generated. 
```

4. Finally, you need to create an API token for ShadowControl, with the [Storagecraft guide found here.](https://support.storagecraft.com/s/article/configuring-users-and-access?language=en_US#node_29429)



Running the script is as follows:
```powershell
.\ITGlueShadowcontrol.ps1 -key [ITGlueAPIKeyHere] -sckey [ShadowControlTokenHere] -hostname [ShadowControlInternal/ExternalHostnameHere]

#Example

.\ITGlueShadowcontrol.ps1 -key ITG.123456789ABCDEFGHIJ -sckey 12345678910 -hostname shadowcontrol.contoso.com

