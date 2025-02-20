# Use case 03 - Real-Time Streaming with Azure Databricks and Event Hubs

**Introduction:**

In this lab, you will explore real-time streaming capabilities using
Azure Databricks. Real-time streaming allows you to process and analyze
data as it arrives, enabling timely insights and actions. Azure
Databricks provides a powerful platform for building and managing
real-time data pipelines, leveraging Apache Spark's streaming
capabilities.

**Objective:**

- Set up a real-time streaming environment in Azure Databricks.

- Ingest and process streaming data from various sources.

- Implement transformations and aggregations on streaming data.

- Visualize and analyze real-time data using Databricks notebooks.

- Integrate real-time streaming with other Azure services for end-to-end
  data processing.

## Task 1: Create Azure Event Hubs Service 

1.  Open your browser, navigate to the address bar, type or paste the
    following URL: +++https://portal.azure.com/+++, then press the
    **Enter** button.

2.  In the **Sign in** window, enter the **Username** and click on the
    **Next** button.

     ![](./media/image1.png)

3.  Then, enter the password and click on the **Sign in** button**.**

      ![](./media/image2.png)

4.  In **Stay signed in?** window, click on the **Yes** button.

      ![](./media/image3.png)

5.  On **Welcome to Microsoft Azure** dialog box, click on **Maybe
    later** button.

      ![](./media/image4.png)

6.  Open the Azure portal, type +++**Event Hubs+++** in the search bar
    and click **Event Hubs**.

      ![](./media/image5.png)

7.  Select **+ Create**.

      ![](./media/image6.png)

8.  In the **Create Namespace** window, under the **Basics** tab, enter
    the following details and click on the **Next:Advanced\>** button.

    a.  **Subscription**: Select the assigned subscription

    b.  **Resource group:** Click on **Create new**\>
        +++**RG-Databricks21**+++

    c.  **Namespace name:** Enter namespace as
        +++**eh-namespace-demo**+++

    d.  **Location**: West US

    e.  **Pricing tier**: Select **Basic**

      ![](./media/image7.png)
      ![](./media/image8.png)

9.  In the **Advanced** tab, leave all in the default state, and click
    on the **Next:Networking** button

     ![](./media/image9.png)

10. In the **Tag** tab, select **Review+create**

      ![](./media/image10.png)

11. In the **Review + submit** tab, once the Validation is Passed, click
    on the **Create** button.

     ![](./media/image11.png)

12. Wait for the deployment to complete. The deployment will take around
    2-3 minutes.

     ![](./media/image12.png)

13. After the deployment is completed, click on **Go to resource**
    button.

     ![](./media/image13.png)
     ![](./media/image14.png)

14. On the Event Hubs Namespace home page, select **Event Hubs** under
    the **Entities** section.
     ![](./media/image15.png)

15. Select the **+ Event Hub**

      ![](./media/image16.png)

16. In the **Create Event Hub** window, under the **Basics** tab, enter
    the following details and click on the **Next:Capture\>** button.

    |   |    |
    |-----|----|
    |Name|	Enter name +++eh-demo+++|
    |Partition count|	2|
    |Cleanup policy	|Delete|
    |Retention time|	1|


      ![](./media/image17.png)

17. Select **Review+create**

     ![](./media/image18.png)

18. In the **Review + submit** tab, once the Validation is Passed, click
    on the **Create** button.

     ![](./media/image19.png)
     ![](./media/image20.png)

19. Now, click on the **eh-demo**

     ![](./media/image21.png)

20. To crate event data , Select **Data Explorer** and click on the
    **Send events**

      ![](./media/image22.png)

21. Copy the following code and paste it into the 'Enter Payload' field,
    then click on the '**Send**' button

    ```
    {
        "temperature": 20,
        "humidity": 60,
        "windSpeed": 10,
        "windDirection": "NW",
        "precipitation": 0,
        "conditions": "Partly Cloudy"
    }
    ```
    ![](./media/image23.png)
    ![](./media/image24.png)

22. Select **Data Explorer** and click on the **Send events**

      ![](./media/image22.png)

23. Copy the following code and paste it into the 'Enter Payload' field,
    then click on the '**Send**' button

     ```
        {
            "temperature": 50,
            "humidity": 60,
            "windSpeed": 50,
            "windDirection": "SW",
            "precipitation": 0,
            "conditions": "Rain"
        }
      ```
      ![](./media/image25.png)

24. Now , click on the View events

     ![](./media/image26.png)
    
     ![](./media/image27.png)

25. In the Event Hubs Instance home page, select 'Shared access
    policies' under settings, and then click on **+Add**
     ![](./media/image28.png)

27. In the **Add SAS Policy** tab enter the databricks and select
    Listen. Click on the **Create** button

     ![](./media/image29.png)
     ![](./media/image30.png)

27. Copy the Primary connection string value into a notepad; we will use
    it in the next task

      ![](./media/image31.png)

## Task 2: Provision an Azure Databricks workspace

1.  Login to +++https://portal.azure.com+++ using the Azure login
    credentials. Search for +++azure databricks+++ from the search
    bar and select it.

      ![](./media/image32.png)

2.  Select **+ Create**.

      ![](./media/image33.png)

3.  Create an **Azure Databricks** resource with the following settings:

    a) **Subscription**: *Select the same Azure subscription that you
      used to create your Azure OpenAI resource*

    b) **Resource group**: *The same resource group where you created
      your Azure Event Hubs*

    c) **Region**: *The same region where you created Azure Event Hubs
      resource*

    d) **Name**: Enter the name as **databricksXXXX** (XXXX*A unique
      number of your choice)*

    e) **Pricing tier**: *Premium* 



4.  Select **Review + create** and wait for deployment to complete. Then
    go to the resource and launch the workspace.

      ![](./media/image34.png)

5.  On the **Review** **+ create** tab, click on the **Create** button.

      ![](./media/image35.png)
     
      ![](./media/image36.png)

6.  Once created, click on **Go to resource**.

      ![](./media/image37.png)

7.  In the **Overview** page for your workspace, use the **Launch
    Workspace** button to open your Azure Databricks workspace in a new
    browser tab; signing in if prompted.
      ![](./media/image38.png)

## Task 3: Creating a ADLS Gen2Storage Account and Container. 

1.  In the Azure home search bar, search for “Storage accounts” and
    select Storage accounts from the results.

     ![](./media/image39.png)

2.  Click on **+Create** button.

      ![](./media/image40.png)

3.  On the page, enter the following details and click on the Next
    button

    - **Subscription**: *Select your Azure subscription*
    
    - **Resource group**: *Select the existing **RG-DatabricksXXX** resource
      group where you created the Azure Databricks workspace.*
    
    - **Storage account name**: storageazuredatabricksXX *(where "xx" is the
      value used in the resource group name)*
    
    - **Region**: *Select the region where you created the Azure Databricks
      workspace*
    
    - **Primary service**: Azure Blob Storage or Azure Data Lake Storage
      Gen2
    
    - **Performance**: Standard
    
    - **Redundancy**: Locally-redundant storage (LRS) *(For a non-production
      solution like this exercise, this option has lower cost and capacity
      consumption benefits)*

        ![](./media/image41.png)

4.  In **Advanced** tab, select **Enable hierarchical namespace** and
    click on the **Review+ create**

      ![](./media/image42.png)

5.  In the **Review + submit** tab, once the Validation is Passed, click
    on the **Create** button.

      ![](./media/image43.png)

       ![](./media/image44.png)

6.  Click the “**Go to resource button**”.

      ![](./media/image45.png)

7.  When deployment has completed, go to the
    deployed **storageazuredatabricksxx** storage account resource and
    use its **Storage browser** page to add a new blob container
    named +++metastore-demo+++. This is where the data for your
    Unity Catalog objects will be stored.

     ![](./media/image46.png)
     
     ![](./media/image47.png)
    
     ![](./media/image48.png)

## Task 4: Configure access to catalog storage

To access the blob container you have created for Unity Catalog, your
Azure Databricks workspace must use a managed account to connect to the
storage account through an *access connector*.

1.  On the Azure portal search bar, search for **Access Connector for
    Azure Databricks**

      ![](./media/image49.png)

2.  Click the **+ Create** button.

     ![](./media/image50.png)

3.  In the Azure portal, create a new **Access connector for Azure
    Databricks** resource with the following settings:

    - **Subscription**: Select your Azure subscription

    - **Resource group**: Select the existing  resource group where you
      created the Azure Databricks workspace.

    - **Name**: connector-xxxxxxx(where "xxxxxxx" is the value used in
      the resource group name)

    - **Region**: *Select the region where you created the Azure
      Databricks wrkspace

      ![](./media/image51.png)

4.  In the **Review + submit** tab, once the Validation is Passed, click
    on the **Create** button.

      ![](./media/image52.png)

5.  Click on the **Go to resource**

     ![](./media/image53.png)
     ![](./media/image54.png)

6.  Select **ConnecterXX**

      ![](./media/image55.png)

7. Copy and Save the Resource Id in notepad

     ![](./media/image56.png)

8. Select the **Storage account**

      ![](./media/image57.png)

9. From the left menu, click on the **Access control(IAM).**

10. On the Access control(IAM) page, Click +**Add** and select **Add
    role assignments.**

     ![](./media/image58.png)

11. Type the **Storage blob data contributor** in the search box and
    select it. Click **Next**

      ![](./media/image59.png)

12. In the **Add role assignment** tab, select **Managed identity** .
    Under Members, click **+Select members**

       ![](./media/image60.png)

13. Select the connector-xxxxxxx access connector for Azure Databricks
    you created previously (you can ignore any other access connectors
    that have been created in your subscription)

      ![](./media/image61.png)

14. In the **Add role assignment** page, Click **Review + Assign**, you
    will get a notification once the role assignment is complete.

      ![](./media/image62.png)

     ![](./media/image63.png)

15. Select **Home**

     ![](./media/image64.png)

## Task 5: Create a cluster

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

1.  In the Azure portal home page select your Azure Databricks
    service(databricksXX)

      ![](./media/image65.png)

2.  In Azure Databricks home page, click on the **Launch Workspace**

      ![](./media/image66.png)

3.  In the sidebar on the left, select the **(+) New** task, and then
    select **Cluster**.

     ![](./media/image67.png)
     ![](./media/image68.png)

4.  In the **New Cluster** page, create a new cluster with the following
    settings:

    - **Cluster name**: *User Name's* cluster (the default cluster name)

    - **Policy**: Unrestricted

    - **Cluster mode**: Single Node

    - **Access mode**: Single user (*with your user account selected*)

    - **Databricks runtime version**: *Select the Runtime12.2 LTS(scale
      2.12, 4Cores)*

    - **Node type**: Standard_DS3_v2

    - **Terminate after** *20* **minutes of inactivity**

&nbsp;

5.  Wait for the cluster to be created. It may take a 5-7 minutes.

    **Note**: If your cluster fails to start, your subscription may have
    insufficient quota in the region where your Azure Databricks workspace
    is provisioned. See CPU core limit prevents cluster creation for
    details. If this happens, you can try deleting your workspace and
    creating a new one in a different region.

      ![](./media/image69.png)
    
      ![](./media/image70.png)

6.  In your cluster's page, select the **Libraries** tab.

      ![](./media/image71.png)

7.  Select **Install New**.

      ![](./media/image72.png)

8.  Select **Maven** as the library source and select **Search
    Packeges**

      ![](./media/image73.png)

9.  In the **Search packages** tab, select **'Maven Central'** and enter
    **+++eventhubs-spark+++** in the search field. Then, select the
    latest version
     ![](./media/image74.png)

10.  Click on **Install**

      ![](./media/image75.png)

      ![](./media/image76.png)

      ![](./media/image77.png)

11.  In the dropdown menu, select 'Settings' to access various
    configuration options.

     ![](./media/image78.png)

12. In the Databricks workspace settings, navigate to the 'Advanced'
    section to configure options, DBFS File Browser is On.

      ![](./media/image79.png)

## Task 6: Configure Unity Catalog

1.  Navigate to the top right corner of your **Databricks workspace**,
    click on your account name (e.g., 'databricks21'), and select
    **'Manage account**' from the dropdown menu to access and modify
    your account settings

     ![](./media/image80.png)

2.  Select **Catalog** and select existing **Metastores**

      ![](./media/image81.png)

3.  Delete existing metastore

      ![](./media/image82.png)

4.  In Catalog pane, select **Create metastore**

     ![](./media/image83.png)

5.  In Create metastore pane enter the following details and select Create

     a)  Name: Enter the MetastoreXX
    
    b)  Region : Westus
    
     c)  ADLS Gen 2 : path:<container>@<storage_account_name>.dfs.core.windows.net/
    
     d)  **Access Connector Id: Enter Access Connector for Azure Databricks
        resource ID which you have saved in Task 4\> Step 10**

      ![](./media/image84.png)

6.  Assign the 'databricks21' metastore to the selected workspace by
    clicking on the 'Assign' button

       ![](./media/image85.png)

7.  Click on the Enable button

      ![](./media/image86.png)

## Task 7: Real-time Data Processing with Azure Databricks

1.  Navigate to the 'Workspace' section by clicking on 'Workspace' in
    the left-hand menu, then expand the 'Workspace' folder in the main
    panel.

      ![](./media/image87.png)

2.  In the 'Users' section, Select the user with the email

     ![](./media/image88.png)

3.  To import files into your Databricks workspace, click on the
    ellipsis icon next to 'Send feedback,' and select '**Import**' from
    the dropdown menu

      ![](./media/image89.png)

4.  In the Import dialog box, select the 'File' option, then click
    'Browse' to locate and upload your file

     ![](./media/image90.png)

5.  Navigate to **+++https://github.com/venki-hari21/Databricks/tree/main/Labfiles+++** location and select Real-time Data Processing with Azure Databricks (and Event Hubs), then download the file
      
6.  Click on the **Import**

      ![](./media/image92.png)

7.  Select **Notebook**

     ![](./media/image93.png)

8.  Start the **cluster**

      ![](./media/image94.png)
     
      ![](./media/image95.png)

9.  Select the 1^(st) cell and run the cell

      ![](./media/image96.png)

10. Select the 2^(nd) cell and run the cell

      ![](./media/image97.png)

11. To check the models loaded in your Databricks workspace, navigate to
    the "Catalog" section in the left sidebar. Here, you can explore
    different layers such as "bronze," "silver," and "gold" to find the
    models you have loaded

     ![](./media/image98.png)

12. Select the 3^(rd) cell and replace the event hub name and Paste
    connection string. Run the cell

     ![](./media/image99.png)
    
     ![](./media/image100.png)

13. Select the 4^(th) cell and run the cell

     ![](./media/image101.png)

14. Click on the **Interrupt**

     ![](./media/image102.png)

15. Defining the schema for the JSON object, select cell and click on
    the run

     ![](./media/image103.png)

16. Select the cell and run the cell.

      ![](./media/image104.png)

17. Select the Interrupt the cell

      ![](./media/image105.png)

18. Reading, aggregating and writing the stream from the silver to the
    gold layer. Select the cell and run the cell

      ![](./media/image106.png)
## Task 8 : Clean up

1.  Navigate to **Azure portal Home** page, click on **Resource
    groups**.

     ![](./media/image107.png)

2.  Click on the resource group.

     ![](./media/image108.png)

3.  In the **Resource group** home page, select the **delete resource
    group**

      ![](./media/image109.png)

4.  In the **Delete Resources** pane that appears on the right side,
    navigate to **Enter “resource group name” to confirm deletion**
    field, then click on the **Delete** button

      ![](./media/image110.png)
