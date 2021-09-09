---
lab:
    title: 'Lab 02: Configure security and compliance for Microsoft Teams'
    type: 'Answer Key'
    module: 'Module 2: Implement security and compliance for Microsoft Teams'
---

# **Lab 02: Configure security and compliance for Microsoft Teams**

# **Student lab answer key**

## **Lab Scenario**

In the labs of this course, you will assume the role of the Global Administrator for Contoso Ltd. Your organization is planning to deploy Microsoft Teams. Before starting the deployment, IT department is gathering business requirements about data security and compliance, including how the data shared in Teams be regulated according to the organization’s compliance requirements. After you complete the planning process, you will protect Teams from threats, and configure Teams to meet your organization compliance requirements.

## **Objectives**

After you complete this lab, you will be able to:

- Configure guest access in Azure and Teams

- Review Access to a resource

- Activate, create and assign sensitivity labels

- Activating Safe Attachments for SharePoint, OneDrive and Teams

- Create, configure and test retention policies

- Create and test a DLP policy to protect GDPR content

## **Lab Setup**

- **Estimated Time:** 90 minutes.

## **Instructions**

### **Exercise 1: Manage guest access for Microsoft Teams**

In this exercise, you will test the guest access features in Microsoft 365. To do so, you will configure guest access in Azure AD, add a new external guest user and revoke the guest access by using access reviews.



#### Task 1 - Review guest access settings (optional)

1. Connect to the **Client 1 VM** and browse to Azure AD admin center (https://aad.portal.azure.com/) as **MOD Administrator**. 

2. In left navigation of the Azure AD admin center, select **Users** > **User settings** > **Manage external collaboration settings**. Review the following settings for external users at the Azure AD level:

	* Guest user access: Guest users have limited access to properties and memberships of directory objects.

	* Guest invite settings: Anyone in the organization can invite guest users including guests and non-admins (most inclusive).

	* Collaboration restrictions: Allow invitations to be sent to any domain (most inclusive)

3. Browse to Microsoft 365 admin center (https://admin.microsoft.com/) as **MOD Administrator**.

4. In left navigation of the Microsoft 365 admin center, select **Settings** > **Org settings**.

	1. Under the **Services** tab, select **Microsoft 365 Groups**. Make sure the checkbox is selected for **Let group owner add people outside your organization to Microsoft 365 Groups**.

	2. Under the **Security & privacy** tab, select **Sharing**. Make sure the checkbox is selected for **Let users add new guests to the organization**.


You have now review guest access settings across different admin centers. You are ready to invite guest for collaboration. 


#### Task 2 - Configure guest access in Teams

Now that you have explored the Teams admin center it is time to configure the first setting. Since this task will take some time to replicate through the tenant, you will configure the guest user access for Microsoft Teams right now, so it is available for later use.

1. Connect to the **Client 1 VM** and browse to Teams admin center (https://admin.teams.microsoft.com) as **Joni Sherman**  (JoniS@&lt;YourTenant&gt;.onmicrosoft.com).

2. In left navigation of the Teams admin center, select **Org-wide settings**. 

4. Select **Guest access** from the list.

5. On the **Guest access** page, check if **Allow guest access in Teams** is enabled. If not, select **On**.

6. Under **Messaging** section, disable **Delete sent messages**

7. Select **Save**.

You have now successfully activated guest access and disallow guests to delete their sent messages for Teams in your tenant.

#### Task 3 - Add a guest to a team
In this task, you will add a guest user by inviting the guest to the team **Group_Afterwork_United States** you created from Lab 1. 

You will change the default settings for inviting/creating guest users and then add your personal Outlook.com account as a guest user to your tenant.

**Note**: You will need an Outlook.com account for this exercise. If you don’t have an outlook account like, you can create a new account from [**https://outlook.com**](https://outlook.com/).

1. Connect to the **Client 2 VM** and browse to the **Teams web client** (https://teams.microsoft.com/) as **Lynne Robbins** (LynneR@&lt;YourTenant&gt;.onmicrosoft.com)

2. Add the guest to **Group_Afterwork_United State** team.
	
	1. Select **Teams** > Select **...** next to the **Group_Afterwork_United State** team.

	2. Select **Add member** and enter your outlook account. 

	3. You will see a message **add &lt;Your outlook account&gt; as a guest**. Select the message and select **Add**. 

3. Accept the guest invite

	1. Open a **New InPrivate window** and check the email with subject **You have been added as a guest to Contoso in Microsoft Teams** from **Outlook Web Portal**(https://outlook.live.com/owa/).

	2. Select **Open Microsoft Teams** from the email. You will be redirected to the sign in page with permission consent request. 
	
	3. Select **Accept** and sign in to Teams web client with your outlook account. 

	4. From the Teams client, select **Teams**, you will see the team **Group_Afterwork_United State**.

4. Test the guest access

	1. Under the team **Group_Afterwork_United State**, select **General** channel, and send the message: **Hello!**.

	2. Select **...** of the message you just posted. Notice there's no **Delete** option. 

You have successfully invite a guest to a team and validate the guest access setting from previous task. 

#### Task 4 - Create access reviews

As a part of your system administrator role, you need to review access to resources in your tenant on a regular basis. You can do that by creating an access reviews.

1. Connect to the **Client 1 VM** and browse to Azure AD admin center (https://aad.portal.azure.com/) as **MOD Administrator**. 

2. Create an access review to monitor guest users.

	In left navigation of the Azure AD admin center, select **Identity Governance** > **Create an access review**. Follow the wizard with the following information:

	1. In **Step 1: Select what to review** section, select **Teams + Groups**.
	
	2. In **Step 2: Select which Teams + Groups** section, select **All Microsoft 365 groups with guest users.** 

	3. In **Step 3: Select review scope** section, select **Guest users only**. Select on **Next: Reviews**.

	4. In the **Select reviewers** section, select **Group owner(s)**.

	5. In the **Specify recurrence of review** section, select **Weekly** for **Review Recurrence** and keep rest as default. Select on **Next: Settings**.

	6. On the **Settings** tab, leave the settings as default. Select on **Next: Review+Create** > **Create**. 

3. Review the access review dashboard from Azure AD.

	1. On the **Identity Governance | Access reviews** page, you will see a access review report named **Review guest access across Microsoft 365 groups**

	2. When the **Status** of the report shows as **Active**, select on the name of the report - **Review guest access across Microsoft 365 groups**.

	3. On the **Review guest access across Microsoft 365 groups | Overview** page, select **Group_Afterwork_United States** under group name.

	4. On the **Group_Afterwork_United States | Overview** page, you can see there's one users show under **Not reviewed** category. 

4. Reviwew the access review and approve the guest user. 

	1. Connect to the **Client 2 VM** and browse to the **Outlook.com** (https://outlook.office.com/) as **Lynne Robbins** (LynneR@&lt;YourTenant&gt;.onmicrosoft.com)

	2. Check the email with the subject **Action required: Review group access**.

	3. Select **Start review >** in the content of the email. 

	4. From the **My Access** (Https://myaccess.microsoft.com) page, select **Review guest access across Microsoft 365 groups**. 

	5. On the **Review guest access across Microsoft 365 groups** page, select the guest account and select **Approve**. 
	
	6. From the **Approve continued access** window, enter **Approved.** to the textbox, and select **Submit**

You have successfully created an access review and approved a guest user in your tenant. 


### **Exercise 2: Implement security for Microsoft Teams**

In this exercise, you will increase the security level in your organization by configuring Safe Attachments to ensure that no malicious content is sent through documents shared in Teams by blocking attachments that contain malware.

#### Task 1 - Configure Safe Attachments for Microsoft Teams

Users in your organization are using Microsoft Teams for communication and collaboration. Business managers are concerned that documents that are shared within Microsoft Teams may contain malware. You will need to ensure that no malicious content is sent through documents shared in Teams by configuring Safe Attachments that blocks documents that contain malware.



1. Connect to the **Client 1 VM** and browse to Microsoft 365 Defender portal (https://security.microsoft.com/) as **MOD Administrator**. 

2. In left navigation of the Microsoft 365 Defender portal, select **Policies & rules** \> **Threat policies** \> **Policies** section \> **Safe Attachments**..

3. On the Safe attachments page, select **Global settings**.

4. In the Global settings fly out that appears, turn on the toggle under **Turn on Defender for Office 365 for SharePoint, OneDrive, and Microsoft Teams**.

5. Select **Save**.


In this task, you have activated Safe Attachments scanning for SharePoint, OneDrive, and Microsoft Teams that blocks block documents that contain malware.



### **Exercise 3: Implement compliance for Microsoft Teams**

Before deploying Microsoft Teams in your organization, you need to evaluate Microsoft Teams compliance features to meet organizations requirements. 

#### Task 1 – Activate sensitivity labels for Teams 

You need to evaluate governance for Microsoft 365 Groups before deploying them in your organizations. In this task, you will activate the sensitivity labels for Teams in Azure AD, for being able to assign labels to teams. 

1. Connect to the **Client 1 VM** with the credentials that have been provided to you.

2. Open **Windows PowerShell** and run as Administrator.

3. Connect to your AAD tenant.

    Enter the following cmdlet in the PowerShell window and press **Enter**. In the Sign in window, sign in as the Global admin - MOD Administrator(admin@&lt;YourTenant&gt;.onmicrosoft.com).

    ```powershell
    Connect-AzureAD
    ```
4. Fetch the current group settings for the Azure AD organization

   ```powershell
   $Setting = Get-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id
   ```

5. Enable the Microsoft Identity Protection (MIP) support in your configuration:
    ```powershell
    $Setting["EnableMIPLabels"] = "True"
    ```
6. To verify the new configuration, run the following cmdlet:
    ```powershell
    $Setting.Values
    ```
7. Then save the changes and apply the settings:

	```powershell
	Set-AzureADDirectorySetting -Id $Setting.Id -DirectorySetting $Setting
	```
	**Note:** If there’s no directory settings object in the tenant yet. You need to use ```New-AzureADDirectorySetting``` to create a directory settings object at the first time.

8. Disconnects the current session from an Azure Active Directory tenant and close the PowerShell window.

    ```powershell
    Disconnect-AzureAD
    ```
	
You have successfully changed your tenants Azure AD settings and activated sensitivity labels for Microsoft 365 Groups and Microsoft Teams.
   
#### Task 2 - Create sensitivity labels for Teams

After activating sensitivity labels for groups, you will now create three sensitivity labels. In this task, you will create three sensitivity labels **General**, **Internal**, and **Confidential**.  For each of them, you will create appropriate user and admin descriptions.

1. Connect to the **Client 1 VM** and browse to Microsoft 365 compliance center (https://compliance.microsoft.com/) as **MOD Administrator**.

2. In left navigation of the Microsoft 365 admin center, select **Information protection**.

3. Select **Turn on now** next to the following warning message to activate content processing in Office online files.

	*Your organization has not turned on the ability to process content in Office online files that have encrypted sensitivity labels applied and are stored in OneDrive and SharePoint. You can turn on here, but note that additional configuration is required for Multi-Geo environments. Learn more*

4. Create the first sensitivity label - **General**

	Select **+ Create a label** and follow the wizard with the following information: 
	
	1. In the **Name &description** section, enter the following information:
		- **Name**: General
		- **Display name**: General
		- **Description for users**: General information without protection.
		- **Description for admins**: General information without encryption, marking or sharing restriction settings activated.

	2. In the **Scope** section, select **Files &amp; emails** and **Groups &amp; Sites** 

	3. In the **Files & emails** and **Auto-labeling** sections, leave the settings as default.
	
	4. In the **Groups & sites** section, select both checkboxes. 
	
		* **Privacy and external user access settings** 
		* **External sharing and Conditional Access settings** 

	5. In the **Privacy & external user access** section, select **None** and check the checkbox of **Let Microsoft 365 group owners add people outside the organization to the group**. 

	6. In the **External sharing & device access** section
	
		* Select **Control external sharing from labeled SharePoint sites** > **Anyone**.

		* Select **Use Azure AD Conditional Access to projtect labeled SharePoint sites** >  **Allow full access from desktop apps, mobile apps, and the web**.

	7. In the **Azure Purview assets** section, leave the settings as default. 

	8. Select **Create label** > **Done**.

5. Create the second sensitivity label - **Internal**

	Select **+ Create a label** and follow the wizard with the following information: 
	
	1. In the **Name &description** section, enter the following information:
		- **Name**: Internal
		- **Display name**: Internal
		- **Description for users**: Internal information with sharing protection
		- **Description for admins**: Internal information with moderate encryption, marking and sharing restriction settings activated

	2. In the **Scope** section, select **Files &amp; emails** and **Groups &amp; Sites** 

	3. In the **Files & emails** section, select both checkboxes. 
		* **Encrypt files and emails**
		* **Mark the content of files**
	
	4. In the **Encryption** section, 

		* Select **configure encryption settings**
		* Assign permissions now or let users decide: **Assign permissions now**.
		* User access to content expires: **Never**.
		* Allow offline access: **Always**.
		* Select **Assign permissions**, and select **+ Add all users and groups in your organization**.

	5. In the **Content marking** sections, 

		* Select the slider and the checkbox **Add a watermark**.
		* Select **Customize text** and enter the following to the **Watermark text** box: **Internal use only**

	6. In the **Auto-labeling** sections, leave the settings as default.
	
	7. In the **Groups & sites** section, select both checkboxes. 
	
		* **Privacy and external user access settings** 
		* **External sharing and Conditional Access settings** 

	8. In the **Privacy & external user access** section, select **None**. 

	9. In the **External sharing & device access** section
	
		* Select **Control external sharing from labeled SharePoint sites** > **Existing guests**.

		* Select **Use Azure AD Conditional Access to projtect labeled SharePoint sites** >  **Allow limited web-only access**.

	10. In the **Azure Purview assets** section, leave the settings as default. 

	11. Select **Create label** > **Done**.

6. Create the second sensitivity label - **Confidential**

	Select **+ Create a label** and follow the wizard with the following information: 
	
	1. In the **Name &description** section, enter the following information:
		- **Name**: Confidential
		- **Display name**: Confidential
		- **Description for users**: Confidential information with all protection
		- **Description for admins**: Confidential information with all restrictive encryption, marking and sharing settings activated

	2. In the **Scope** section, select **Files &amp; emails** and **Groups &amp; Sites** 

	3. In the **Files & emails** section, select both checkboxes. 
		* **Encrypt files and emails**
		* **Mark the content of files**
	
	4. In the **Encryption** section, 

		* Select **configure encryption settings**
		* Assign permissions now or let users decide: **Assign permissions now**.
		* User access to content expires: **Never**.
		* Allow offline access: **Never**.
		* Select **Assign permissions**, and select **+ Add all users and groups in your organization**.

	5. In the **Content marking** sections, 

		* Select the slider and the checkbox **Add a watermark**.
		* Select **Customize text** and enter the following to the **Watermark text** box: **Confidential.**

	6. In the **Auto-labeling** sections, leave the settings as default.
	
	7. In the **Groups & sites** section, select both checkboxes. 
	
		* **Privacy and external user access settings** 
		* **External sharing and Conditional Access settings** 

	8. In the **Privacy & external user access** section, select **Private**. 

	9. In the **External sharing & device access** section
	
		* Select **Control external sharing from labeled SharePoint sites** > **Only people in your organization**.

		* Select **Use Azure AD Conditional Access to projtect labeled SharePoint sites** >  **Block access**.

	10. In the **Azure Purview assets** section, leave the settings as default. 

	11. Select **Create label** > **Done**.

7. Publish sensitivity labels

	1. On the **Information protection** page, select **Publish labels** from the top menu.

	2. In the **Labels to publish** section, select **Choose sensitivity labels to publish**. Select all of the labels and select **Add**.

	3. In the **Users and groups** section, keep the default settings. 

	4. In the **Settings** section, keep the default settings. 

	5. In the **Document** section, select **General** in the dropdown menu **Apply this label by default to documents**.

	6. In the **Emails** section, select **General** in the dropdown menu **Apply this label by default to emails**. 

	7. In the **Sites and Groups** section, select **General** in the dropdown menu **Apply this label by default to groups and sites**.	

	8. In the **Name** section, enter the following:

		- **Name**: All company sensitivity labels

		- **Enter a description for your sensitivity label policy**: Default sensitivity labels for all users in the company.

	9. Select **Submit** > **Done**.
	
In this task, you have created and published three new sensitivity labels available for all users, which can be assigned to new and existing Teams.

#### Task 3 - Assign sensitivity labels to Teams

Once the sensitivity labels are created and published, users can now assign them to teams. Furthermore, users can modify assigned labels if needed. In this task, you will assign the "Internal" label to the "Teams Rollout" team.

**Note:** It can take several minutes till the newly created sensitivity labels are available to users.

1. Connect to the **Client 2 VM** with the credentials that have been provided to you.

2. Open the Teams Desktop client, where you are still signed in as **Megan Bowen**.

3. On the Teams overview select the **…** on the right side next to the Team "**Teams Rollout,"** then select **Edit team** from the dropdown list.

4. On the **Edit "Teams Rollout" team** window, select the dropdown menu below Sensitivity and select **Internal**.

5. Select **Done** to save the changes.

You have successfully applied a sensitivity labels to an existing team. The configured settings of the Internal label are now applied to the Teams Rollout team. Continue with the next task.


#### Task 4 – Test external access with sensitivity labels (optional)

Even with enabled guest access sensitivity labels can deny guest access for specific teams. In this task you will try to add a guest user to an internal team. Enabling guest access for teams can take up to 24 hours. If you cannot find the guest user in step 4 you should test it again the next day.

1. Connect to the **Client 2 VM** with the credentials that have been provided to you.

2. Open the Microsoft Teams Desktop Client, where you are signed in as **Megan Bowen**.

3. On the Teams overview select **…** right next to the Team "**Teams Rollout"** then select **Add member** from the dropdown list.

4. On the **Add members to Teams Rollout** page, enter the name of the guest user you just invited.

5. You will not be able to find the guest user, because guest users are restricted from this team.

6. Perform the steps 3 and 4 for the **Contoso** team, where you can find and add the guest user to the **Contoso** team.

7. Select **Close.**

**Note:** It can take up to 24 hours after enabling, till guest access is available in teams. If you cannot add guest users to any team, return to this task at a later point of this lab.

You have successfully tested the sensitivity labels setting to prevent guest access to a protected team and you can confirm, the labels are working as predicted.

#### Task 5 - Create a new retention policy to retain content

Teams retention settings are very important for managing the lifecycle of company data, therefore, the capabilities of retention policies need to be evaluated in the Teams pilot. In this task, you will create a new retention policy that retains the Teams channel messages of the "Sales" Team for 7 years after last modification.

1. Connect to the **Client 1 VM** with the credentials that have been provided to you.

2. Open **Microsoft Edge**, maximize the window and navigate to the **Microsoft 365 compliance center** at [**https://compliance.microsoft.com/**](https://compliance.microsoft.com/).

3. On the **Pick an account** page, select the **MOD Administrator** (admin@&lt;YourTenant&gt;.onmicrosoft.com) and sign in with the provided credentials.

4. In **Office 365 Compliance center**, on the left navigation pane, select **policies**, scroll down to **Data** and select **Retention**.

5. On the **Retention** page, select **New retention policy** to open the **Create retention policy wizard.**

6. On the **Name your policy** page, enter the following and select **Next**: 

	- **Name**: Sales retention policy

	- **Description**: Retention policy for Sales department that will retain channel messages for 7 years.

7. On the **Choose locations to apply the policy** page, configure the following settings:

	- **Exchange email**: Off

	- **SharePoint sites**: Off

	- **OneDrive accounts**: Off

	- **Office 365 groups**: Off

	- **Skype for Business**: Off

	- **Exchange public folders**: Off

	- **Teams channel messages**: On

	- **Teams chats**: Off

8. Select **Edit** right from **Teams channel messages** to open the right-side pane.

9. Select the checkbox left from **Sales** and select **Done**.

10. Select **Next**

11. On the **Decide if you want to retain content, delete it, or both** page, select **Next**.

12. On the **Review and finish** page, review your settings and select **Submit**.

13. Select **Done**. Leave the browser open for the next task.

In this this task, you have successfully created a new retention policy named **Sales retention policy** that retains the channel messages and chat of the **Sales** Team for **7 years after the last modification**.

#### Task 6 - Create a new retention policy to delete content

After configuring a retain policy to protect data from deletion, you also need to evaluate the capabilities of retention policies to delete content automatically. For demonstration purpose, you will set the deletion threshold to a single day and apply the retention policy to the "Teams Rollout" team, to remove all channel messages older than a day automatically.

1. Connect to the **Client 1 VM** with the credentials that have been provided to you.

2. You are still signed in to the **Microsoft 365 Compliance center**, as **MOD Administrator** in the **Information governance** section and on the **Retention** tab.

3. Select **New retention policy** to open the **Create retention policy wizard.**

4. On the **Name your policy** page, enter the following and select **Next**: 

	- **Name**: Teams Rollout deletion policy

	- **Description**: Retention policy for the Teams Rollout team to delete messages older than a day.

5. On the **Choose locations to apply the policy** page, configure the following settings and select **Next**:

	- **Exchange email**: Off

	- **SharePoint sites**: Off

	- **OneDrive accounts**: Off

	- **Office 365 groups**: Off

	- **Skype for Business**: Off

	- **Exchange public folders**: Off

	- **Teams channel messages**: On

	- **Teams chats**: Off

6. Select **Edit** right from **Teams channel messages** to open the right-side pane.

7. Select the checkbox left from **Teams Rollout** and select **Done**.

8. Select **Next**

9. On the **Decide if you want to retain content, delete it, or both** page, select **Only delete items when they reach a certian age** with the following information and then select **Next**:

	- Select **Delete items older than** drop down and then select **Custom**: 1 Day 

	- **Delete the content based on**: when it was created


10. On the **Review and finish** page, review your settings and select **Submit**.

11. Select **Done**. Leave the browser open for the next task.

You have successfully created a second retention policy for testing the deletion capabilities to clean up the "Teams Rollout" team from all conversation messages older than a day.

#### Task 7 – Test the retention policy for deleting content (optional)

In this task you will test the retention policy for deleting content from the Teams Rollout team after a day. Before you can see the retention policy taking any effect, you must create some conversation content in the team.

**Note:** Because you need to wait for 24 hours till the retention policy deletes anything, this task is marked as optional. After creating content in the Teams Rollout team, you need to return to this task after waiting 24 hours to see the retention policies effect.

1. Connect to the **Client 2 VM** with the credentials that have been provided to you.

2. Open the Teams Desktop client from the taskbar, where you are still signed in as **Megan Bowen**.

3. Select the **Teams Rollout** team and the **General** channel.

4. Select **New conversation** from the lower end of the main window.

5. Write the following text to the text box:

	Hello world!

6. Leave the client open and add other content to the team, as you like.

7. Come back after 24 hours to see, the content has been deleted automatically.

You have added a conversation message to a team, which is deleted by the deletion retention policy after 24 hours.

#### Task 8 - Create a DLP policy for GDPR (PII) content from a template

According to your organization compliance requirements, you need to implement basic protection of PII data for European users. You will create a new DLP Policy named **GDPR DLP Policy** from the template "General Data Protection Regulation (GDPR)," The DLP policy you create will detect if GDPR sensitive content is shared with people outside of your organization. If the policy detects at least one occurrence of the GDPR sensitive information, it will send email to Joni Sherman and block people from sharing the content and restricting access to shared content. Furthermore, it will display a tip to users who tried to share the sensitive content, and it will allow them to override the policy with business justification. Since you are evaluating the DLP policies, you will create the DLP policy in a test mode with policy tips enabled.

1. Connect to the **Client 1 VM** with the credentials that have been provided to you.

2. Open **Microsoft Edge**, maximize the window and navigate to the **Microsoft 365 compliance center** at [**https://compliance.microsoft.com/**](https://compliance.microsoft.com/).

3. On the **Pick an account** page, select the **MOD Administrator** (admin@&lt;YourTenant&gt;.onmicrosoft.com) and sign in with the provided credentials.

4. In **Microsoft 365 compliance center**, on the left navigation pane, select **Show all** from the bottom of the navigation pane and then select **Data loss prevention**.

5. On the **Data loss prevention** page, select the **Policies** tab, then select **+ Create policy**.

6. On the **Start with a template or create a custom policy** page, select the **Search for specific templates** search box and type: **GDPR**. Select **Privacy** under **Categories**, then select the **General Data Protection Regulation (GDPR)** template from the **Templates** section.

7. Select **Next**

8. On the **Name your DLP policy** page, change the default values to the following and select **Next**:

	- **Name**: GDPR DLP Policy

	- **Description**: Data loss prevention policy for GDPR regulations in Teams.

9. On the **Choose locations to apply the policy** page, apply the following selection and select **Next**:

	- **Exchange email**: Off

	- **SharePoint sites**: Off

	- **OneDrive accounts**: Off

	- **Teams chat and channel messages**: On

	- **Microsoft Cloud App Security**: Off

10. On the **Define** **policy settings** page, stay with the default selection from the template **Review and customize default settings from the template** and select **Next**.

11. On the **Info to protect** page, leave the default settings and select **Next**.

12. On the **Protection actions** page, ensure that the following settings are configured, and then select **Next**:

	- A checkbox is selected for: **Detect when a specific amount of sensitive info is being shared at one time**

		- In the **At least __ or more instances of the same sensitive info type** box, type: **1**

	- Select the checkbox for **Send incident reports in email**

	- Select **Choose what to include in the report and who receives it** to open the right-side pane

	- Select **Add or remove people**, select the checkbox for **Joni Sherman**

	- Select **Add** and **Save**

	- Select the checkbox for **Restrict access or encrypt the content**

13. On the **Customize access and override settings** page, ensure that the following settings are configured, and then select **Next**:

	- A checkbox is selected for: **Restrict access or encrypt the content in Microsoft 365 locations**

	- Select **Block users from accessing shared SharePoint, OneDrive, and Teams content**.

	- Select **Block only people outside your organization. Users inside your organization will continue to have access**.

	- Select **Override the rule automatically if they report it as false positive**.

14. On the **Test or turn on the policy** page, select **Turn it on right away** and select Next.

15. On the Review your settings page, review your settings, select **Submit** then **Done**.

16. Stay on the **Data loss prevention page** and leave the browser opened.

After completing this task, you have created a DLP Policy from the template "General Data Protection Regulation (GDPR)" that detects if GDPR sensitive content is shared with people outside of your organization. The policy is extra sensitive for the configured threshold of 1 rule match and Joni Sherman will be notified if a matching occurs.

#### Task 9 - Create a DLP policy from scratch

After creating a DLP Policy for protecting GDPR relevant data, you will create another policy from scratch. Instead of using a template, you will configure rules directly with custom rules and actions.

1. Connect to the **Client 1 VM** with the credentials that have been provided to you.

2. You are still signed in to the **Microsoft 365 Compliance center**, as **MOD Administrator** in the **Data loss prevention** section and on the **Policies** tab.

3. In **Microsoft 365 compliance center**, on the left navigation pane, select **Show all** and then select **Data loss prevention**.

4. On the **Data loss prevention** page, select **+ Create policy**.

5. Select **Custom** and **Custom policy** below **Categories** and **Templates**, to create a blank policy and select **Next**.

6. On the **Name your policy** page, type the following values, and then select **Next**:

	- **Name**: Credit card data DLP Policy

	- **Description**: Data loss prevention policy for credit card data in Teams.

7. On the **Choose locations to apply the policy** page, apply the following selection and select **Next**:

	- **Exchange email**: Off

	- **SharePoint sites**: Off

	- **OneDrive accounts**: Off

	- **Teams chat and channel messages**: On

	- **Microsoft Cloud App Security**: Off

8. Leave the radio button selection unchanged on the **Define policy settings** page and select **Next**.

9. Select **+ Create rule** and enter the following information:

	- **Name**: Credit card numbers found

	- **Description**: Basic rule for protecting credit card numbers form being shared in Teams.

10. Below **Conditions**, select **+ Add condition** and **Content contains**.

11. Leave the group name of **Default**, select **Add** and **Sensitive information types**.

12. From the right-side pane, check the box left of **Credit Card Number** and select **Add**.

13. Leave the high **High confidence** unchanged and do not change the **Instance count** of 1.

14. Below **Action**, select **+ Add an action** and **Restrict access or encrypt content in Microsoft 365 locations**.

15. Select the checkbox of **Restrict access or encrypt content in Microsoft 365 locations** again and select **Block everyone. Only the content owner, the last modifier and the site admin will continue to have access.** 

16. Below **User notification**, select the slider to **On** and select **Customize the policy tip text**.

17. Enter the following text to the textbox: **Credit card numbers are not allowed to be shared!**

18. Below **Incident reports**, select the slider **Send an alert to admins when a rule match occurs** and select **Add or remove people**.

19. On the **Add or remove people** page, select the checkbox left from **Joni Sherman** and select **Add**.

20. Select **Save**.

21. Review the rule settings and select **Next**.

22. Select the radio button **Turn it on right away** and select **Next**.

23. Review the policy settings again and select **Submit** then **Done**.

24. Leave the browser open.

You have successfully created a new custom DLP policy for protecting credit card numbers from being shared via Teams conversations.

#### Task 10 – Test the DLP Policies

To make sure your configured DLP policies are working as expected, you need to perform some testing with your pilot users.

**Note:** It can take up to 24 hours till new DLP policies take effect. If the steps does not work, continue with the lab, and perform task 6 at a later point of working through this lab.

1. Connect to the **Client 2 VM** with the credentials that have been provided to you.

2. Open the Teams Desktop client from the taskbar, where you are still signed in as **Megan Bowen**.

3. In the left-hand navigation pane, select **Teams**, and then select the **General** channel below **Teams Rollout**.

4. Select **New conversation** from the main window.

5. Enter the following lines to the textbox:

	- MasterCard: 5105105105105100

	- Visa: 4111111111111111

	- Visa: 4012888888881881

6. Select the arrow to the right from the lower-right corner below the text box to send the message.

7. After a moment, you should see a text in red above your new conversation message that states, "**This message was blocked.**" **Select What can I do?** To see the reason why this message was blocked.

8. Select **Report** to notify the admin about this DLP policy violation. Now you can see a different message above your conversation entry, that states **Blocked.** **You’ve reported this to your admin.**

9. Connect to the **Client 1 VM** with the credentials that have been provided to you.

10. You should still be logged in to the **Microsoft 365 Compliance center**. If not, open Microsoft Edge, maximize the browser, and navigate to the **Microsoft 365 Compliance center**: [**https://compliance.microsoft.com**](https://compliance.microsoft.com/).

11. Select **Reports** from the left-hand navigation pane and scroll down to **Organizational data**.

12. Below **DLP Policy Matches** and **DLP Incidents**, you can now see the DLP policy matches. Select **DLP Policy Matches** to open the detailed view.

13. On the **DLP Policy Matches** page, inspect the rule matches.

You have successfully tested your DLP policy to block sharing of credit card information via Teams chat and channel conversations.

END OF LAB
