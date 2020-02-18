--- 
lab: 
    title: 'Lab: Configure Security and Compliance for teams and content '
    type: 'Answer Key' 
    module: 'Module 02: Implement Microsoft Teams Governance, Security and Compliance' 
---

# Lab 02: Configure Security and Compliance for teams and content
# Student lab answer key
## Lab Scenario  

In the labs of this course you will assume the role of Joni Sherman, a System Administrator for Contoso Ltd. Your organization is planning to deploy Microsoft Teams. Before starting the deployment, IT department is gathering business requirements about Teams governance as well as data security and compliance, including how the data shared in Teams be regulated according to the organization's compliance requirements. After you complete the planning process, you will configure Office 365 Groups governance, protect Teams from threats, and configure Teams to meet your organization compliance requirements.

## Objectives

After you complete this lab, you will be able to:

- Create classification labels
- Configure expiration policies
- Restrict creation of new teams to members of a security group
- Create naming policies
- Reset all Azure AD settings to defaults
- Activating ATP protection for SharePoint, OneDrive and Teams
- Configure retention policies
- Create a DLP policy to protect GDPR content

## Lab Setup  
- **Estimated Time:** 90 minutes.

## Instructions
### Exercise 1: Implement Governance and Lifecycle Management for Microsoft Teams

Your organization has started the planning process for Microsoft 365 services adoption. You are assigned as a Teams admin role to plan Teams governance. Since Teams relies on Office 365 groups, you need to plan governance procedures for Office 365 groups, including creating and configuring Office 365 groups classification labels, creating Office 365 groups expiration policies, configuring Office 365 Group creation policy permissions, and configuring Office 365 Groups naming policies.

#### Task 1 - Create classification labels
You need to evaluate governance of Office 365 Groups before deploying them in your organizations. One of the tasks is to add information about the group purpose. You will create classification labels in order to inform users what type of documents are stored within the group or what type of data is inside the email exchange within the group. In this task, you will create three classifications “Standard, Internal and Confidential”. For each of them, you will create appropriate classification desriptions "Standard: General communication, Internal: Company internal data, Confidential: Data that has regulatory requirements"

1. Connect to the **Client 1 VM** with the credentials that have been provided to you.

2. On the taskbar at the bottom of the page, right click the **Start** button and then select **Windows PowerShell (Admin)**.

3. Confirm the User Account Control window with **Yes**. 

4. In the PowerShell window, enter the following to install the Azure AD Preview module:
```Install-Module AzureADPreview```

5. When you are prompted to install from the Untrusted repository, also confirm by entering **Y** and pressing Enter.

6. Type in the following cmdlet to connect to Azure AD in your tenant:
```Connect-AzureAD```

7. A **Sign in** dialog box will open. Sign in as **admin@YourTenant.onmicrosoft.com** using the O365 Credentials provided to you.

8. To add classification descriptions for unified groups on the directory level, load the unified group template into a variable and modify it in the next steps. To load the unifed group template, use the following cmdlet:
	```powershell
	$Template = Get-AzureADDirectorySettingTemplate | Where {$_.DisplayName -eq "Group.Unified"}
	```

9. Check if a Azure AD setting is already existing and load it, if yes. If not, create a blank Azure AD setting object. Run the following cmdlet to populate the “$Setting” variable:

	```powershell
	if (!($Setting=Get-AzureADDirectorySetting|Where {$_.TemplateId -eq $Template.Id})) {$Setting = $Template.CreateDirectorySetting()}
	```

10. Modify the “ClassificationList” setting from the setting object variable by using the following cmdlet:
	```powershell
	$Setting["ClassificationList"] = "Standard, Internal, Confidential"
	```
11. Assiciate meaningful descriptions to each classification, by using the following cmdlet:
	```powershell
	$Setting["ClassificationDescriptions"] = "Standard: General communication, Internal: Company internal data, Confidential: Data that has regulatory requirements"
	```
12. To verify the classifications and calssificationdescriptions values, run the following cmdlet:
	```powershell
	$Setting.Values 
	```
13. As soon as the “Setting” variable attributes contain the desired values, write back the settings object to your directory. Use the following cmdlet, to create a new “Group.Unified” Azure AD configuration with the custom settings:

	```powershell
	New-AzureADDirectorySetting -DirectorySetting $Setting
	```
	**Note:** Since this is a new tenant, there’s no directory settings object in the tenant yet. You need to use New-AzureADDirectorySetting to create a directory settings object at the first time. 

	If there’s an existing directory settings object, you will need to use following cmdlet to update the directory setting in Azure Active Directory:

	```powershell
	Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
	```
 
14. Close the PowerShell window.

In this task, you have created classifications and classification descriptions for the Office 365 Groups, that will be used as Microsoft Teams classifications.

#### Task 2 - Assign classification labels
Once the classification label and descriptions are created, users can now assign them to the teams. Furthermore, users can modify existing classifications if needed. In this task, you will assign the “Confidential" classification to to the “Sales" team.

1. Connect to the **Client 2 VM** with the credentials that have been provided to you.

2. Open Microsoft Edge, maximize the window and navigate to the **Microsoft Teams** home page by entering the following URL in the address bar: [**https://teams.microsoft.com/**](https://teams.microsoft.com/)

3. When you see the **Pick an account** window select [**lynner@YourTenant.onmicrosoft.com**](mailto:lynner@yourtenant.onmicrosoft.com) and sign in with the provided credentials.

4. On the Microsoft Teams landing page click **Use the web app instead**

5. On the Teams overview select the three dots (**…**) right next to **Sales**, then select **Edit team** from the dropdown list.
![Edit Sales Team](media/M02-EditSalesTeam.png)

6. On the lower end of the **Edit "Sales" team** window, note the red message saying **Classification must be updated to save changes**. Select **Change setting**. 
![Update Classification](media/M02-UpdateClassification.png)

7. In the new Classification dropdown menu, select **Confidential.** 

8. Move the cursor and hover over the icon (**i)** right next to the **Classification** dropdown menu and note the classification descriptions.
![Classification Descriptions](media/M02-ClassificationDescriptions.png)

9. Select **Done** to save the changes.

10. Close the Edge Browser window.

You have successfully applied a classification to an existing team. Continue wit the next task.

#### Task 3 - Create and assign expiration policy    
Based on the organization requirement, unneeded groups should be deleted automatically after 90 days. To evaluate the group expiration policy experience, you will configure an expiration policy, that will delete the **Teams Rollout** group after a 90 days.

1. Connect to the **Client 1 VM** with the credentials that have been provided to you.

2. Open Microsoft Edge, maximize the browser, and navigate to the **Azure Portal**: [https://portal.azure.com](https://portal.azure.com/). 

3. Sign in with the global admin credential (**admin@YourTenant.onmicrosoft.com**).

4. If an Azure Advisor recommendations window is displayed, close it with the **X**.

5. In the **Microsoft Azure portal**, below the **Azure services** section, select **Azure Active Directory** ( You might need to select **More services** to see the option).

6. In the **Azure Active Directory**, on the left navigation pane, select **Groups.**

7. On the **Groups - All groups** page**,** on the left navigation pane, select **Expiration.**

8. In the **Groups - Expiration** page, use the **Group lifetime (in days)** drop-down box, and choose **Custom**.

9. In the box right from **Custom,** type **90**.

10. In the **Email contact for groups with no owners** field, type **JoniS@YourTenant.onmicrosoft.com**.

11. In the Field **Enable expiration for the Office 365 groups**, select the **Selected** button, and then select  **+** **Add** button to open a right-side pane.

12. In the **Select groups** pane, type **Teams Rollout** into the textbox and selec the group.

13. Use the **Select** button on the lower end of the right-side pane to apply the policy to the **Selected group**.

14. Back on the **Groups - Expiration** page, select **Save**.

15. Close the Edge Browser window.

You have successfully created a new expiration policy and configured the **Teams Rollout** team to expire after 90 days. If the team won’t have a owner after 90 days, Joni Sherman is notified about the expiration if the team.

#### Task 4 - Configure group creation policy    

You are an administrator for your Teams organization. You need to limit which users are able to create Office 365 groups. You will create a security group named **GroupCreators** which only the members of the group are allowed to create Office 365 groups.

1. Connect to the **Client 1 VM** with the credentials that have been provided to you.

2. On the taskbar at the bottom of the page, right click the **Start** button and then select **Windows PowerShell**.

3. Connect to the Azure AD in your tenant with the following cmdlet:
```Connect-AzureAD```

4. A Sign in dialog box will open. Sign in as **admin@YourTenant.onmicrosoft.com** using the O365 Credentials provided to you.

5. Create a new security group “GroupCreators” with the following cmdlet:
	```powershell
	New-AzureADGroup -DisplayName “GroupCreators” -SecurityEnabled:$true -MailEnabled:$false -MailNickName “GroupCreators”
	```

6. Replace **&lt;ObjectId&gt;** with the ObjectId from the output of the previous step and run following cmdlet to add **Lynne Robbins** to the new security group:
	```powershell
	Add-AzureADGroupMember -ObjectId **&lt;ObjectId&gt;** -RefObjectId (Get-AzureADUser -SearchString “Lynne Robbins”).ObjectId
	```

7. Run following cmdlet to fetch the unified group template again and load it into the “$template” variable:
	```powershell
	$Template = Get-AzureADDirectorySettingTemplate | Where {$_.DisplayName -eq "Group.Unified"}
	```

8. Run following cmdlet to check if a Azure AD setting is already existing and load it, if existing. If not, create a blank Azure AD setting object and populate the “$Setting” variable:
	```powershell
	if (!($Setting=Get-AzureADDirectorySetting|Where {$_.TemplateId -eq $Template.Id})) {$Setting = $Template.CreateDirectorySetting}
	```

9. Run following cmdlet to modify the group creation setting for your tenant with the “EnableGroupCreation” attribute:
	```powershell
	$Setting["EnableGroupCreation"] = “False”
	```

10. Run following cmdlet to add the just created security group “GroupCreators” as permitted group to create groups, by their ObjectID:
	```powershell
	$Setting["GroupCreationAllowedGroupId"] = (Get-AzureADGroup -SearchString “GroupCreators”).objectid
	```

11. Write back the chaned settings object to your Azure AD tenant, by using the following cmdlet:
	```powershell
	Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where {$_.DisplayName -eq "Group.Unified"}).id -DirectorySetting $Setting
	```

12. To test the newly configured settings, connect to the **Client 2 VM** with the credentials that have been provided to you.

13. Open a Edge browser window and navigate to the **Microsoft Teams web client** page by entering the following URL in the address bar: [**https://teams.microsoft.com/**](https://teams.microsoft.com/)**.**

 

14. On the Pick an account window, select **MeganB@YourTenant.OnMicrosoft.com** and sign in with her credentials.

15. Select **Join or create a team** from the lower end of the teams overview and you won’t see the option to **Create team**, resp. when you try to create a team, you will receive an error message.

16. Close all open windows.

In this task, you have succerssfully created a security group and configured Azure AD settings to restrict the creation of new groups to members of this security group only. At the end of the task, you have successfully tested the new group creation restrictions.

#### Task 5 - Configure a new naming policy  
As part of your Teams planning project, you will configure the naming policy where each new Office 365 Group or Team needs to comply with the organization’s regulations on naming objects. Each group name should start with letters **Group** and end with the **Country** attribute. Furthermore, there is an internal regulation that forbids using following specific keywords in Teams names: CEO, Payroll and HR. 

1. Connect to the **Client 1 VM** with the credentials that have been provided to you.

2. Open Microsoft Edge, maximize the browser, and navigate to the **Azure Portal**: [https://portal.azure.com](https://portal.azure.com).

3. When you see the **Pick an account** window, select **admin@YourTenant.onmicrosoft.com** and sign in.

4. In the **Microsoft Azure portal**, under **Azure services** section, select **Azure Active Directory**.

5. In the **Azure Active Directory**, on the left navigation pane, select **Groups.**

6. On the **Groups - All groups** page, on the left navigation pane, select **Naming policy.**

7. Select **Download** in the main window to download a blocked words sample file. **Save** the file and select **Open folder**.

8. Right-click the file and select **Edit** to open **Notepad**.

9. Type **CEO,Payroll,HR** into the Notepad window and save the file in place. Afterwards, close the Notepad file.

10. Back to the **Groups - Naming policy** page, under **Blocked words** section, at **Step 3. Upload your .csv file**, select the **folder** icon, then browse for **BlockedWords.csv** file, which is located in your **Downloads** folder, and select **Open.** 

11. On the **Group - Naming policy** page, select **Save**. After this step **BlockedWords.csv** file that contains blocked words be uploaded to the naming policy.

12. On the **Groups - Naming policy** page, select **Group naming policy** section from the top pane.

13. Under the **Group naming policy** section, select the checkbox next to **Add prefix** and from the drop-down menu, choose string, and in the empty box to the right, type **“****Group**“.

14. Under the **Group naming policy** section, select the checkbox next to **Add suffix** and from the drop-down list, choose attribute, and from the drop-down list choose **CountryOrRegion**.

15. On the **Groups - Naming policy** page, under **Current policy** section, preview the group name format listed as **Group&lt;Group name&gt;&lt;CountryOrRegion&gt;**.
![GroupNaming Policy](media/M02-GroupNamingPolicy.png)

16. Since you tested naming policy for evaluation, select on the **Groups - Naming policy** page, select **Discard** and confirm with **Yes**.

In this task, you have configured a naming policy that will block specific words to be used in an Office 365 Group name, as well as you have evaluated the options for prefix and suffix of the Office 365 Group name.

#### Task 6 – Remove the changed Azure AD settings again  
You can revert the Azure AD settings changes to defaults with following steps.

1. Connect to the **Client 1 VM** with the credentials that have been provided to you.

2. On the taskbar at the bottom of the page, right click the **Start** button and then select **Windows PowerShell**.

3. Connect to the Azure AD in your tenant with the following cmdlet:

```Connect-AzureAD```

4. A Sign in dialog box will open. Sign in as **admin@YourTenant.onmicrosoft.com** using the O365 Credentials provided to you.

5. To load the unifed group template, use the following cmdlet:
	```powershell
	$Template = Get-AzureADDirectorySettingTemplate | Where {$_.DisplayName -eq "Group.Unified"}
	```
6. Create a blank Azure AD tenant settings object:
	```powershell
	$Setting = $Template.CreateDirectorySetting()
	```
7. Check the Azure AD tenant settings configured in the template:
	```powershell
	$Setting.Values
	```
8. Check the current configured Azure AD tenant settings and note the differences, to the values in the “$Setting” variable, that contains the default template settings:
	```powershell
	(Get-AzureADDirectorySetting).Values
	```
9. Apply the default settings, to revert all changes:
	```powershell
	Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where {$_.DisplayName -eq "Group.Unified"}).id -DirectorySetting $Setting
	```
10. Check your configured Azure AD tenant settings again, which are all on default again:
	```powershell
	(Get-AzureADDirectorySetting).Values 
	```
11. Close the PowerShell window.

You have successfully reset all Azure AD tenant settings in your test tenant. This is the end of exercise 1.

### Exercise 2: Implementing security for Microsoft Teams
In this exercise, you will increase the security level in your organization by configuring an ATP policy to ensures no malicious content is sent through documents shared in Teams by blocking attachments that contain malware. 

#### Task 1 - Configure ATP for Microsoft Teams 
Users in your organization are using Microsoft Teams for communication and collaboration. Business managers are concerned that documents that are shared within Microsoft Teams may contain malware. You will need to ensure that no malicious content is sent through documents shared in Teams by configuring ATP policy that blocks documents that contain malware. 

1. Connect to the **Client 1 VM** with the credentials that have been provided to you.

2. Open Microsoft Edge, maximize the browser, and navigate to the **Microsoft** **Security admin center**: [**https://security.microsoft.com**](https://security.microsoft.com). 

3. When you see the **Pick an account** window, select **admin@YourTenant.onmicrosoft.com** and sign in.

4. In the **Microsoft 365 security** in the left navigation pane, select **Policies**.

5. On the **Policies** page, scroll to the **Threat Protection** section and select **ATP safe attachments (Office 365).**

6. A new browser tab with the **Office 365 Security & compliance center** will open.

7. On the **ATP safe attachments** page, select the check box next to **Turn on ATP for SharePoint, OneDrive, and Microsoft Teams**, and then select the **Save** button.

8. Close the Edge browser window.

In this task, you have activated ATP safe attachments scanning for SharePoint, OneDrive, and Microsoft Teams that blocks block documents that contain malware.

### Exercise 3: Implementing Compliance for Microsoft Teams
Before deploying Microsoft Teams in your organization, you need to evaluate Microsoft Teams compliance features to meet organizations requirements. First, you will configure retention settings on data in Microsoft Teams. Next you will configure DLP policy that will search for all GDPR related data. At the end you will perform scoped directory search.

#### Task 1 - Create a new retention policy for a single team  
Before deploying Microsoft Teams in your organization, you will need to evaluate Microsoft Teams retention settings. You will create a new retention policy that retains the content of the "Sales" Team for 7 years after last modification. 

 
1. Connect to the **Client 1 VM** with the credentials that have been provided to you.

2. Open Microsoft Edge, maximize the browser, and navigate to the **Office** **365** **Security &amp; Compliance center**: [**https://protection.office.com**](https://protection.office.com)

3. When you see the **Pick an account** window, select **admin@YourTenant.onmicrosoft.com** and sign in.

4. In **Office 365 Security &amp; Compliance center**, on the left navigation pane, select **information governance**, and then choose **Retention.**

5. On the **Retention** page, select **Create**, and then on the **Name your policy page, i**n the name box, type **Sales retention policy.** In the **Description** box**,** type **Retention policy for Sales department that will retain data for 7 years**, and then select **Next**.

6. On the **Decide if you want to retain content, delete it, or both** pabe, select **Yes, I want to retain it, For this long…, 7 years**, and then choose **Retain the content based on** **when it was modified.** Under **Do you want us to delete it after this time?** select **No**, and then select, **Next**. 

7. On the **Chose locations** page, scroll down and select **Teams channel messages**, then click **Choose teams**. 

8. On the **Edit locations** page, select **Choose teams**, **and then** select the Team **Sales**, select **Choose** and at the end select **Done.**

9. On the **Choose locations page,** select **Next** and on **Review your settings** page, select **Create this policy.**

10. Leave the browser open for the next task.


In this this task, you have successfully created a new retention policy named **Sales retention policy** that retains the channel messages of the **Sales** Team for **7 years after the last modification**. 

#### Task 2 - Create a DLP policy for GDPR (PII) content from a template  
According to your organization compliance requirements, you need to implement basic protection of PII data for European users. You will create a new DLP Policy named **GDPR DLP Policy** from the template "General Data Protection Regulation (GDPR)". The DLP policy you create will detect if GDPR sensitive content is shared with people outside of your organization. If the policy detects at least one occurrence of the GDPR sensitive information, it will send email to the Global Admin and block people from sharing the content and restricting access to shared content. Furthermore, it will display a tip to users who tried to share the sensitive content and it will allow them to override the policy with business justification. Since you are evaluating the DLP policies, you will create the DLP policy in a test mode with policy tips enabled.

1. Connect to the **Client 1 VM** with the credentials that have been provided to you.

2. Open Microsoft Edge, maximize the browser, and navigate to the **Microsoft** **C****ompliance center**: [**https://compliance.microsoft.com**](https://compliance.microsoft.com)**.**

3. You are still signed in as [**admin@YourTenant.onmicrosoft.com**](mailto:admin@yourtenant.onmicrosoft.com). When you see the **Pick an account** window, select **admin@YourTenant.onmicrosoft.com** and sign in.

4. In **Microsoft** **Compliance Center**, on the left navigation pane, select **Show all** and and select **Data loss prevention**.

5. On the Data loss prevention page, select **+ Create a policy**

6. On the **New DLP policy** page, select the searchbox and enter **GDPR.** Select the **General Data Protection Regulation (GDPR)** and select **Next**.

7. On the **Name your policy** page, change the name to **GDPR DLP Policy,** in the Description box type **Data loss prevention policy for GDPR regulations** **in Teams** and then select **Next**.

8. On the **Choose locations** page, select **Let me choose specific locations** and select **Next**.

9. Uncheck **Exchange email**, **SharePoint sites** and **OneDrive accounts**. Leave **Teams chat and channel messages** turned on. Select **Next**.

10. On the **Customize the type of content you want to protect** page, ensure that default option is selected **Find content that contains,** select the checkbox next to **Detect when this content is shared,** and from the drop-down menu, choose **with people outside my organization.** At the end, select **Next**.

11. On the **What do you want to do if we detect sensitive info?** page, ensure that following settings are configured:

	- A checkbox is selected for **Send incident reports in email.**

	- A checkbox is selected for **Detect when content that's being shared contains.** In the **instances of the same sensitive info** **type** box, type **1**.

	- A checkbox is selected next to **Restrict access or encrypt the content** and **Block people from sharing and restrict access to shared content** radio button is selected.

12. On the **Customize access and override permissions** page, ensure that following settings are configured:

	- Select **Only people outside your organization.**

	- Turn **On** the setting for **Let people who see the tip override the policy**.

	- Select the check box next to **Require a business justification to override**, and then select **Next**.

13. On the **Do you want to turn on the policy or test things out first?** page, select **I'd like to test it out first,** then select check box next to **Show policy tips while in test mode,** and at the end select **Next**.

14. On the Review your settings page, select **Create.**

15. Leave the browser open for the next task.


After completing this task, you have created a DLP Policy from the template "General Data Protection Regulation (GDPR)" that detects if GDPR sensitive content is shared with people outside of your organization.
