# Lab 06: Build an asset management data app with PowerTable

## Scenario

Acme Beverages needs an Asset Management data app. The company tracks its asset data in spreadsheets. This approach makes the data hard to manage and keep consistent.

To solve this problem, Acme Beverages moves the asset data into a database. The company then manages the data with a mini data app in Microsoft Fabric that uses Fabric Plan. The solution uses these key components:
- Fabric SQL database: Stores the assets, employees, and locations tables that the app uses for data management.
- Fabric Plan item: Holds the PowerTable app and connects it to the system database.
- PowerTable sheets: Provide a spreadsheet-like interface to view, format, edit, import, and audit the data in the database.


In this lab, you're a data professional at Acme Beverages. You build the Asset Management app that replaces the spreadsheets. The app lets users manage asset data directly in Microsoft Fabric.

## Introduction

In this lab, you build an Asset Management data app in Microsoft Fabric. You create a Fabric SQL database and a Fabric Plan item, and then you create PowerTable sheets for the Assets, Employees, and Locations tables from an Excel spreadsheet. You then configure the Assets sheet with formatted columns, a conditional formatting rule, lookups, and a formula column. Finally, you manage data with the Row Editor, Form Editor, Bulk Editor, Find and Replace, file import, and audit features of PowerTable.

### Objectives

After completing this lab, you will be able to:
- Create a Fabric SQL database for data management.
- Create a Fabric Plan item and set up its connection to the system database.
- Create PowerTable sheets for the Assets, Employees, and Locations tables by importing data from an Excel file.
- Configure the Assets sheet with image and single-select columns, a conditional formatting rule, lookup values, and a formula column.
- Update and insert data by using the Row Editor, Form Editor, Bulk Editor, and Find and Replace.
- Import new records from a file and review changes in the audit logs.


## Architecture

The learner uploads asset, employee, and location data from assets.xlsx through the Fabric Plan item's PowerTable sheets. A connection using an organizational account links the Plan item to the Fabric SQL database, which stores the tables. Users then view, format, edit, import, and audit the data in the PowerTable sheets.

![Architecture diagram](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/architecture.png)

### Prerequisites

To complete this lab, you need:
- Access to a Microsoft Fabric training workspace with permission to create items.
- Permission to create **SQL database** and **Plan** items in the workspace.
- The **assets.xlsx** file, which contains the asset, employee, location, and import data that you use in this lab.
- An organizational account to create the Fabric Plan connection.


## Exercise 1: Create the Fabric SQL database and Fabric Plan item

In this exercise, you create the Fabric SQL database that the PowerTable app uses for data management and the Fabric Plan item that holds the PowerTable app.

### Task 1: Create a workspace

In this task, you create the Microsoft Fabric workspace for the lab. The workspace holds the Fabric SQL database and the Fabric Plan item that you create later.

1. In Microsoft Fabric, on the **Home** page, under **Welcome to Fabric**, select **New workspace**.

    ![Screenshot 1](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image1.png)

    The **Create a workspace** pane opens.

1. In the **Create a workspace** pane, in the **Name** field, enter the following value:

    `FabricPlan-Day2-568795`

    The **This name is available** message appears under the `Name` field.

    ![Screenshot 2](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image2.png)

    >[!Note] The workspace name must be unique. If the name isn't available, add more digits to the end of the name, and use that name in the rest of the lab.

1. Under **Advanced**, verify that your account appears in the **Contact list** field.

1. Select **Apply**.

    Fabric creates the workspace and opens the **FabricPlan-Day2-568795** workspace page.


### Task 2: Create a folder in the workspace

In this task, you create a folder in the workspace. The folder keeps the lab items organized in one place.

1. On the **FabricPlan-Day2-568795** workspace page, select **New folder**.

    ![Screenshot 3](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image3.png)

    The **New folder** dialog box opens.

1. In the **New folder** dialog box, enter the following folder name:

    `FabricPlan-Day2`

1. Select **Create**.

    ![Screenshot 4](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image4.png)

    The **FabricPlan-Day2** folder appears in the workspace.


### Task 3: Create a Fabric SQL database

In this task, you create a SQL database named `fabric_plan_training` in the training workspace. You use this database to store the data that you work with later in the lab.

1. Go to the **FabricPlan-Day2** folder in the **FabricPlan-Day2-568795** workspace.

1. On the toolbar, select **New item**.

    The **New item** pane opens.

1. In the **New item** pane, select **All items**.

1. In the **Filter by keyword** box, enter `SQL`.

1. Select **SQL database**.

    The **New SQL database** dialog box opens.
       ![Screenshot 5](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image5.png)

1. In the **New SQL database** dialog box, enter the following details:

    - **Name** - `fabric_plan_training`


1. Select **Create**.

    ![Screenshot 6](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image6.png)

    The database opens on the **Home** tab, and the **Build your database** page appears.

1. In the **Explorer** pane, verify that the **fabric_plan_training** database appears.

    ![Screenshot 7](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image7.png)


### Task 4: Create a Fabric Plan item

In this task, you start creating a Plan item in the same workspace as the **fabric_plan_training** database. You configure the Plan item and its connection in the next exercise.

1. Go to the **FabricPlan-Day2** folder in the **FabricPlan-Day2-568795** workspace.

    The **fabric_plan_training** SQL database and the **fabric_plan_training** SQL analytics endpoint appear in the item list.

1. On the toolbar, select **New item**.

    The **New item** pane opens.

1. In the **New item** pane, select **All items**.

1. In the **Filter by keyword** box, enter `Plan`.

1. Select **Plan**. The **New Plan** dialog box opens.

    ![Screenshot 8](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image8.png)

1. In the **New Plan** dialog box, enter the following details:

    - **Name** - `Asset Management`
    - **Location** - Keep the default workspace location.


1. Select **Create**.

    ![Screenshot 9](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image9.png)

    The **Asset Management** Plan item opens on the **Home** tab, and the **How Would You Like To Get Started?** page appears.


## Exercise 2: Create the PowerTable sheets

In this exercise, you create the **Assets**, **Employees**, and **Locations** PowerTable sheets by using data that you import from an Excel spreadsheet.

### Task 1: Create the Assets PowerTable sheet

In this task, you create the **Assets** PowerTable sheet in the **Asset Management** Plan item. You also set up the connection that links the Plan item to the system database so that workspace viewers can use the Plan item.

1. On the **How Would You Like To Get Started?** page, under **Create new sheet**, select **PowerTable**.

    ![Screenshot 10](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image10.png)

    The **New PowerTable Sheet** dialog box opens.

1. In the **New PowerTable Sheet** dialog box, enter the following details:

    - **Name** - `Assets`


1. Select **Create**.

    ![Screenshot 11](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image11.png)

    The **Assets** sheet opens on the **Welcome to PowerTable** page, and the **Extra configuration recommended** banner appears at the top of the page.

    >[!Note] You set up the connection only one time for each Fabric Plan item. This connection links the Plan item to the system database. It also lets users with the Viewer workspace role use this Plan item.

1. On the **Extra configuration recommended** banner, select **Set up connection**. The **Select Fabric SQL Connection** dialog box opens.

    ![Screenshot 12](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image12.png)

1. In the **Select Fabric SQL Connection** dialog box, select **+ Create Connection**.

    ![Screenshot 13](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image13.png)

    The **Create New Connection** dialog box opens.

    >[!Note] If a Fabric SQL connection already exists, you can select the drop-down arrow in the **Select a Connection** box and select that connection instead. In that case, skip to step 8.

1. In the **Create New Connection** dialog box, under **Connection credentials**, configure the following settings:

    - **Connection** - **Create new connection**
    - **Connection name** - `FabricSql-outh`
    - **Authentication kind** - **Organizational account**

    ![Screenshot 14](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image14.png)

    >[!Note] Under **You are currently signed in as**, your account appears by default. To use a different account, select **Switch account**.


1. Select **Create**.

    The **Select Fabric SQL Connection** dialog box opens, and the new connection appears in the **Select a Connection** box.

1. In the **Select Fabric SQL Connection** dialog box, select **Connect**.

    ![Screenshot 15](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image15.png)

    The dialog box closes, and the PowerTable **Home** page opens.

1. Verify that the **Connection saved successfully!** message appears in the upper-right corner of the page.

    ![Screenshot 16](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image16.png)

    The connection is saved, and PowerTable is connected to the Fabric SQL database.

1. On the PowerTable **Home** page, select **Create a New App**.

    ![Screenshot 17](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image17.png)

    The **Select Fabric SQL Connection** dialog box opens.

1. In the **Select a Connection** list, select **FabricSql-outh**.

1. Under **Select Database From**, verify that **Database Item** is selected.

1. In the **Database Name** box, select the search icon.

    ![Screenshot 18](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image18.png)

    The **OneLake catalog** opens on the **Select destination database** page.

1. In the list of databases, select **fabric_plan_training**, the database that you created earlier.

    ![Screenshot 19](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image19.png)

    >[!Note] Make sure that you select **fabric_plan_training** and not **\_\_fabric_plan_sys**.

1. Select **Add**.

    The **Select Fabric SQL Connection** dialog box opens again, and **fabric_plan_training** appears in the **Database Name** box.

1. In the **Select Fabric SQL Connection** dialog box, select **Connect**.

    ![Screenshot 20](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image20.png)

    The dialog box closes, and PowerTable connects the new app to the **fabric_plan_training** database.

1. In the **Select Table** dialog box, under **Select Table**, select **New Table**.

1. In the **Schema** list, select **dbo**.

1. In the **Table Name** box, enter `assets`.

1. Under **Import Data**, select **Upload File**.

    ![Screenshot 21](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image21.png)

1. In the **Import Type** list, select **Excel**.

    The **Upload File** area appears below the **Import Type** list.

1. In the **Upload File** area, select **Click or drop files here**.

    ![Screenshot 22](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image22.png)

    The **Open** dialog box opens.

1. In the **Open** dialog box, go to **This PC** > **Local Disk (D:)** > **Labfiles**.

1. Select the **assets.xlsx** file.

1. Select **Open**.

    ![Screenshot 23](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image23.png)

    The **assets.xlsx** file appears under **Uploaded File** in the **Select Table** dialog box.

    >[!Note] If you uploaded the wrong file, select **Change** next to the file name, and then select a different file. You can upload only .xlsx and .xls files, and the file size must not exceed 25 MB.

1. Select **Next**.

    ![Screenshot 24](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image24.png)

    The wizard moves to the **Preview Data** step.

1. In the **Preview Data** section, in the worksheet list, select **Sheet1**.

    The preview table shows the asset records from **Sheet1**, and **Total Row Count** shows **99**.

1. Select **Next**.

    ![Screenshot 25](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image25.png)

    The **Configure Table** page opens and lists the 17 columns from the file.

1. In the **Asset Id** row, select the **Identity Column** check box.

    The **Asset Id** column now has **INT** as its data type, with **Primary Key** and **Identity Column** selected.

    >[!Note] An identity column generates the ID value automatically for each new record.

1. Select **Finish**.

    ![Screenshot 26](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image26.png)

    The **Assets** sheet opens and shows the imported data. The status bar shows **Rows: 99** and **Columns: 17**.

1. On the **Home** tab, select **Save**.

    ![Screenshot 27](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image27.png)

    Your changes, including the **Assets** sheet, are saved.


### Task 2: Create the Employees PowerTable sheet

In this task, you create a second PowerTable sheet for employee data. A separate sheet keeps the employee records apart from the asset records in the **Assets** sheet.

1. On the **Home** tab, select **New PowerTable Sheet**.

    ![Screenshot 28](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image28.png)

    The **New PowerTable Sheet** dialog box opens.

1. In the **New PowerTable Sheet** dialog box, for **Name**, enter `Employees`.

1. Select **Create**.

    ![Screenshot 29](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image29.png)

    A new **Employees** tab opens and shows the **Welcome to PowerTable. Let's get started.** page.

1. Select **Create a New App**.

    ![Screenshot 30](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image30.png)

    The **Select Fabric SQL Connection** dialog box opens.

1. In the **Select Fabric SQL Connection** dialog box, in the **Select a Connection** list, select **FabricSql-outh**.

1. Next to **Database Name**, select the search icon.

    The **OneLake catalog** opens.

1. In the **OneLake catalog**, select **fabric_plan_training**.

1. Select **Add**.

    The **Database Name** box shows **fabric_plan_training**.

    >[!Note] Use the connection and the database that you created earlier in this lab.

1. Select **Connect**.

    ![Screenshot 31](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image31.png)

    The **Select Table** page of the wizard opens.

1. On the **Select Table** page, configure the following settings:

    - **Select Table** - **New Table**
    - **Schema** - **dbo**
    - **Table Name** - `employees`
    - **Import Data** - **Upload File**
    - **Import Type** - **Excel**

    ![Screenshot 32](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image32.png)


1. In the **Upload File** section, select **Click or drop files here**.

    A file browser opens.

1. In the file browser, go to **This PC** > **Local Disk (D:)** > **Labfiles**.

1. Select the **assets.xlsx** file.

1. Select **Open**.

    The **assets.xlsx** file uploads to the **Upload File** section of the wizard.

    >[!Note] You can upload only .xlsx and .xls files. The file size must not exceed 25 MB.

1. Select **Next**.

1. In the **Preview Data** section, in the worksheet list, select **Sheet2**.

    The preview shows the employee data from the **Sheet2** worksheet, and **Total Row Count** shows **30**.

1. Select **Next**.

    ![Screenshot 33](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image33.png)

    The **Configure Table** page opens and lists the nine columns of the **employees** table.

1. On the **Configure Table** page, in the **Employee Id** row, select the **Identity Column** check box.

    >[!Note] The **Primary Key** check box for the **Employee Id** column is already selected. Keep the existing settings for the other columns.

1. Select **Finish**.

    ![Screenshot 34](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image34.png)

    The wizard closes, and the **Employees** sheet shows the imported employee records. The status bar shows **Rows: 30** and **Columns: 9**.

1. On the **Home** tab, select **Save**.

    ![Screenshot 35](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image35.png)

    Your changes, including the new **Employees** sheet, are saved.


### Task 3: Create the Locations PowerTable sheet

In this task, you add a new PowerTable sheet for the location data. A separate sheet keeps the location records in their own table, so you can manage them independently of the employee records.

1. Select **Home** > **New PowerTable Sheet**.

    ![Screenshot 36](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image36.png)

    The **New PowerTable Sheet** dialog box opens.

1. In the **New PowerTable Sheet** dialog box, for **Name**, enter `Locations`.

1. Select **Create**.

    ![Screenshot 37](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image37.png)

    A new **Locations** tab opens and shows the **Welcome to PowerTable. Let's get started.** page.

1. Select **Create a New App**.

    ![Screenshot 38](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image38.png)

    The **Select Fabric SQL Connection** dialog box opens.

1. In the **Select Fabric SQL Connection** dialog box, in the **Select a Connection** list, select **FabricSql-outh**.

1. Next to **Database Name**, select the search icon.

    The **OneLake catalog** opens.

1. In the **OneLake catalog**, select **fabric_plan_training**.

1. Select **Add**.

    The **Database Name** box shows **fabric_plan_training**.

    >[!Note] Use the same connection and database that you created earlier in this lab.

1. Select **Connect**.

    ![Screenshot 39](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image39.png)

    The **Select Table** page of the wizard opens.

1. On the **Select Table** page, configure the following settings:

    - **Select Table** - **New Table**
    - **Schema** - **dbo**
    - **Table Name** - `locations`
    - **Import Data** - **Upload File**
    - **Import Type** - **Excel**


1. Under **Upload File**, select **Click or drop files here**.

    ![Screenshot 40](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image40.png)

    A file browser opens.

1. In the file browser, go to **This PC** > **Local Disk (D:)** > **Labfiles**.

1. Select the **assets.xlsx** file.

1. Select **Open**.

    The **assets.xlsx** file uploads to the wizard.

1. Select **Next**.

    The **Preview Data** page opens.

1. In the **Preview Data** section, in the worksheet list, select **Locations**.

1. In the **Source Table Name** box, enter `locations`.

1. Keep the default values for **Start Cell** and **End Cell**.

1. Verify that the check box next to the **Locations** label is selected.

1. Select the chevron next to the check box to expand the data preview.

    The preview shows the rows and columns from the **Locations** worksheet.

1. Select **Next**.

    ![Screenshot 41](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image41.png)

    The **Configure Table** page opens.

1. On the **Configure Table** page, select the **Locations** tab.

1. In the **Location Id** row, select the **Identity Column** check box.

    ![Screenshot 42](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image42.png)

1. Select **Finish**.

    The **Creating Tables** dialog box opens. Each table shows **Created successfully** when the import completes.

1. In the **Creating Tables** dialog box, select **Done**.

    ![Screenshot 43](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image43.png)

    The **Locations** tab shows the data from the **locations** table. The table includes the **Location Id**, **Location**, **Description**, **Building**, and **Floor** columns.

1. On the **Home** tab, select **Save**.

    ![Screenshot 44](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image44.png)

    Your changes are saved, including the new **Locations** sheet.


## Exercise 3: Set up the Assets sheet

In this exercise, you format the **Assets** sheet columns, add a conditional formatting rule, configure lookups, and add a formula column.

### Task 1: Format the Logo URL column

In this task, you change the input type of the **Logo URL** column so that the sheet renders the logo images instead of raw text. You also give the column a shorter display name.

1. In the **Asset Management** app that you created, expand the **Explorer** pane on the left side.

1. In the **Explorer** pane, select the **Assets** sheet if it isn't already open.

    ![Screenshot 45](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image45.jpg)

    The **Assets** sheet opens.

1. Select the collapse icon (**«**) to collapse the **Explorer** pane.

1. Hover over the **Logo URL** column header.

1. In the **Logo URL** column header, select the ellipsis (**...**).

1. On the context menu, select **Edit**.

    ![Screenshot 46](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image46.jpg)

    The **Logo URL** side panel opens on the right, on the **General** tab.

1. On the **General** tab, in the **Input Type** list, select **Image**.

    ![Screenshot 47](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image47.jpg)

    The **Input Type** box shows **Image**. This setting tells PowerTable to render the stored URL as a thumbnail instead of raw text.

1. In the **Logo URL** side panel, select the **Display** tab.

1. In the **Display Name** box, enter `Logo`.

1. Select **Save**.

    ![Screenshot 48](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image48.jpg)

    The column header changes to **Logo**, and the column cells show the logo images.

1. Drag the right edge of the **Logo** column header to resize the column.

1. In the top-right corner of the toolbar, select **Save**.

    ![Screenshot 49](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image49.jpg)

    PowerTable saves your changes to the sheet.

1. Verify that the **Assets** sheet shows the manufacturer logos in the **Logo** column.

    ![Screenshot 50](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image50.jpg)


### Task 2: Format the Asset Type and Status columns

In this task, you convert the **Asset Type** and **Status** columns to single-select lists that use the values already in the data. A single-select list restricts entry to existing values, which prevents typos and keeps filtering reliable.

1. Hover over the **Asset Type** column header.

1. In the **Asset Type** column header, select the ellipsis (**...**).

1. In the menu, select **Edit**.

    ![Screenshot 51](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image51.jpg)

    The **Asset Type** pane opens on the right, with the **General** tab selected.

1. On the **General** tab, in the **Input Type** list, select **Single Select**.

1. In the **Values Type** list, select **Distinct Values**.

1. Select **Save**.

    ![Screenshot 52](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image52.jpg)

    The **Asset Type** column now accepts only values that already exist in the column.

1. Hover over the **Status** column header.

1. In the **Status** column header, select the ellipsis (**...**).

1. In the menu, select **Edit**.

    The **Status** pane opens on the right, and the **General** tab is selected.

1. On the **General** tab, in the **Input Type** list, select **Single Select**.

1. In the **Values Type** list, select **Distinct Values**.

1. Select **Save**.

    The **Status** column now shows its values as single-select tags.

1. In the top-right corner of the toolbar, select **Save**.

    ![Screenshot 53](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image53.jpg)

    Your changes to the sheet are saved.

1. Review the **Assets** sheet to confirm that the **Asset Type** and **Status** columns show their values as tags.

    ![Screenshot 54](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image54.jpg)


### Task 3: Create a conditional formatting rule for retired assets

In this task, you add a formatting rule that styles every row whose status is **Retired**. This visual highlight helps users find retired assets without filtering the sheet.

1. In the toolbar, select the **Format** tab.

1. Select **Format Rules**.

1. In the **Format Rules** menu, select **Create Rule**.

    ![Screenshot 55](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image55.jpg)

    The **Create Formatting Rule** pane opens on the right.

1. In the **Title** box, enter `Retired`.

1. In the **Apply To** list, select **Rows**.

1. Under **Condition If**, in the column list, select **Status**.

1. In the operator list, select **is**.

1. In the value list, select **Retired**.

1. Under **Style**, select the **Italic** (**I**) button.

1. In the **Fill Color** list, select a gray color.

1. In the **Font Color** list, select a red color.

1. Select **Apply**.

    ![Screenshot 56](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image56.jpg)

    The **Retired** rule is applied, and the **Manage Rule** pane opens. Every row with the **Retired** status appears in italic red text on a gray background.

1. In the **Manage Rule** pane, confirm that the **Retired** rule is turned on.

1. Select **X** to close the **Manage Rule** pane.

    ![Screenshot 57](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image57.jpg)

    The **Manage Rule** pane closes.

1. In the toolbar, select **Save**.

    ![Screenshot 58](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image58.jpg)

    Your formatting changes to the **Assets** sheet are saved.

1. Review the **Assets** sheet to confirm that every row with the **Retired** status appears in italic red text on a gray background.

    ![Screenshot 59](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image59.jpg)


### Task 4: Configure lookup values for the Assigned To and Location columns

In this task, you point the **Assigned To** and **Location** columns at the **employees** and **locations** tables. The columns then display readable names instead of ID numbers, which makes the asset data easier to understand.

1. In the **Assets** sheet, hover over the **Assigned To** column header.

1. In the **Assigned To** column header, select the ellipsis (**...**).

1. On the context menu, select **Edit**.

    ![Screenshot 60](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image60.jpg)

1. On the **General** tab of the **Assigned To** side panel, set the following values:

    - **Input Type** - **Single Select**
    - **Values Type** - **Lookup**
    - **Lookup Schema** - **dbo**
    - **Lookup Table** - **employees**
    - **Lookup Key Column** - **Employee Id**
    - **Lookup Display Column** - **Full Name**

    ![Screenshot 61](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image61.jpg)

    The lookup pulls its values from the **employees** table. The **Assets** sheet shows employee names but still stores the underlying **Employee Id** value.


1. In the **Assigned To** side panel, select the **Display** tab.

1. In the **Display Name** field, enter the following value:

    `Assigned Employee`

    ![Screenshot 62](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image62.jpg)

1. Select **Save**.

    The column header changes to **Assigned Employee**, and the cells show employee names instead of ID numbers.

1. In the **Assets** sheet, hover over the **Location** column header.

1. In the **Location** column header, select the ellipsis (**...**).

1. On the context menu, select **Edit**.

    ![Screenshot 63](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image63.jpg)

    The **Location** side panel opens.

1. On the **General** tab of the **Location** side panel, set the following values:

    - **Input Type** - **Single Select**
    - **Values Type** - **Lookup**
    - **Lookup Schema** - **dbo**
    - **Lookup Table** - **locations**
    - **Lookup Key Column** - **Location Id**
    - **Lookup Display Column** - **Location**

    ![Screenshot 64](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image64.jpg)


1. Select **Save**.

    The **Location** column shows location names from the **locations** table instead of ID numbers.

1. In the toolbar, select **Save**.

    ![Screenshot 65](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image65.jpg)

    Your changes are saved.


### Task 5: Add a formula column to calculate the end-of-life date

In this task, you add a formula column that calculates the expected end-of-life date for each asset from its purchase date and expected lifetime. You can then plan asset replacements without calculating dates by hand.

1. Select the **PowerTable** tab.

1. On the **PowerTable** tab, select the drop-down arrow next to **Insert Column**.

1. In the drop-down menu, select **Formula Column**.

    ![Screenshot 66](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image66.jpg)

    The **Add Formula Column** pane opens on the **General** tab.

1. On the **General** tab, enter the following values:

    - **Column Name** - `Expected EOL Date`
    - **Formula** - `DATEADD([Purchase Date], (365*[Expected Lifetime In Years]))`

    ![Screenshot 67](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image67.jpg)

    >[!Important] Don't copy and paste the formula, because a pasted formula doesn't work. Type the formula manually so that PowerTable correctly links the column references. To insert a column reference while you type, press **Ctrl+Space**.


1. In the **Add Formula Column** pane, select **Save**.

    The **Expected EOL Date** column appears in the table and shows the calculated end-of-life date for each asset.

1. In the toolbar, select **Save**.

    ![Screenshot 68](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image68.jpg)

    The new formula column is saved.

1. Review the **Assets** sheet.

    ![Screenshot 69](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image69.jpg)

    The **Expected EOL Date** column shows a calculated date for each asset, and retired assets appear in red italic text on a gray background.


## Exercise 4: Set up the Assets sheet

<!-- Editor note: This exercise duplicates Exercise 3 (same title, tasks, columns, rule, and formula). Delete Exercise 4 and all its tasks from the guide. Then renumber the later exercises (Exercise 5 becomes Exercise 4) and update any cross-references. -->

In this exercise, you format the **Assets** sheet columns, add a conditional formatting rule, configure lookups, and add a formula column.

### Task 1: Format the Logo URL column

In this task, you change the **Logo URL** column so that it shows each manufacturer's logo as an image instead of a URL. You also give the column a shorter display name to make the sheet easier to read.

1. Hover over the **Logo URL** column header.

1. In the **Logo URL** column header, select the ellipsis (**...**).

1. In the menu, select **Edit**.

    ![Screenshot 70](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image70.png)

    The **Logo URL** dialog opens on the **General** tab.

1. On the **General** tab, select the **Input Type** list.

1. In the **Input Type** list, select **Image**.

    ![Screenshot 71](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image71.png)

    >[!Note] The **Image** input type supports direct image URLs and Base64 strings that start with data:image. The **Data Type** stays **VARCHAR**.

1. In the **Logo URL** dialog, select the **Display** tab.

1. In the **Display Name** field, enter `Logo`.

1. Select **Save**.

    ![Screenshot 72](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image72.png)

    The column header changes to **Logo**, and the column shows the manufacturer logos as images.

1. On the **PowerTable** tab, select **Save**.

    ![Screenshot 73](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image73.png)

    PowerTable saves the column changes to the sheet.

1. Verify that the **Assets** sheet shows the manufacturer logos in the **Logo** column.

    ![Screenshot 74](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image74.png)


### Task 2: Format the Asset Type and Status columns

In this task, you change the **Asset Type** and **Status** columns to single-select lists that use the distinct values already in each column. A single-select list keeps new entries consistent with the existing values.

1. In the **Assets** sheet, hover over the **Asset Type** column header.

    The ellipsis (**...**) appears in the column header.

1. In the **Asset Type** column header, select the ellipsis (**...**).

1. In the menu, select **Edit**.

    ![Screenshot 75](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image75.png)

    The **Asset Type** dialog opens on the **General** tab.

1. On the **General** tab, in the **Input Type** list, select **Single Select**.

1. In the **Values Type** list, select **Distinct Values**.

1. Select **Save**.

    ![Screenshot 76](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image76.png)

    The dialog closes, and the **Asset Type** column offers a list of its distinct values when you edit a cell.

1. In the **Assets** sheet, hover over the **Status** column header.

    The ellipsis (**...**) appears in the column header.

1. In the **Status** column header, select the ellipsis (**...**).

1. In the menu, select **Edit**.

    The **Status** dialog opens on the **General** tab.

1. On the **General** tab, in the **Input Type** list, select **Single Select**.

1. In the **Values Type** list, select **Distinct Values**.

1. Select **Save**.

    The dialog closes, and the **Status** column offers a list of its distinct values when you edit a cell.

1. On the **PowerTable** tab, select **Save**.

    ![Screenshot 77](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image77.png)

    PowerTable saves the column changes to the sheet.

1. Verify that the **Assets** sheet shows the **Asset Type** and **Status** values as colored labels.

    ![Screenshot 78](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image78.png)


### Task 3: Create a conditional formatting rule for retired assets

In this task, you create a conditional formatting rule that highlights every asset with the **Retired** status. The highlight helps users find retired assets in the sheet without filtering the data.

1. In the toolbar, select the **Format** tab.

1. Select **Format Rules** > **Create Rule**.

    ![Screenshot 79](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image79.png)

    The **Create Formatting Rule** dialog opens.

1. In the **Create Formatting Rule** dialog, configure the following settings:

    - **Title** - `Retired`
    - **Apply To** - **Rows**


1. Under **Condition If**, select the following values:

    - Column - **Status**
    - Operator - **is**
    - Value - **Retired**


1. Under **Style**, select the **Italic** (**I**) button.

1. In the **Fill Color** list, select a gray color.

1. In the **Font Color** list, select a red color.

    ![Screenshot 80](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image80.png)

1. Select **Apply**.

    The **Manage Rule** dialog opens, and rows with the **Retired** status appear in red italic text on a gray background.

1. In the **Manage Rule** dialog, select the close (**X**) button.

    ![Screenshot 81](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image81.png)

    The **Manage Rule** dialog closes, and the **Retired** rule stays turned on.

1. On the **PowerTable** tab, select **Save**.

    ![Screenshot 82](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image82.png)

    PowerTable saves the formatting rule to the sheet.

1. In the **Assets** sheet, confirm that rows with the **Retired** status appear in red italic text on a gray background, for example **IT-1245**, **IT-1246**, **IT-1247**, and **IT-1255**.

    ![Screenshot 83](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image83.png)


### Task 4: Configure lookup values for the Assigned To and Location columns

In this task, you configure the **Assigned To** and **Location** columns to show lookup values instead of ID numbers. Lookup values make the sheet easier to read and help users enter consistent data.

1. In the **Assets** sheet, hover over the **Assigned To** column header.

1. In the **Assigned To** column header, select the ellipsis (**...**) > **Edit**.

    ![Screenshot 84](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image84.png)

    The column settings for **Assigned To** open.

1. In the **Assigned To** dialog, on the **General** tab, configure the following settings:

    - **Input Type** - **Single Select**
    - **Values Type** - **Lookup**
    - **Lookup Schema** - **dbo**
    - **Lookup Table** - **employees**
    - **Lookup Key Column** - **Employee Id**
    - **Lookup Display Column** - **Full Name**

    ![Screenshot 85](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image85.png)

    The **Assigned To** column shows employee names instead of employee ID numbers.


1. In the **Assets** sheet, hover over the **Location** column header.

1. In the **Location** column header, select the ellipsis (**...**) > **Edit**.

    The **Location** dialog opens.

1. In the **Location** dialog, on the **General** tab, configure the following settings:

    - **Input Type** - **Single Select**
    - **Values Type** - **Lookup**
    - **Lookup Schema** - **dbo**
    - **Lookup Table** - **locations**
    - **Lookup Key Column** - **Location Id**
    - **Lookup Display Column** - **Location**

    The dialog closes, and the **Location** column shows location names instead of location ID numbers.


1. On the **PowerTable** tab, select **Save**.

    ![Screenshot 86](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image86.png)

    PowerTable saves the lookup settings for both columns.


### Task 5: Add a formula column to calculate the end-of-life date

In this task, you add a formula column that calculates when each asset reaches its end of life. The column uses each asset's purchase date and expected lifetime, so users don't need to calculate the date by hand.

1. On the **PowerTable** tab, select the **Insert Column** drop-down arrow > **Formula Column**.

    ![Screenshot 87](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image87.png)

    The **Add Formula Column** dialog opens.

1. On the **General** tab, in the **Column Name** box, enter the following value:

    `Expected EOL Date`

1. In the **Formula** box, enter the following formula by typing it character by character:

    `DATEADD([Purchase Date], (365*[Expected Lifetime In Years]))`

    ![Screenshot 88](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image88.png)

    >[!Important] Don't copy and paste the formula. PowerTable references columns in a special way behind the scenes, so a pasted formula doesn't work. To insert a column reference while you type, press **Ctrl+Space**.

1. Select **Save**.

    The dialog closes, and the **Expected EOL Date** column appears in the **Assets** sheet with a calculated end-of-life date for each asset.

1. In the top-right corner of the toolbar, select **Save**.

    ![Screenshot 89](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image89.png)

    PowerTable saves your changes to the sheet.

1. Review the **Assets** sheet.

    ![Screenshot 90](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image90.png)

    The **Expected EOL Date** column shows a calculated date for each asset. Retired assets appear in red italic text on a gray background.


## Exercise 5: Manage data in PowerTable

In this exercise, you edit, import, and audit asset data in the **Assets** sheet by using the PowerTable editing tools.

### Task 1: Edit a single row with the Row Editor

In this task, you change the status of one asset and remove its employee assignment directly in the sheet. Single-row edits let you update individual records as quickly as in a spreadsheet.

1. In the **Assets** sheet, find the fourth row, which has the **Asset Tag** value **IT-1248**.

1. In the **IT-1248** row, select the **Status** cell.

    A drop-down list opens and shows the current value **In Use**.

1. Under **Select an option**, select **Available**.

    ![Screenshot 91](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image91.png)

    The **Status** cell for **IT-1248** shows **Available**.

1. In the **IT-1248** row, right-click the **Assigned To** cell, which contains **John Williams**.

1. In the shortcut menu, select **Clear Contents**.

    ![Screenshot 92](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image92.png)

    The **Assigned To** cell for **IT-1248** is now empty.

1. Select **Save to Database**.

    ![Screenshot 93](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image93.png)

    The **Save Changes?** dialog box opens.

    >[!Note] The **Save to Database**, **Preview Changes**, and **Discard Changes** buttons become available after you edit a row. **Preview Changes** shows the number of pending changes, for example **Preview Changes (1)**.

    >[!Note] You can use the keyboard or the mouse to move between rows and columns. You can also copy and paste values between rows and columns, and from other applications into the sheet.

1. In the **Save Changes?** dialog box, optionally select the **Don't show this again** check box so that you don't need to confirm every save.

1. Select **Proceed**.

    ![Screenshot 94](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image94.png)

    PowerTable saves your changes to the database.

1. Verify that the **Data saved successfully** message appears.

    ![Screenshot 95](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image95.png)

    The page refreshes and shows your changes. The **Save to Database**, **Preview Changes**, and **Discard Changes** buttons are unavailable again.


### Task 2: Insert a row with a form

In this task, you set PowerTable to open a form when you insert a row. A form shows all fields for the new record in one place. This helps you enter complete and consistent data.

1. In the **Assets** sheet, select the **PowerTable** tab on the ribbon.
1. On the **PowerTable** tab, select the arrow next to **Insert Row**.
1. Select the toggle next to **Insert Using Form By Default** to turn it on.

    ![Screenshot 96](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image96.png)

    The toggle turns green, and **Insert Row** now opens a form by default.

1. On the **PowerTable** tab, select **Insert Row**.

    >[!Note] Select the **Insert Row** button itself, not the arrow next to it.

    ![Screenshot 97](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image97.png)

    The **Record Details** pane opens with the **Form Editor** tab selected.

1. On the **Form Editor** tab, enter the following details for the new asset:

    - **Asset Type** - **Laptop**
    - **Asset Tag** - `IT-4000`
    - **Manufacturer** - `Lenovo`
    - **Features** - `OS: Windows 11 Pro`
    - **Model** - `IdeaPad Flex 5`
    - **Status** - **Available**
    - **Expected Lifetime In Years** - `3`
    - **Warranty Exp Date** - `06/25/2027`
    - **Serial Number** - `SN-202600001`


1. Select **Apply**.

    ![Screenshot 98](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image98.png)

    The new row appears at the bottom of the **Assets** sheet as a pending change.

1. Above the **Assets** sheet, select **Save to Database**.
1. If the **Save Changes?** dialog box opens, select **Proceed**.

    PowerTable saves the new row to the database.


### Task 3: Update a single row with the Form Editor

In this optional task, you use the **Form Editor** to change the values of one existing record. The form shows every field of the record in one pane, so you can update the record without scrolling across the sheet.

1. In the **Assets** sheet, select the row selector check box next to any row, for example, the row with the **Asset Tag** value **IT-1248**.

    The **Row** tab appears on the ribbon.

1. On the ribbon, select **Row** > **Manage Record**.

    The **Record Details** pane opens with the **Form Editor** tab selected.

1. In the **Assigned To** list, select **Kate Williams**.

1. In the **Status** list, select **In Use**.

1. Select **Apply**.

    ![Screenshot 99](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image99.png)

    The row updates in the sheet, and **Preview Changes (1)** appears above the sheet.

1. Verify that the **Save to Database**, **Preview Changes (1)**, and **Discard Changes** buttons are available above the sheet.

    ![Screenshot 100](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image100.png)

    >[!Note] Selecting **Apply** doesn't save the change to the database. It creates a pending change. You can combine pending changes with other edits, and then save, preview, or discard them together.


### Task 4: Update multiple rows with the Form Editor

In this optional task, you use the **Form Editor** to update the same fields across several records at once. You apply a shared change, such as a new location or status, to a group of assets in one step. When you edit multiple rows, only the fields that you edit change, and all other fields keep their current values.

1. In the **Assets** sheet, select the row selector check boxes next to multiple rows, for example, the rows with the **Asset Tag** values **IT-1249**, **IT-1250**, and **IT-1251**.

    The toolbar shows **3 Selected**.

1. On the ribbon, select **Row** > **Manage Record**.

    The **Record Details** pane opens with the **Form Editor** tab selected.

1. In the **Location** list, select **Depot**.

1. In the **Status** list, select **Under Maintenance**.

1. Select **Apply**.

    ![Screenshot 101](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image101.png)

    The selected rows show the new **Location** and **Status** values, and the count on the **Preview Changes** button increases.


### Task 5:Update multiple rows with Bulk Edit

In this optional task, you use **Bulk Edit** to apply several actions to a group of selected rows. Bulk actions can offset values, set new values, or clear cells, so you can make complex changes without editing each row.

1. In the **Assets** sheet, clear the check boxes of any rows that are still selected.

1. Select the row selector check boxes next to the rows with the **Asset Tag** values **<VALUE>**, **<VALUE>**, **<VALUE>**, and **<VALUE>**.

    The toolbar shows **4 Selected**.

1. On the ribbon, select **Row** > **Bulk Edit**.

    The **Bulk Edit** pane opens with **Action 1** ready to configure.

1. Under **Action 1**, configure the following settings to add 30 days to the current warranty expiration date:

    - **Field** - **Warranty Exp Date**
    - **Action** - **Offset Value**
    - Interval - **Day**
    - Offset by - **Add**
    - Add - `30`


1. Select **+ Add Action**.

    **Action 2** appears in the **Bulk Edit** pane.

1. Under **Action 2**, configure the following settings to set the status to Available:

    - **Field** - **Status**
    - Action - **Set cell value**
    - New Value - **Available**


1. Select **+ Add Action**.

    **Action 3** appears in the **Bulk Edit** pane.

1. Under **Action 3**, configure the following settings to clear the **Assigned To** value:

    - **Field** - **Assigned To**
    - Action - **Clear cell contents**


1. Select **Apply**.

    ![Screenshot 102](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image102.png)

    The selected rows update in the sheet, and the count on the **Preview Changes** button increases.


### Task 6: Preview changes and save to the database

In this optional task, you review pending changes, revert some of them, and then save the rest to the database. The **Preview Changes** and **Discard Changes** buttons are enabled whenever a change is pending. You can use them to check or undo edits before they reach the database.

1. Above the **Assets** sheet, select **Preview Changes**.

    ![Screenshot 103](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image103.png)

    The **Preview** page opens and lists all pending changes.

1. Review the **All**, **Added**, **Updated**, and **Deleted** filters at the top of the **Preview** page.

    Changed values appear with the previous value crossed out above the new value.

1. Select the row selector check boxes next to one or more rows whose pending changes you want to revert.

1. Select **Reset**.

    ![Screenshot 104](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image104.png)

    The pending changes for the selected rows are reverted.

    >[!Note] To revert all pending changes at once, select **Discard Changes** above the sheet instead.

1. Select **Back to PowerTable**.

    The **Assets** sheet opens again.

1. Verify that none of the row check boxes in the **Assets** sheet are selected.

    ![Screenshot 105](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image105.png)

    The remaining pending changes stay in the **Assets** sheet, ready to save.

1. Above the **Assets** sheet, select **Save to Database**.

1. If the **Save Changes?** dialog box appears, select **Proceed**.

    The pending changes are saved to the database. The **Save to Database** button is unavailable until you make another change.


### Task 7: Find and replace values

In this optional task, you use the **Find and Replace** feature to find a value in a column and replace it with another value. This feature lets you standardize data, such as manufacturer names, across many rows at once.

1. In the toolbar above the **Assets** sheet, select **Find and Replace**.

    ![Screenshot 106](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image106.png)

    The **Find and Replace** dialog box opens.

1. In the **Find and Replace** dialog box, configure the following settings:

    - **Find** - `Apple`
    - **Column** - **Manufacturer**
    - **Match Case** - Leave the check box cleared.
    - **Match entire cell contents** - Select the check box.
    - **Replace With** - `Apple Inc`

    ![Screenshot 107](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image107.png)


1. Select **Replace all**.

    A message appears, for example, **All occurrences have been replaced in 23 rows**. The **Preview Changes** button shows the number of pending changes.

1. Verify that the updated values appear in the **Manufacturer** column.

    ![Screenshot 108](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2006/media/image108.png)

    The updated cells are highlighted in the table.

1. In the upper-right corner of the **Find and Replace** dialog box, select **X**.

    The dialog box closes.

1. Above the **Assets** sheet, select **Save to Database**.

1. If the **Save Changes?** dialog box appears, select **Proceed**.

    The updated **Manufacturer** values are saved to the database.



## Summary

In this lab, you created a SQL database in Microsoft Fabric to store and manage your data. You also created a Fabric Plan item with PowerTable sheets for the **Assets**, **Employees**, and **Locations** tables.

You set up the **Assets** sheet with formatted columns and a conditional formatting rule for retired assets. You added lookups that display employee names and location names. You also added a formula column that calculates the expected end-of-life date.

Next, you updated and inserted data by using the **Row Editor**, the **Form Editor**, **Bulk Edit**, **Find and Replace**, and file import. Finally, you reviewed your changes in the audit logs.
