# **Use case 05-Connect to and manage Azure Databricks Unity Catalog in Microsoft Purview**

This lab outlines how to register Azure Databricks, and how to
authenticate and interact with Azure Databricks in Microsoft Purview.
For more information about Microsoft Purview

## Task 0: Install self-hosted integration runtime

1.  Navigate to **+++https://www.microsoft.com/en-us/download/details.aspx?id=105539)+++** location and select
    **IntegrationRuntime_5.48.9089.1** file and install

2.  Navigate to **+++https://www.oracle.com/java/technologies/downloads/#license-lightbox+++** location and select **VC_redist.x64**
    file and install

3.  Navigate to **+++https://aka.ms/vs/17/release/vc_redist.x64.exe+++** location and select
    jdk-11.0.26_windows-x64_bin file and install

## **Task 1: Create an Azure Purview Account**

To create and use the Azure Purview platform, you will need to provision
an Azure Purview account.

1.  Sign in to the Azure portal +++https://portal.azure.com/+++,
    navigate to the **Home** screen, and then click **Create a
    resource**.

     ![](./media/image1.png)

2.  Search the Marketplace for "**Microsoft Purview**" and
    click **Create**.

      ![](./media/image2.png)
 
      ![](./media/image3.png)

3.  Provide the inputs given below on the **Basics** tab.

      |      |     |
      |----------|-----------|
      |Parameter|	Example Value|
      |Subscription|	Your Azure Subscription|
      |Resource group|	+++Purviewlab-rg+++|
      |Microsoft Purview account name|	+++pvlab-{random number}-pv+++|
      |Location|	West US|


      ![](./media/image4.png)

4.  Select **Review + Create**.

      ![](./media/image5.png)

5.  On the **Review + Create** tab, once the message in the ribbon
    returns "**Validation passed**", verify your selections and
    click **Create**.

      ![](./media/image6.png)

6.  Wait several minutes while your deployment is in progress. Once
    complete, click **Go to resource**.

     ![](./media/image7.png)

Stay on the same page and continue to the next exercise.

## Task 2: Provision an Azure Databricks workspace

1.  Login to +++https://portal.azure.com+++ using the Azure login
    credentials. Search for +++**azure databricks**+++ from the search
    bar and select it.

      ![](./media/image8.png)

2.  Select **+ Create**.

     ![](./media/image9.png)

3.  Create an **Azure Databricks** resource with the following settings:

    - **Subscription**: *Select the same Azure subscription that you
      used to create your Azure OpenAI resource*

    - **Resource group**: *The same resource group where you created
      your Azure Purview account*

    - **Region**: *The same region where you created your Azure OpenAI
      resource*

    - **Name**: Enter the name as **databricksXXXX** (XXXX*A unique
      number of your choice)*

    - **Pricing tier**: *Premium* 


4.  Select **Review + create** and wait for deployment to complete. Then
    go to the resource and launch the workspace.

     ![](./media/image10.png)

5.  On the **Review** **+ create** tab, click on the **Create** button.

     ![](./media/image11.png)
    
     ![](./media/image12.png)

5.  Once created, click on **Go to resource**.

      ![](./media/image13.png)

## **Task 3: Create a Key Vault**

1.  From the Azure portal menu or the Home page, select "Create a
    resource".

    ![](./media/image1.png)

2.  In the "Search the Marketplace" box, type "Key Vault" and select it
    from the results.

     ![](./media/image14.png)

3.  Click on "Create" to start the Key Vault creation process.

      ![](./media/image15.png)

4.  **Configure Key Vault Settings**:

    - **Name**: Enter a unique name for your Key Vault. This name must
      be globally unique.

    - **Subscription**: Choose the Azure subscription you want to use.

    - **Resource Group**: Select an existing resource group

    - **Region**: Choose the region *where you created your Azure
      Purview account*

    - **Pricing Tier**: Select the pricing tier that suits your needs
      (Standard or Premium).

     ![](./media/image16.png)

5.  Review your settings and click "Create" to deploy the Key Vault.

      ![](./media/image17.png)

6.  Once the deployment is complete, navigate to Key Vault by selecting
    "Go to resource”

      ![](./media/image18.png)

## Task 4: Grant Permissions to Microsoft Purview

1.  In Key Vault, go to " Access configuration" under the "Settings"
    section.

     ![](./media/image19.png)

2.  Choose "Vault access policy" and click on **Apply**

      ![](./media/image20.png)

      ![](./media/image21.png)

3.  Select **Access policies** and click on "**+Create**"

      ![](./media/image22.png)

4.  Under **Secret permissions**, click **Select all**. Then,
    click **Next**.

     ![](./media/image23.png)

5.  Search for your **account name**, select your account name from the
    search results, then click **Next**.

     ![](./media/image24.png)

6.  Skip the **Application (optional)** page by clicking **Next** again.

     ![](./media/image25.png)

7.  Review your selections then click **Create**.

      ![](./media/image26.png)
 
      ![](./media/image27.png)

8.  In the **Key vault** home page, select **Access policies** and
    click **+ Create**.

     ![](./media/image28.png)

9.  Under **Secret permissions**, select **Get** and **List**. Then,
    click **Next**.

      ![](./media/image29.png)

10. Search for the name of your **Microsoft Purview account**
    (e.g. pvlab-{random number}-pv), select the item, then
    click **Next**

      ![](./media/image30.png)

11. Skip the **Application (optional)** page by clicking **Next** again.

     ![](./media/image31.png)

12. Review your selections then click **Create**.

      ![](./media/image32.png)
      
      ![](./media/image33.png)

## Task 5: Generate a Secret

In order to securely store our Azure SQL Database password, we need to
generate a secret.

1.  Navigate to **Secrets** and click **Generate/Import**.

     ![](./media/image34.png)

2.  **Copy** and **paste** the values below into the matching fields and
    then click **Create**.

  **Name - +++sql-secret+++**
 
  **Value - +++sqlPassword+++**
 
   ![](./media/image35.png)
   ![](./media/image36.png)
## Task 6: Add Credentials to Microsoft Purview

To make the secret accessible to Microsoft Purview, we must first
establish a connection to Azure Key Vault.

1.  Navigate back to the **Home** tab of Azure portal and select **All
    resources**.

     ![](./media/image37.png)

2.  Open the **Microsoft Purview account** **(pvlab-RandomId-pv)**.

      ![](./media/image38.png)

3.  Open the **Microsoft Purview Governance Portal**.

      ![](./media/image39.png)

4.  Navigate to **Data Map**, dropdown the **Source management** and
    click on **Credentials.**

      ![](./media/image40.png)

5.  In the Credentials pane, click **Manage Key Vault connections**.

      ![](./media/image41.png)

6.  In the **Manage Key Vault connections** tab, Click **New**.

      ![](./media/image42.png)

7.  **Copy** and **paste** the value below to set the name of your **Key
    Vault connection**, and then use the drop-down menu items to select
    the appropriate **domain**, **Subscription** and **Key Vault name**,
    then click **Create**.

      **Name – +++KeyVault01+++**

      ![](./media/image43.png)

8.  Since we have already granted the Microsoft Purview managed identity
    access to our Azure Key Vault, click **Confirm**.

      ![](./media/image44.png)

9.  In the **Manage Key Vault connections** tab, click **Close**.

       ![](./media/image45.png)

10. Under **Credentials** click +**New**.

      ![](./media/image46.png)

11. Using the drop-down menu items, set the **Authentication
    method** to SQL authentication and the **Key Vault connection** to
    KeyVault01. Once the drop-down menu items are
    set, **Copy** and **paste** the values below into the matching
    fields, and then click **Create**.

      - **Name - +++credential-SQL+++**
      
      - **User name - +++sqladmin+++**
      
      - **Secret name - +++sql-secret+++**

       ![](./media/image47.png)

       ![](./media/image48.png)

## Task 7. Grant Access to Microsoft Purview's Data Plane

By default, the identity used to create the Microsoft Purview account
resource will have full access to the Microsoft Purview Governance
Portal. The following instructions detail how to provide access to
additional users within your Azure Active Directory.

1.  Select **Domains** under **Data Map** . On the Domains page,
    select **Role assignments** near **Overview**.

      ![](./media/image49.png)

2.  Scroll down and on the right-hand side of **Data curators**, click
    the **add **icon.

      ![](./media/image50.png)

3.  Search for the user within your **Microsoft Entra ID** and select
    the **Account**. Then select **OK**.

     ![](./media/image51.png)
 
     ![](./media/image52.png)

## Task 8: Generate a Personal Access Token in Azure Databricks

1.  Navigate back to the **Home** tab of Azure portal and select **All
    resources**.

      ![](./media/image53.png)

2.  Select Azure Databricks resource

      ![](./media/image54.png)

3.  In Azure Databricks home page, click on the **Launch Workspace**

     ![](./media/image55.png)

4.  Click on your user icon in the top right corner. Select **Settings**

       ![](./media/image56.png)

5.  Select the Developer under User .In the **Access Tokens** tab, click
    on **Manage**

     ![](./media/image57.png)

6.  Click on **Generate New Token**

      ![](./media/image58.png)

7.  Click on the **Generate**

     ![](./media/image59.png)

8.  Copy the token; we will use this token in the next task

      ![](./media/image60.png)

## Task 9: Create a cluster

Azure Databricks is a distributed processing platform that uses Apache
Spark *clusters* to process data in parallel on multiple nodes. Each
cluster consists of a driver node to coordinate the work, and worker
nodes to perform processing tasks. In this exercise, you'll create
a *single-node* cluster to minimize the compute resources used in the
lab environment (in which resources may be constrained). In a production
environment, you'd typically create a cluster with multiple worker
nodes.

**Tip**: If you already have a cluster with a 13.3 LTS **ML** or higher
runtime version in your Azure Databricks workspace, you can use it to
complete this exercise and skip this procedure.

1.  In the sidebar on the left, select the **(+) New** task, and then
    select **Cluster**.

      ![](./media/image61.png)
      ![](./media/image62.png)

2.  In the **New Cluster** page, create a new cluster with the following
    settings:

    - **Cluster name**: *User Name's* cluster (the default cluster name)

    - **Policy**: Unrestricted

    - **Cluster mode**: Single Node

    - **Access mode**: Single user (*with your user account selected*)

    - **Databricks runtime version**: *Select the latest Runtime*

    - **Node type**: Standard_DS3_v2

    - **Terminate after** *20* **minutes of inactivity**

3.  Wait for the cluster to be created. It may take a 5-7 minutes.

      **Note**: If your cluster fails to start, your subscription may have
      insufficient quota in the region where your Azure Databricks workspace
      is provisioned. See CPU core limit prevents cluster
      creation for details. If this happens, you can try deleting your workspace and
      creating a new one in a different region.

      ![](./media/image63.png)
    
      ![](./media/image63.png)
    
     ![](./media/image65.png)

4.  Navigate to the **Tags** tab.Note down **the ClusterId** for future
    use.

      ![](./media/image66.png)

5.  Go to the **Permissions** tab.

     ![](./media/image67.png)

6.  Add the user or group and assign the Can Attach To and Can Restart
    permissions.

      ![](./media/image68.png)

## Task 10: Register

This section describes how to register an Azure Databricks workspace in
Microsoft Purview by using the Microsoft Purview governance portal.

1.  Navigate back to the **Home** tab of Azure portal and select **All
    resources**.

    ![](./media/image37.png)

2.  Open the **Microsoft Purview account** **(pvlab-RandomId-pv)**.

3.  Open the **Microsoft Purview Governance Portal**.

     ![](./media/image69.png)

4.  Navigate to **Domains**, click on **+New collection.**

     ![](./media/image70.png)

5.  Enter the Display name as **+++purviewDemo+++**, and then
    click **Create**.

     ![](./media/image71.png)
 
     ![](./media/image72.png)

6.  Select **Data Map** on the left pane and select **Integration
    runtimes**

     ![](./media/image73.png)

7.  In Integration runtimes pane, select +New

      ![](./media/image74.png)

8.  Now select **Self-Hosted**

     ![](./media/image75.png)

9.  Copy the **Key 1**value in a notepad.

       ![](./media/image76.png)

10. Click on your Windows search box, type **Microsoft** **integration
    runtime**, then click on **Microsoft** **integration runtime** under
    Best match.

     ![](./media/image77.png)

11. Enter the **Authentication key** and select **Register**

      ![](./media/image78.png)
      ![](./media/image79.png)

12. Return to Purview Studio; the integration runtimes are now
    successfully connected.
     ![](./media/image80.png)

13. Select **Data Map** on the left pane.

      ![](./media/image81.png)

14. Select Data source and the select **Register**.

       ![](./media/image82.png)

15. In **Register sources**, select **Azure
    Databricks** \> **Continue**.

       ![](./media/image83.png)

16. On the **Register sources (Azure Databricks)** screen, do the
    following:

    a).  For **Name**, enter a name that Microsoft Purview will list as
        the data source.

    b).  For **Azure subscription** and **Databricks workspace name**,
        select the subscription and workspace that you want to scan from
        the dropdown. The Databricks workspace URL is automatically
        populated.

     c).  Select Domain and click on Register

     ![](./media/image84.png)
    
     ![](./media/image85.png)

## Task 11: Scan

1.  Go to **Data** **Sources**.

     ![](./media/image86.png)

2.  **Name**: Enter a name for the scan.

3.  **Extraction method:** Indicate to extract metadata from Hive
    Metastore or Unity Catalog. Select **Hive Metastore**.

4.  **Connect via integration runtime**: Select the configured
    self-hosted integration runtime.

5.  **Credential**: Select the credential to connect to your data
    source. Make sure to:

6.  Select **Access Token Authentication** while creating a credential.

      ![](./media/image87.png)
      ![](./media/image88.png)

7.  Enter cluster ID and select Create button

      ![](./media/image89.png)

8.  Click on Continue

      ![](./media/image90.png)

9.  In Set a scan trigger tab , select **Once** and click on Continue
    button

     ![](./media/image91.png)

10. Select Save and Run button

      ![](./media/image92.png)
      ![](./media/image93.png)
      
      ![](./media/image94.png)
