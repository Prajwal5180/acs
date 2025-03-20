## Lab: Configuring Access Controls with Entra ID in Azure Communication Services

### Objective:
Learn how to configure access controls using Microsoft Entra ID to secure Azure Communication Services resources.

### Prerequisites:
- An active Azure subscription
- Azure Communication Services resource
- Basic knowledge of Azure portal and Azure CLI

### Lab Steps:

#### Step 1: Create an Azure Communication Services Resource
1. **Navigate to the Azure portal** and sign in with your Azure account.
2. **Create a new Azure Communication Services resource**:
   - Go to **Create a resource** > **Communication Services**.
   - Fill in the required details and click **Create**.

#### Step 2: Register an Application with Microsoft Entra ID
1. **Open the Azure portal** and go to **Azure Active Directory**.
2. **Register a new application**:
   - Click on **App registrations** > **New registration**.
   - Enter a name for the application and click **Register**.
3. **Create a client secret**:
   - Navigate to **Certificates & secrets**.
   - Click on **New client secret** and add a description.
   - Copy the client secret value for later use.

#### Step 3: Assign Roles to the Application
1. **Navigate to the Azure Communication Services resource**.
2. **Open Access control (IAM)**:
   - Click on **Add role assignment**.
   - Select the **Contributor** role or create a custom role with the necessary permissions:
     - **Microsoft.Communication/CommunicationServices/Read**
     - **Microsoft.Communication/CommunicationServices/Write**
     - **Microsoft.Communication/EmailServices/Write**
3. **Assign the role to the registered application**.

#### Step 4: Configure Authentication in Your Application
1. **Set up environment variables**:
   - Use the values from the registered application (client ID, client secret, tenant ID).
2. **Authenticate using Azure CLI**:
   - Run the following command to create a service principal:
     ```bash
     az ad sp create-for-rbac --name <application-name> --role Contributor --scopes /subscriptions/<subscription-id>
     ```
   - Copy the returned JSON values for use in your application.

#### Step 5: Test Access Controls
1. **Develop a sample application** to test access controls:
   - Use the Azure Communication Services SDK to integrate with Entra ID.
   - Ensure the application can authenticate and access the Communication Services resource.
2. **Run the application** and verify that access controls are correctly configured.

### Conclusion:
By completing this lab, you should be able to configure access controls using Microsoft Entra ID to secure Azure Communication Services resources effectively.

## Lab: Configuring Access Controls with Microsoft Entra ID for Azure Communication Services

### Objective
Learn how to configure access controls using Microsoft Entra ID (formerly Azure Active Directory) for Azure Communication Services.

### Prerequisites
- An active Azure subscription
- Azure Communication Services resource
- Microsoft Entra ID setup
- Azure CLI installed

### Steps

#### 1. **Create a Service Principal**
1. Open the Azure portal and navigate to **Azure Active Directory**.
2. Select **App registrations** and click **New registration**.
3. Enter a name for your application and click **Register**.
4. Note the **Application (client) ID** and **Directory (tenant) ID**.

#### 2. **Assign Roles to the Service Principal**
1. Navigate to your Azure Communication Services resource.
2. Select **Access control (IAM)** and click **Add role assignment**.
3. Choose the role **Azure Communication Services Email Sender** or other relevant roles.
4. Select **Members** and add your registered application.

#### 3. **Configure Authentication in Your Application**
1. Use the Azure CLI to create a service principal:
   ```bash
   az ad sp create-for-rbac --name <app-name> --role <role> --scopes /subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Communication/communicationServices/<resource-name>
   ```
2. Note the **appId**, **password**, and **tenant** values.

#### 4. **Set Up Managed Identity (Optional)**
1. Navigate to your Azure Communication Services resource.
2. Select **Identity** under **Settings**.
3. Enable **System assigned managed identity**.
4. Assign the necessary roles to the managed identity.

#### 5. **Integrate with Azure Communication Services SDK**
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

#### 6. **Test the Configuration**
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

### Conclusion
You have successfully configured access controls using Microsoft Entra ID for Azure Communication Services. This setup enhances security by leveraging managed identities and role-based access control.
