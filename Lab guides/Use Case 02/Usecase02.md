# Use case 02-Setup and use Unity Catalog for Data Management in Azure Databricks

Unity Catalog offers a centralized governance solution for data and AI,
simplifying security by providing a single place to administer and audit
data access. In this exercise, you'll configure Unity Catalog for an
Azure Databricks workspace and use it to manage data.

**Note**: In some cases, Unity Catalog may already be enabled for your
workspace. You can still follow the steps in this exercise to assign a
new storage account for your catalog.

This lab will take approximately **45** minutes to complete.

**Note**: The Azure Databricks user interface is subject to continual
improvement. The user interface may have changed since the instructions
in this exercise were written.

**Before you start**

You'll need an [Azure subscription](https://azure.microsoft.com/free) in
which you have global administrator rights.

**IMPORTANT**: This exercise assumes you have *Global
Administrator* rights in your Azure subscription. This level of access
is required to manage the Databricks account in order to enable Unity
Catalog in an Azure Databricks workspace.

## Task 1: Create an Azure Databricks workspace

**Tip**: If you already have a premium tier Azure Databricks workspace,
you can skip this procedure and use your existing workspace.

1.  Login to +++https://portal.azure.com+++  using the Azure login
    credentials. Search for +++azure databricks+++ from the search
    bar and select it.

     ![](./media/image1.png)

2.  Create an **Azure Databricks** resource with the following settings:

    a)  **Subscription**: **Select your Azure subscription**

    b)  **Resource group**: Create a new resource group
        named **+++msl-XX+++** (where "xxxxxxx" is a unique value)*

    c)  **Workspace name**: +++databricks-xxx+++ (where "xxx" is the value used in the resource group name)

    d)  **Region**: Select any **East US2** region

    e)  **Pricing tier**: **Premium** 

    f)  **Managed Resource Group name**: databricks-xxxxxxx-managed (where "xxxxxxx" is the
        value used in the resource group name)

      ![](./media/image2.png)
     
      ![](./media/image3.png)

3.  On the **Review + Create** tab, once the message in the ribbon
    returns "**Validation passed**", verify your selections and
    click **Create**.

      ![](./media/image4.png)
     
      ![](./media/image5.png)

4.  Wait several minutes while your deployment is in progress. Once
    complete, click **Go to resource**.

     ![](./media/image6.png)
 
     ![](./media/image7.png)

## Task 2: Prepare storage for the catalog

When using Unity Catalog in Azure Databricks, data is stored in an
external store; which can be shared across multiple workspaces. In
Azure, it's common to use an Azure Storage account with support for a
Azure Data Lake Storage Gen2 hierarchical namespace for this purpose.

1.  In Azure portal home page, search for +++**Storage account**+++ from
    the search bar and select it.

     ![](./media/image8.png)

2.  Select the **+Create**

      ![](./media/image9.png)

3.  In the Azure portal, create a new **Storage account** resource with
    the following settings:

    a)  **Subscription**: *Select your Azure subscription*
    
    b)  **Resource group**: *Select the existing ***msl-xxxxxxx***  resource
        group where you created the Azure Databricks workspace.*
    
    c)  **Storage account name**: storeXXXX *(where "xx" is the value used
        in the resource group name)*
    
    d)  **Region**: *Select the region where you created the Azure
        Databricks workspace*
    
    e)  **Primary service**: Azure Blob Storage or Azure Data Lake Storage
        Gen2
    
    f)  **Performance**: Standard
    
    g)  **Redundancy**: Locally-redundant storage (LRS) *(For a
        non-production solution like this exercise, this option has lower
        cost and capacity consumption benefits)*

      ![](./media/image10.png)
     
      ![](./media/image11.png)

4.  Select **Review + create** and wait for deployment to complete.

      ![](./media/image12.png)

5.  When deployment has completed, go to the
    deployed ***storexxxxxxx*** storage account resource

     ![](./media/image13.png)

6.  When deployment has completed, go to the
    deployed **storageazuredatabricksxx** storage account resource and
    use its **Storage browser** page to add a new blob container
    named +++**data+++**. This is where the data for your Unity Catalog
    objects will be stored.

     ![](./media/image14.png)
    
    ![](./media/image15.png)
    
    ![](./media/image16.png)

## Task 3: Configure access to catalog storage

To access the blob container you have created for Unity Catalog, your
Azure Databricks workspace must use a managed account to connect to the
storage account through an *access connector*.

1.  On the Azure portal search bar, search for **Access Connector for
    Azure Databricks** and select it

    ![](./media/image17.png)

2.  Click the **+ Create** button.

     ![](./media/image18.png)

3.  In the Azure portal, create a new **Access connector for Azure
    Databricks** resource with the following settings:

    a)  **Subscription**: *Select your Azure subscription*

    b)  **Resource group**: *Select the
        existing **msl-xxxxxxx** resource group where you created the
        Azure Databricks workspace.*

    c)  **Name**: connector-xxxxxxx *(where "xxxxxxx" is the value used
        in the resource group name)*

    d)  **Region**: *Select the region where you created the Azure
        Databricks workspace*

      ![](./media/image19.png)

4.  In the **Review + submit** tab, once the Validation is Passed, click
    on the **Create** button.

      ![](./media/image20.png)

4.  Click on the **Go to resource**

     ![](./media/image21.png)

5.  Copy and Save the Resource Id in notepad

      ![](./media/image22.png)

4.  Click on the **Home** button

      ![](./media/image23.png)

5.  Select the Storage account

     ![](./media/image24.png)

6.  From the left menu, click on the **Access control(IAM).**

7.  On the Access control(IAM) page, Click +**Add** and select **Add
    role assignments.**

     ![](./media/image25.png)

8.  In the **Job function roles** list, search for and select
    the **Storage blob data contributor** role.

      ![](./media/image26.png)

9.  In the **Add role assignment** tab, select **Managed identity** .
    Under Members, click **+Select members**

     ![](./media/image27.png)

10.  Select **Next**. Then on the **Members** page, select the option to
    assign access to a **Managed Identity** and then find and select
    the connector-xxxxxxx access connector for Azure Databricks you
    created previously (you can ignore any other access connectors that
    have been created in your subscription)

      ![](./media/image28.png)
     
      ![](./media/image29.png)

11.  In the **Add role assignment** page, Click **Review + Assign**, you
    will get a notification once the role assignment is complete.

      ![](./media/image30.png)
     
      ![](./media/image31.png)

12.  Select **Home** button

      ![](./media/image32.png)

## Task 4: Configure Unity Catalog

Now that you have created a blob storage container for your catalog and
provided a way for an Azure Databricks managed identity to access it,
you can configure Unity Catalog to use a metastore based on your storage
account.

1.  In the Azure portal home page select your Azure Databricks
    service(databricksXX)

     ![](./media/image33.png)

2.  In the Azure portal, view the **msl-*xxxxxxx*** resource group,
    which should now contain three resources:

    a)  The **databricks-*xxxxxxx*** Azure Databricks workspace

    b)  The **store*xxxxxxx*** storage account

    c)  The **connector-*xxxxxxx*** access connector for Azure
        Databricks

      ![](./media/image34.png)

3.  Open the **databricks-xxxxxxx** Azure Databricks workspace resource
    you created and earlier, and on its **Overview** page, use
    the **Launch Workspace** button to open your Azure Databricks
    workspace in a new browser tab; signing in if prompted.

      ![](./media/image35.png)

4.  In the **databricks-*xxxxxxx*** menu at the top right,
    select **Manage account** to open the Azure Databricks account
    console in another tab.

     ![](./media/image36.png)
      ![](./media/image37.png)

    **Note**: If **Manage account** is not listed or doesn't successfully
    open, you may need to have a global administrator add your account to
    the **Account Admin** role in your Azure Databricks workspace.

5.  Delete the existing **metastore**

     ![](./media/image38.png)
    
     ![](./media/image39.png)

6.  In the Azure Databricks account console, on the **catalog** page,
    select **Create metastore**.

7.  Create a new metastore with the following settings:

    a)  **Name**: metastore-xxxxxxx *(where xxxxxxx is the unique value
        you've been using for resources in this exercise)*

    b)  **Region**: *Select the region where you created your Azure
        resources*

    c)  **ADLS Gen 2
        path**: **data@storexxxxxxx.dfs.core.windows.net/** *(where
        storexxxxxx is the your storage account name)*

    d)  **Access Connector Id**: *The **resource ID for your access
        connector** (copied from its Overview page in the Azure portal)*

      ![](./media/image40.png)

8.  After creating the metastore, select
    the **databricks-*xxxxxxx*** workspace and assign the metastore to
    it.

      ![](./media/image41.png)

## Task 5: Work with data in Unity Catalog

Now that you've assigned an eternal metastore and enabled Unity Catalog,
you can use it to work with data in Azure Databricks.

1.  Close the Azure Databricks account console browser tab and return to
    the tab for your Azure Databricks workapace. Then refresh the
    browser.

2.  On the **Catalog** page, select the **Main** catalog for your
    organization and note that schemas
    named **default** and **Information_schema** have already been
    created in your catalog.

      ![](./media/image42.png)

3.  Sin the **Catalog Explore** pane, select **Create schema**
    ![](./media/image43.png)

4.  In the Create a new schema tab, enter the schema
    name +++sales+++ and click on the **Create** button

     ![](./media/image44.png)
 
      ![](./media/image45.png)

5.  In the Catalog explorer in Azure Databricks workspace, with
    the **sales** schema selected, select **Create** \> **Create
    table**.

     ![](./media/image46.png)

6.  Click on **Browse for file**, navigate to **C:\Labfiles** location
    and select **products.csv**, then click on the **Open** button.

     ![](./media/image47.png)

7.  Select the Create table

     ![](./media/image48.png)
 
     ![](./media/image49.png)

**Note**: You may need to wait a few minutes for serverless compute to
start.

## Task 6:Manage permissions

1.  With the **products** table selected, on the **Permissions** tab
    verify that by default there are no permissions assigned for the new
    table (you can access it because you have full administrative
    rights, but no other users can query the table).

      ![](./media/image50.png)

2.  Select **Grant**, and configure access to the table as follows:

    - **Principals**: All account users

    - **Privileges**: SELECT

    - **Additional privileges required for access**: Also grant USE SCHEMA on main.sales

       ![](./media/image51.png)

## Task 7: Track lineage

1.  On the **+ New** menu, select **Query** and create a new query with
    the following SQL

      ![](./media/image52.png)
        ```
        SELECT Category, COUNT(*) AS Number_of_Products
        FROM main.sales.products
        GROUP BY Category;
    
        ```

3.  Ensure serverless compute is connected, and run the query to see the
    results.

      ![](./media/image53.png)
     
      ![](./media/image54.png)

4.  **Save** the query as Products by Category in the workspace folder
    for your Azure Databricks user account.

      ![](./media/image55.png)
     
      ![](./media/image56.png)

5.  Return to the **Catalog** page. Then expand the **main** catalog and
    the **sales** schema, and select the **products** table.

      ![](./media/image57.png)
     
      ![](./media/image58.png)

6.  On the **Lineage** tab, select **Queries** to verify that the
    lineage from the query you created to the source table has been
    tracked by Unity Catalog.

      ![](./media/image59.png)

**Task 8: Clean up**

1.  Navigate to **Azure portal Home** page, click on **Resource
    groups**.

     ![](./media/image60.png)

2.  Click on the resource group.

3.  In the **Resource group** home page, select the **delete resource
    group**

      ![](./media/image61.png)

4.  In the **Delete Resources** pane that appears on the right side,
    navigate to **Enter “resource group name” to confirm deletion**
    field, then click on the **Delete** button
