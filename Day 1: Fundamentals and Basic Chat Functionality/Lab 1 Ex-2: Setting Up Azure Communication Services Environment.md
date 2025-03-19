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
