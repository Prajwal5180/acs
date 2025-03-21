# Lab 01 : Setting Up Azure Communication Services Environment 

## Estimated Duration:  minutes

## Lab Overview



## Lab objectives
In this lab, you will complete the following tasks:
+ Task 1: Provisioning Azure Communication Services (ACS) resources
+ Task 2: Configuring access controls with Microsoft Entra ID
+ Task 3: Exploring the Azure portal to familiarize with ACS features

### Prerequisites:
- An active Azure subscription
- Basic knowledge of Azure portal and Azure CLI

## Task 1: Provisioning Azure Communication Services (ACS) Resources

Azure Communication Services provides APIs for integrating multichannel communication features—such as voice, video, chat, text messaging/SMS, and email—into your applications. With its REST APIs and client library SDKs, you don't need deep technical expertise to add communication capabilities to your apps.

1. On the Azure portal, in **Search resources, services and docs (G+/)** box at the top of the portal search for **Communication Services (1)** and select **Communication Services (2)**.

   ![](../images/day1-lab1-1.png)

1. On the **Communication Services** blade, click **+ Create**.

   ![](../images/day1-lab1-2.png)

1. On **Create Resource** blade specify the following settings and click **Review + Create**.

    | Setting | Value |
    | --- | --- |
    | Subscription | **Keep it as default (1)**  |
    | Resource group | **acs-<inject key="DeploymentID"></inject> (2)** |
    | Resource Name | **communication-<inject key="DeploymentID"></inject> (3)**|
    | Data location | **United States (5)** |

   ![](../images/day1-lab1-5.png)

4. After the validation gets succeeded, click on **Create** to provision communication services.

   ![](../images/day1-lab1-4.png)

    >**Note**: After clicking on Submit, it might take 2 minutes to provision the communication services.

1. **Navigate to the Communication Services resource** you created.
1. **Go to "Keys"** in the left-hand menu.
1. **Copy the connection strings and service endpoints**:
   - You will use these values to authenticate and connect to the Communication Services resource.

## Task 2: Configuring access controls with Microsoft Entra ID

#### 1. **Create a Service Principal**

1. In the Azure portal, search for **Microsoft Entra ID (1)** and select **Microsoft Entra ID (2)**.

   ![](../images/day1-lab1-6.png)

2. Select **App registrations (2)** under **Manage (1)** section from the left pan.

   ![](../images/day1-lab1-9.png)

3. Click on **+ New registration** from upper left corner of App registrations page.

   ![](../images/day1-lab1-7.png)

3. On **Register an application** blade specify the following settings and click **Register**.

    | Setting | Value |
    | --- | --- |
    | Name | **communication-app (1)**  |
    | Supported account types | **Accounts in this organizational directory only (xxxx - Single tenant) (2)** |

   ![](../images/day1-lab1-8.png)

1. Navigate to **Certificates & secrets** under manage section of the newly registered app.

1. Click on **+ New client secret** under client secrets section. On new **Add a client secret** tab, provide the **Description** and click on **Add**. Copy the client secret value and paste it in notepad for later use before leaving the page as it cannot be viewed, except for immediately after creation.

4. Note the **Application (client) ID** and **Directory (tenant) ID**.

#### 2. **Assign Roles to the Service Principal**

1. Navigate to your **communication-<inject key="DeploymentID"></inject>** Azure Communication Services resource.

2. Select **Access control (IAM) (1)** and click on **+ Add (2)** dropdown the click on **Add role assignment (3)**.

   ![](../images/day1-lab1-11.png)

3. In the Add role assignment tab, Search and select the role **Communication and Email Service Owner** then click on **Next**.



4. On Members tab, keep assign access to as User, group or service principal then under **Members** select **+ Members**. From select members, search and select  **communication-app** service principal then choose **Select**.

1. Click on **Review + assign** twice.

2. Note the **appId**, **password**, and **tenant** values.

**Integrate with Azure Communication Services SDK**
1. Install the Azure Communication Services SDK in your project:
   ```bash
   dotnet add package Azure.Communication.Common
   dotnet add package Azure.Communication.Email
   ```
2. Use the following code to authenticate using Microsoft Entra ID:
   ```csharp
   var clientSecretCredential = new ClientSecretCredential(
       "<tenant-id>",
       "<client-id>",
       "<client-secret>"
   );

   var emailClient = new EmailClient(
       new Uri("https://<resource-name>.communication.azure.com"),
       clientSecretCredential
   );
   ```

**Test the Configuration**
1. Send a test email using the configured client:
   ```csharp
   var emailMessage = new EmailMessage(
       "<sender-email>",
       new EmailRecipients(new List<EmailAddress> { new EmailAddress("<recipient-email>") }),
       "Test Email",
       new EmailContent("This is a test email.")
   );

   var response = await emailClient.SendAsync(emailMessage);
   Console.WriteLine($"Email sent with status: {response.Status}");
   ```

## Task 2: Exploring the Azure portal to familiarize with ACS features

By the end of this lab, you will have hands-on experience navigating the **Azure Portal**, discovering key features of **Azure Communication Services**, and understanding how to use the portal to manage communication resources.

### **Lab Steps:**

#### **1. Access the Azure Portal**
   - Open a web browser and navigate to the [Azure Portal](https://portal.azure.com).
   - Log in using your **Azure account credentials**.
   
   **Expected Outcome:**
   - You should now be on the **Azure Portal Dashboard**.

---

#### **2. Navigate to Azure Communication Services**
   - On the left sidebar of the Azure Portal, click on the **"Search"** bar at the top.
   - Type **"Communication Services"** in the search bar.
   - Select **"Communication Services"** from the search results.

   **Expected Outcome:**
   - You will be taken to the **Communication Services** section where you can see all your communication resources.

---

#### **3. Create a New Communication Service**
   - Click on the **"Create"** button at the top of the Communication Services page.
   - In the **"Create Communication Services"** wizard, fill in the required information:
     - **Subscription**: Choose the subscription you wish to use.
     - **Resource Group**: Select an existing resource group or create a new one.
     - **Region**: Choose the region closest to you or your users.
     - **Name**: Give your Communication Service a unique name.
   - Click **Review + Create**, review the configuration, and then click **Create**.

   **Expected Outcome:**
   - After a few moments, your new Communication Service will be created, and you will see it listed in the **Communication Services** section.

---

#### **4. Explore Communication Service Overview**
   - After the resource has been created, click on the resource name to access the **Communication Services Overview** page.
   - Review the available options:
     - **Overview Tab**: View the basic details of the service such as name, subscription, and resource group.
     - **Keys and Connection Strings**: Access the keys and connection strings for your communication service.
     - **Monitoring**: Explore metrics, logs, and diagnostics for your communication resources.
   
   **Expected Outcome:**
   - You will gain an understanding of the communication service dashboard and the various monitoring and management features available.

---

#### **5. Explore Communication Service Features**
   - Within the **Communication Services** resource page, explore the following features:
   
     - **Voice and Video**: Learn how to enable voice and video calling for your app.
     - **Chat**: Explore the setup for adding real-time chat capabilities to your applications.
     - **SMS**: Familiarize yourself with how to send SMS messages through Azure Communication Services.
     - **Email**: Set up email communication through Azure's email services.

   **Steps for each feature:**
   - Click on each feature's tab to explore the specific configuration options and settings. 
   - You will likely see options for adding APIs, setting up necessary credentials, and creating communication flows.

   **Expected Outcome:**
   - You should now be familiar with the key communication features available within Azure Communication Services.

---

#### **6. Explore API Access and Authentication**
   - Under the **"Keys and Connection Strings"** section, view the **connection strings** and **API keys**.
   - Familiarize yourself with the options for authenticating applications or services that need to access Azure Communication Services via REST APIs or SDKs.
   - Copy the **connection string** to be used in future projects or applications.

   **Expected Outcome:**
   - You will understand how to obtain and use the connection strings for integrating Azure Communication Services with your applications.

---

#### **7. Configure Resource Access with Role-Based Access Control (RBAC)**
   - In the **Communication Services** resource page, click on the **Access control (IAM)** section from the left-hand menu.
   - Click **+ Add > Add role assignment** to assign roles to users or service principals.
   - Select a role (e.g., **Contributor**, **Reader**) and assign it to a user or application.
   - Click **Save**.

   **Expected Outcome:**
   - You should now understand how to manage access control for your Azure Communication Services resources through RBAC.

---

#### **8. Review Logs and Diagnostics**
   - Under the **Monitoring** section of your Communication Service, explore the **logs**, **metrics**, and **diagnostics** features.
   - Learn how to set up **alerts** to monitor activity on your Communication Service resources.

   **Expected Outcome:**
   - You will have hands-on experience with how to monitor your communication service and troubleshoot any issues that arise.



### Conclusion:
By completing this lab, you should be able to provision Azure Communication Services resources using both the Azure portal and Azure CLI effectively.
