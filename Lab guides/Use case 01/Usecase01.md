# Use Case 01 - End-to-End Streaming Pipeline with Delta Live Tables in Azure Databricks.V.12 

Solution focus area

Contoso Corp, a mid-sized manufacturing company, is dedicated to
delivering the highest quality products to its customers. To achieve
this, Contoso Corp focuses on ensuring the optimal performance of its
machinery by continuously monitoring various IoT sensors installed on
the equipment. By leveraging Azure Databricks and real-time data
streaming, the company can ingest, process, and analyze data from these
sensors. This advanced approach allows Contoso Corp to detect anomalies
early and predict maintenance needs, thereby maintaining the efficiency
and reliability of their manufacturing processes.

Personas and scenario

- **Remy Morris** - Data Architect for Contoso Corp

- **Mark Brown** - Azure Databricks Data Engineer for Contoso Corp

- **Casey Jensen** - Data Analyst for Contoso Corp

   ![](./media/image1.png)

These personas will participate in the following sequential scenarios:

- Remy Morris, Data Architect for Contoso Corp, is responsible for
  designing the overall data architecture and strategy for Contoso Corp.
  He ensures that the data infrastructure is scalable, secure, and
  capable of handling real-time data streams from IoT sensors. Remy
  selects appropriate technologies and tools for data ingestion,
  processing, and storage.

- Mark Brown, Azure Databricks Engineer for Contoso Corp, works closely
  with Remy to set up the real-time data streaming and processing
  workflows. He provides an Azure Databricks workspace and creates Azure
  Databricks clusters and notebooks for data ingestion and processing.
  Mark implements real-time data streaming solutions using Azure
  Databricks.

- Casey Jensen, Data Analyst for Contoso Corp, analyzes the ingested and
  processed data to extract actionable insights. She creates reports and
  dashboards to help the operations team monitor machinery performance
  and predict maintenance needs.

![](./media/image2.png)

## Exercise 0: Understand the VM and the credentials

In this task, we will identify and understand the credentials that we
will be using throughout the lab.

1.  **Instructions** tab hold the lab guide with the instructions to be
    followed throughout the lab.

2.  **Resources** tab has got the credentials that will be needed for
    executing the lab.

    - **URL** – URL to the Azure portal

    - **Subscription** – This is the ID of the subscription assigned to
      you

    - **Username** – The user id with which you need to login to the
      Azure services.

    - **Password** – Password to the Azure login. Let us call this
      Username and password as Azure login credentials. We will use
      these creds wherever we mention Azure login credentials.

    - **Resource Group** – The **Resource group** assigned to you.

    \[!Alert\] **Important:** Make sure you create all your resources under
    this Resource group

     ![](./media/image3.png)

3.  **Help** tab holds the Support information. The **ID** value here is
    the **Lab instance ID** which will be used during the lab execution.

     ![](./media/image4.png)

## Exercise 1: Provision an Azure Databricks workspace

**(Led by Remy Morris, Data Architect)**

**Scenario: Laying the Foundation**

Remy needs to set up the foundational infrastructure for the real-time
data streaming solution. He decides to provision a new Azure Databricks
workspace, which will serve as the processing engine for IoT sensor
data.

***Tip**: If you already have an Azure Databricks workspace, you can
skip this procedure and use your existing workspace.*

This exercise includes a script to provision a new Azure Databricks
workspace. The script attempts to create a *Premium* tier Azure
Databricks workspace resource in a region in which your Azure
subscription has sufficient quota for the compute cores required in this
exercise; and assumes your user account has sufficient permissions in
the subscription to create an Azure Databricks workspace resource.

If the script fails due to insufficient quota or permissions, you can
try to create an Azure Databricks workspace interactively in the Azure
portal

## Task 0: Sync Host environment time

1.  In your VM, navigate and click in the **Search bar**, type
    **Settings** and then click on **Settings** under **Best match**.

     ![](./media/image5.png)

2.  On Settings window, navigate and click on **Time & language**.

    ![](./media/image6.png)

3.  On **Time & language** page, navigate and click on **Date & time**.

    ![](./media/image7.png)

4.  Scroll down and navigate to **Additional settings** section, then
    click on **Syn now** button. It will take 3-5 minutes to syn.

     ![](./media/image8.png)

5.  Close the **Settings** window.

    ![](./media/image9.png)

## Task 1: Create an Azure Databricks workspace

1.  Login to +++https://portal.azure.com+++ using the Azure login
    credentials. Search for **+++azure databricks+++** from the search
    bar and select it.

     ![](./media/image10.png)

2.  Select **+ Create**.

     ![](./media/image11.png)

3.  Create an **Azure Databricks** resource with the following settings:

    - **Subscription**: *Select the same Azure subscription that you
      used to create your Azure OpenAI resource*

    - **Resource group**: *The same resource group where you created
      your Azure Event Hubs*

    - **Region**: *Select the region East US 2*

    - **Name**: Enter the name as **databricksXXXX** (XXXX*A unique
      number of your choice)*

    - **Pricing tier**: *Premium* 


4.  Select **Review + create** and wait for deployment to complete. Then
    go to the resource and launch the workspace.

     ![](./media/image12.png)

5.  On the **Review** **+ create** tab, click on the **Create** button.

     ![](./media/image13.png)
     ![](./media/image14.png)

5.  Once created, click on **Go to resource**.

     ![](./media/image15.png)

6.  In the **Overview** page for your workspace, use the **Launch
    Workspace** button to open your Azure Databricks workspace in a new
    browser tab; signing in if prompted.

     ![](./media/image16.png)

## Exercise 2: Create a cluster

**(Led by Mark Brown, Azure Databricks Data Engineer)**

**Scenario: Building the Engine**

With the workspace ready, Mark takes over to set up the computing
environment needed to handle real-time data streams. He creates a
cluster that will power the data processing.

Azure Databricks is a distributed processing platform that uses Apache
Spark *clusters* to process data in parallel on multiple nodes. Each
cluster consists of a driver node to coordinate the work, and worker
nodes to perform processing tasks. In this exercise, you’ll create
a *single-node* cluster to minimize the compute resources used in the
lab environment (in which resources may be constrained). In a production
environment, you’d typically create a cluster with multiple worker
nodes.

**Tip**: If you already have a cluster with a 13.3 LTS or higher runtime
version in your Azure Databricks workspace, you can use it to complete
this exercise and skip this procedure.

### Task 1: Create a cluster in Databricks

1.  In the **Overview** page for your workspace, use the **Launch
    Workspace** button to open your Azure Databricks workspace in a new
    browser tab; signing in if prompted.

    **Tip**: As you use the Databricks Workspace portal, various tips and
    notifications may be displayed. Dismiss these and follow the
    instructions provided to complete the tasks in this exercise.

     ![](./media/image17.png)

2.  In the sidebar on the left, select the **(+) New** task, and then
    select **Cluster**.

    ![](./media/image18.png)

3.  In the **New Cluster** page, create a new cluster with the following
    settings:

    - **Cluster name**: Databricks Cluster

    - **Policy**: Unrestricted

    - **Cluster mode**: Single Node

    - **Access mode**: Single user (*with your user account selected*)

    - **Databricks runtime version**: 13.3 LTS (Spark 3.4.1, Scala 2.12)
      or later

    - **Use Photon Acceleration**: Selected

    - **Node type**: Standard_D4ds_v5

    - **Terminate after** *20* **minutes of inactivity**

     ![](./media/image19.png)

4.  Wait for the cluster to be created. It may take 5-7 minute.

    ![](./media/image20.png)
    ![](./media/image21.png)
    ![](./media/image22.png)

**Note**: If your cluster fails to start, your subscription may have
insufficient quota in the region where your Azure Databricks workspace
is provisioned. See CPU core limit prevents cluster creation for
details. If this happens, you can try deleting your workspace and
creating a new one in a different region. You can specify a region as a
parameter for the setup script like this: ./mslearn-databricks/setup.ps1
eastus

## Exercise 3: Create a notebook and ingest data

**(Led by Mark Brown, Azure Databricks Data Engineer)**

**Scenario: Bringing the Data to Life**

Mark needs to ingest data from IoT sensors placed across the
manufacturing floor. This data includes real-time readings of
temperature and humidity from various machines.

### Task 1: Ingesting data

1.  In the sidebar, use the **(+) New** link to create a **Notebook**.
    In the **Connect** drop-down list, select your cluster if it is not
    already selected. If the cluster is not running, it may take a
    minute or so to start.

     ![](./media/image23.png)

2.  In the first cell of the notebook, enter the following code, which
    uses *shell* commands to download data files from GitHub into the
    file system used by your cluster.

    ```
    %sh
    rm -r /dbfs/device_stream
    mkdir /dbfs/device_stream
    wget -O /dbfs/device_stream/device_data.csv https://github.com/MicrosoftLearning/mslearn-databricks/raw/main/data/device_data.csv
    ```
    ![](./media/image24.png)

3.  Use the **▸ Run Cell** menu option at the left of the cell to run
    it. Then wait for the Spark job run by the code to complete.

     ![](./media/image25.png)

## Exercise 4: Use delta tables for streaming data

**(Led by Mark Brown, Azure Databricks Data Engineer)**

**Scenario: Streaming the Data Flow**

Mark sets up a real-time streaming pipeline to continuously process data
from the IoT sensors. Using Delta Lake, he ensures data consistency and
enables efficient querying.

Delta lake supports *streaming* data. Delta tables can be a *sink* or
a *source* for data streams created using the Spark Structured Streaming
API. In this example, you’ll use a delta table as a sink for some
streaming data in a simulated internet of things (IoT) scenario. In the
next task, this delta table will work as a source for data
transformation in real time.

### Task 1: Define the Streaming Pipeline:

1.  In a new cell, run the following code to create a stream based on
    the folder containing the csv device data:

    ```
    from pyspark.sql.functions import *
    from pyspark.sql.types import *
    
    # Define the schema for the incoming data
    schema = StructType([
       StructField("device_id", StringType(), True),
       StructField("timestamp", TimestampType(), True),
       StructField("temperature", DoubleType(), True),
       StructField("humidity", DoubleType(), True)
    ])
    
    # Read streaming data from folder
    inputPath = '/device_stream/'
    iotstream = spark.readStream.schema(schema).option("header", "true").csv(inputPath)
    print("Source stream created...")
    
    # Write the data to a Delta table
    query = (iotstream
            .writeStream
            .format("delta")
            .option("checkpointLocation", "/tmp/checkpoints/iot_data")
            .start("/tmp/delta/iot_data"))
    ```
    ![](./media/image26.png)

2.  Use the **▸ Run Cell** menu option at the left of the cell to run
    it.

  > This delta table will now become the source for data transformation in
  > real time.
  >
  > Note: The code cell above creates the source stream. Therefore, the
  > job run will never change to a completed status. To manually stop
  > streaming, you can run query.stop() in a new cell.
  >
   ![](./media/image27.png)
## Exercise 5: Create a Delta Live Table Pipeline

**(Led by Mark Brown, Azure Databricks Data Engineer)**

**Scenario: Transforming Data for Analysis**

Mark now sets up Delta Live Tables to transform raw IoT data. This step
adds derived metrics such as temperature in Fahrenheit and standardized
humidity readings.

### Task 1: Define Delta Live Tables

A pipeline is the main unit for configuring and running data processing
workflows with Delta Live Tables. It links data sources to target
datasets through a Directed Acyclic Graph (DAG) declared in Python or
SQL.

1.  Select **Pipelines** in the left sidebar ,select **Create Pipeline**
    and then select **ETL pipeline**.

    ![](./media/image28.png)
   
    ![](./media/image29.png)

2.  In the **Create pipeline** page, create a new pipeline with the
    following settings. Select Create

    - **Pipeline name**: Ingestion Pipeline

    - **Product edition**: Advanced

    - **Pipeline mode**: Triggered

    - **Source code**: Leave it blank

    - **Storage options**: Hive Metastore

    - **Storage location**: dbfs:/pipelines/device_stream

    - **Target schema**: default

    ![](./media/image30.png)
    
    ![](./media/image31.png)

3.  Select **Create pipeline**.

     ![](./media/image32.png)

4.  Once the pipeline is created, open the link to the blank notebook
    under **Source code** in the right-side panel:

     ![](./media/image33.png)

5.  In the first cell of the blank notebook, enter (but don't run) the
    following code to create Delta Live Tables and transform the data:
    ```
    import dlt
    from pyspark.sql.functions import col, current_timestamp
    
    @dlt.table(
       name="raw_iot_data",
       comment="Raw IoT device data"
    )
    def raw_iot_data():
       return spark.readStream.format("delta").load("/tmp/delta/iot_data")
    
    @dlt.table(
       name="transformed_iot_data",
       comment="Transformed IoT device data with derived metrics"
    )
    def transformed_iot_data():
       return (
           dlt.read("raw_iot_data")
           .withColumn("temperature_fahrenheit", col("temperature") * 9/5 + 32)
           .withColumn("humidity_percentage", col("humidity") * 100)
           .withColumn("event_time", current_timestamp())
       )
    
    ```

   
    ![](./media/image34.png)

7.  Close the browser tab containing the notebook (the contents are
    automatically saved) and return to the pipeline.

     ![](./media/image35.png)

8.  Then select **Start**

     ![](./media/image36.png)

9.  Select **Ingestion Pipeline**

     ![](./media/image37.png)

10. Now ,the pipeline has successfully completed

     ![](./media/image38.png)

11. After the pipeline has successfully completed, go back to the
    recent **Delta Live Tables Ingestion** that you created first, and
    verify that the new tables have been created in the specified
    storage location by running the following code in a new cell:

    ```
    %sql
    SHOW TABLES
    ```



## Exercise 6: View results as a visualization

**(Led by Casey Jensen, Data Analyst)**

**Scenario: Extracting Insights**

Casey loads the transformed data to create visualizations that help the
operations team monitor equipment health in real time

1.  In the sidebar on the left, select the **Workspace** task and then
    select the first notebook

    ![](./media/image39.png)

2.  Click on the **Interrupt**

     ![](./media/image40.png)

3.  Add a new code cell and run the following code to load
    the transformed_iot_data into a dataframe:
    ```
    %sql
    SELECT * FROM transformed_iot_data
    ```

     ![](./media/image41.png)

     ![](./media/image42.png)

4.  Above the table of results, select **+** and then
    select **Visualization** to view the visualization editor

    ![](./media/image43.png)

5.  In the Visualization Editor tab, enter the following details and
    click on the **Save** button.

    - **Visualization type**: Line

    - **X Column**: timestamp

    - **Y Column**: *Add a new column and
      select* **temperature_fahrenheit**. *Apply
      the* **Sum** *aggregation*.

      ![](./media/image44.png)
      
      ![](./media/image45.png)

6.   View the resulting chart in the notebook.

      ![](./media/image46.png)

7.  Add a new code cell and enter the following code to stop the
    streaming query:

    ![](./media/image47.png)

    ![](./media/image48.png)

## Exercise 7 : Clean up

1.  Navigate to Azure portal home page, type **Resource groups** in the
    Azure portal search bar, navigate and click on **Resource
    groups** under **Services**.
   ![](./media/image49.png)

2.  Click on the assigned resource group.

      ![](./media/image50.png)

3.  Carefully select all the resources you’ve created, navigate to the
    command bar, and click on Delete

    **Important Note**: Don’t click on **Delete resource group**. If you
    don’t see the **Delete** option in the command bar, then click on the
    horizontal ellipsis

    ![](./media/image51.png)

4.  In the **Delete Resources** pane that appears on the right side,
    enter the **delete** and click on **Delete** button.

     ![](./media/image52.png)

5.  On **Delete confirmation** dialog box, click on D**elete** button.

     ![](./media/image52.png)
      ![](./media/image53.png)
       ![](./media/image54.png)

6.  Navigate to Azure portal home page, type **Resource groups** in the
    Azure portal search bar, navigate and click on **Resource
    groups** under **Services**.
    ![](./media/image49.png)

7.  Click on the NetworkWacherRG resource group.

     ![](./media/image56.png)

8.  In the **Resource group** home page, select the **delete resource
    group**

     ![](./media/image57.png)

9.  In the **Delete Resources** pane that appears on the right side,
    navigate to **Enter “resource group name” to confirm deletion**
    field, then click on the **Delete** button

    ![](./media/image58.png)

10. On **Delete confirmation** dialog box, click on D**elete** button.

    ![](./media/image59.png)
    ![](./media/image60.png)

**Summary:**

This use case you through setting up an end-to-end streaming pipeline
using Azure Databricks. You'll learn how to ingest streaming data,
process it in real-time, and store the results. The steps include
configuring the Databricks environment, creating a streaming job, and
using Structured Streaming to process data.

