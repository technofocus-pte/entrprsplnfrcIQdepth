## Lab 08: Build an interactive financial performance dashboard with Intelligence Sheets

## Introduction

An **intelligence sheet** is a canvas-based analysis layer in Plan (preview) for Microsoft Fabric. It sits on top of a semantic model, a planning sheet, or a PowerTable sheet, and gives you a visualization library with 100+ charts, matrices, tables, KPI cards, Gantt charts, and filters, without leaving the planning environment.

In this lab, you import the **Enterprise Dataset** semantic model, create a Plan app, and build an interactive enterprise dashboard on an intelligence sheet. You compare **Actuals**, **Plan**, and **Forecast** with a variance chart, a trend line, KPI cards, and a matrix, connect all visuals to a date filter, and collaborate with comments.

## Scenario

A mid-to-large SaaS company sells software products across several regions and customer segments. Each month, the finance team builds slides and spreadsheets to answer the same questions for leadership:
- Which regions and countries are above or below plan and forecast?
- How do actuals compare with the forecast month by month?
- Which products are ahead of or behind plan?
- How do product categories perform against plan over time?


The team wants one live dashboard on top of the governed semantic model that everyone can filter by date and comment on. In this lab, you are a financial analyst who builds this **SaaS Enterprise - Financial Performance Dashboard** with an intelligence sheet in Microsoft Fabric.

### Objectives

After completing this lab, you will be able to:
- Import a semantic model, create a Plan (preview) app, and connect the app to the semantic model
- Create an intelligence sheet and describe its canvas controls, filter, bookmark, and parameter panes, and visualization types
- Build an integrated variance bar chart that compares Actuals with Forecast and Plan by region and country
- Build a grouped line chart, a KPI card with a product trellis, and a matrix with variance columns
- Connect all visuals to a date range with a Super Filter
- Add a dashboard header and collaborate with comments and @mentions


### Architecture

You import the **Enterprise Dataset** .pbix file into a Fabric workspace, which creates a Power BI semantic model and report. The **Financial-Plan** Plan app connects to the semantic model, and an intelligence sheet in the app reads the model's measures and dimensions to render the dashboard visuals. A Super Filter and page-level and global filters drive all visuals, and users collaborate on the dashboard with comments, bookmarks, and Reading view.

![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image1.png)

| **Component**                     | **Role in the lab**                                                                                         |
|-----------------------------------|-------------------------------------------------------------------------------------------------------------|
| Enterprise Dataset.pbix           | Sample file with 1.8 million rows of synthetic SaaS data                                                    |
| Enterprise Dataset semantic model | Star schema with 8 dimensions, 3 fact tables, and a **Measure** table (**Actuals**, **Plan**, **Forecast**) |
| Financial-Plan (Plan app)         | Hosts the intelligence sheet and connects to the semantic model                                             |
| Intelligence sheet                | Canvas with the variance chart, grouped line chart, KPI card, matrix, Super Filter, and header              |
| Filters, bookmarks, parameters    | Control the data shown and save analytical views                                                            |
| Comments                          | Context-aware collaboration with @mentions at the cell, row, column, or report level                        |

### Sample dataset

The **SaaS Enterprise** dataset models a mid-to-large software business across multiple products, regions, and customer segments. It contains 1.8 million rows of transactional data structured as a star schema. All figures are synthetic but calibrated to produce meaningful variance analysis, realistic trend lines, and visible differences between Actual, Plan, and Forecast.

| **Table type** | **Tables**                                                                                                                                               |
|----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Dimensions (8) | \[Dim\] Account, \[Dim\] Business Unit, \[Dim\] Customer Segment, \[Dim\] Date, \[Dim\] Department, \[Dim\] Geography, \[Dim\] Product, \[Dim\] Scenario |
| Facts (3)      | \[Fact\] Financial, \[Fact\] Invoice Line, \[Fact\] Operational                                                                                          |
| Measures       | **Measure** table - most measures in this lab come from **Measure \> Financial \> Base Measures**                                                        |

### Prerequisites

- Access to a Microsoft Fabric workspace on a Fabric capacity where you can create Plan (preview) items
- Permission to create and edit intelligence sheets
- The **Enterprise Dataset.pbix** file in **C:\LabFiles**.
- A second user in your organization (optional, for the @mention step)


## Exercise 1: Set up the environment

In this exercise, you create a workspace, import the Enterprise Dataset semantic model, create a Plan app, and connect the app to the semantic model.

### Task 1: Create a workspace

1. In the Fabric portal (+++https://app.fabric.microsoft.com/+++), on the **Home** page, select **New workspace**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image2.png)

1. In the **Create a workspace** pane, enter `FabricPlan-Day2-Financial-@lab.LabInstance.Id` as the **Name**, and then select **Advanced**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image3.png)

1. Under the workspace type, select **Fabric**, confirm your Fabric capacity, and then select **Apply**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image4.png)

    >[!Note] The screenshots show a workspace name that ends with a different number. Your workspace name ends with your own lab instance ID.


### Task 2: Import the sample semantic model

1. In the workspace, select **Import \> Report, Paginated Report or Workbook \> From this computer**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image5.png)

1. In the **Open** dialog box, go to **C:\LabFiles**, select **Enterprise Dataset**, and then select **Open**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image6.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image7.png)

    >[!Note] The file is about 60 MB, so the upload can take a minute or two. Don't close the browser tab while it uploads.

1. Verify that the **Enterprise Dataset** report and the **Enterprise Dataset** semantic model appear in the workspace.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image8.png)


### Task 3: Create a Plan app

1. In the workspace, select **+ New item**, search for `Plan`, and then select **Plan**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image9.png)

1. In the **New Plan** dialog box, enter `Financial-Plan` as the **Name**, confirm the **Location** is your workspace, and then select **Create**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image10.png)

    The plan opens to the **How Would You Like To Get Started?** screen. Under **Get data**, you can connect a semantic model or an Excel/CSV file, and under **Create new sheet**, you can create PowerTable, Planning, and Intelligence sheets.


### Task 4: Connect the semantic model

1. Under **Get data**, select **Semantic Model**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image11.png)

1. In the **Select Semantic Model** dialog box, keep **Semantic Model Item** selected, and then select the **Semantic Model** box.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image12.png)

1. In the **OneLake catalog**, select **Enterprise Dataset**, and then select **Add**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image13.png)

    >[!Note] Select the **Enterprise Dataset** item whose **Owner** is your lab user and **Location** is your workspace.

1. Confirm that **Semantic Model** shows **Enterprise Dataset**, and then select **Connect**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image14.png)

    >[!Note] If the dialog asks you to select a connection, select or create a shared cloud connection that uses your organizational account.

1. In the **Data** pane, expand **Semantic Model** and confirm that the **Measure**, **\[Dim\]**, and **\[Fact\]** tables are listed.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image15.png)


## Exercise 2: Explore the intelligence sheet

In this exercise, you create an intelligence sheet and take a quick tour of its controls before you build the dashboard.

### Task 1: Create an intelligence sheet

1. On the **Home** tab, select **New Intelligence Sheet**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image16.png)

1. In the **New Intelligence Sheet** dialog box, keep the name **Intelligence 1** (or enter a name of your choice), and then select **Create**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image17.png)

    The intelligence sheet opens with an empty canvas, the **Visualizations** pane, and the **Data** pane.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image18.png)


### Task 2: Review the canvas controls

1. Select the **Intelligence** tab and review the controls that apply to the whole canvas:

    | **Control**           | **Purpose**                                                 |
    |-----------------------|-------------------------------------------------------------|
    | **Refresh Data**      | Reloads data from the semantic model                        |
    | **Canvas**            | Sets page size, scaling unit, alignment, layers, and themes |
    | **Layout**            | Manages the canvas layout                                   |
    | **Elements**          | Adds text, shapes, buttons, and grid layouts                |
    | **Edit Interactions** | Defines how filtering one visual affects the other visuals  |
    | **Notes**             | Adds notes to the canvas                                    |
    | **Settings**          | Opens the sheet settings                                    |

1. Select **Canvas** to open the **Canvas** pane, review the **General**, **Layers**, and **Themes** tabs, and then close the pane.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image19.png)


### Task 3: Review the filter, bookmark, and parameter panes

1. In the sidebar on the right, select the **Filter** icon.

    The **Filter** pane has **Page level** filters, which apply to the visuals on this sheet, and **Global level** filters, which apply across the app.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image20.png)

    >[!Tip] For example, you can drag **Year** from **\[Dim\] Date** to **Global level** and select **2025** and **2026** to filter the whole dashboard. This lab doesn't require this filter.

1. Select the **Bookmark** icon.

    Bookmarks save the current analytical view, including the filter state and scroll position. On the **Public** tab, you share views with everyone; on the **Private** tab, you save your own views. In Reading view, users can save private bookmarks.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image21.png)

1. Select the **Parameter** icon (**{x}**).

    Parameters store reusable values, such as a target margin percentage, or drive conditional display logic based on a user selection. Parameters can be defined at **Page** or **Global** level.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image22.png)


### Task 4: Review the visualization types

1. In the **Visualizations** pane, review the available visual types:

    | **Visual**       | **Use**                                                                                                |
    |------------------|--------------------------------------------------------------------------------------------------------|
    | **Charts 100+**  | Library of 100+ charts, organized by chart family (Bar/Column, Line, Area, Waterfall, Combo, and more) |
    | **Planning**     | Embeds a planning sheet on the canvas                                                                  |
    | **PowerTable**   | Embeds a PowerTable sheet on the canvas                                                                |
    | **Matrix**       | Shows hierarchical row and column data                                                                 |
    | **Table**        | Shows flat tabular data                                                                                |
    | **Gantt**        | Shows Gantt and resource Gantt charts                                                                  |
    | **Super Filter** | Adds interactive filters such as date ranges                                                           |
    | **KPIs**         | Shows key measures as cards, with trellis support                                                      |


## Exercise 3: Build the dashboard visuals

In this exercise, you build the four main visuals of the dashboard: a variance chart, a grouped line chart, a KPI card, and a matrix.

### Task 1: Build an integrated variance bar chart

In this task, you compare **Actuals** with **Forecast** and **Plan** by region and country.

1. In the **Visualizations** pane, select **Charts 100+**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image23.png)

1. In the search box, enter `variance`. Under **Bar/Column**, select **Integrated Variance Bar Chart**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image24.png)

1. In the **Data** pane, expand **DimGeography**, and select **RegionName** and **CountryName**. Both fields appear under **Category**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image25.png)

1. Expand **Measure \> Financial \> Base Measures**, and map the measures as follows:

    | **Field well**            | **Measure**  |
    |---------------------------|--------------|
    | Actual(s)                 | **Actuals**  |
    | Comparison 1 (vs Actuals) | **Forecast** |
    | Comparison 2 (vs Actuals) | **Plan**     |

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image26.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image27.png)

    The chart shows Actuals by region, with the variance against Forecast and Plan at the end of each bar.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image28.png)

1. Use the drill controls in the visual header to drill down from **RegionName** to **CountryName**, and select **Expand all** to show both levels at the same time.

1. Select a data label, and in the on-object toolbar, set the scaling to `Thousands`.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image29.png)

1. Select a bar, open the series color picker, enter `#3f7aab`, and then select **Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image30.png)


### Task 2: Build a grouped line chart

In this task, you compare **Actuals** and **Forecast** month by month.

1. Select an empty area of the canvas. In the **Visualizations** pane, select **Charts 100+**, expand **Line**, and then select the **Grouped line** chart.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image31.png)

1. Map the following fields:

    | **Field well** | **Field**                                             |
    |----------------|-------------------------------------------------------|
    | Category       | **\[Dim\] Date \> Month_Name**                        |
    | Actual(s)      | **Measure \> Financial \> Base Measures \> Actuals**  |
    | Forecast       | **Measure \> Financial \> Base Measures \> Forecast** |

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image32.png)

    The chart shows how Actuals and Forecast change over the months.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image33.png)


### Task 3: Build a KPI card with a product trellis

In this task, you compare **Actuals** with **Plan** for each product.

1. Select an empty area of the canvas, and in the **Visualizations** pane, select **KPIs**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image34.png)

1. Map the following fields:

    | **Field well**            | **Field**                     |
    |---------------------------|-------------------------------|
    | Actual(s)                 | **Actuals**                   |
    | Comparison 2 (vs Actuals) | **Plan**                      |
    | Trellis Row               | **DimProduct \> ProductName** |

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image35.png)

    The KPI visual tiles into one card per product, with the variance to plan and an up or down arrow, for an at-a-glance comparison across the product portfolio.


### Task 4: Build a matrix with variance columns

In this task, you compare **Actuals** with **Plan** by product category and subcategory over time.

1. Select an empty area of the canvas, and in the **Visualizations** pane, select **Matrix**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image36.png)

1. Map the following fields:

    | **Field well**               | **Field**                                                           |
    |------------------------------|---------------------------------------------------------------------|
    | Rows                         | **DimProduct \> ProductCategory**, **ProductSubCategory**           |
    | Columns                      | **\[Dim\] Date \> Date** hierarchy with **Year** and **Month** only |
    | Values (AC)                  | **Actuals**                                                         |
    | Compare to Prior Period (PY) | **Plan**                                                            |

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image37.png)

    >[!Note] In the **Date** hierarchy, clear **Quarter** and **Day** so that only **Year** and **Month** remain.

1. On the **Matrix** tab, select **Show Columns**, and then select **Actuals - Plan** and **(Actuals - Plan)%**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image38.png)

    The matrix adds the absolute and percentage variance columns automatically.


## Exercise 4: Add interactivity and collaboration

In this exercise, you connect all visuals to a date range, add a header, and collaborate with comments.

### Task 1: Add a date filter

1. Select an empty area of the canvas, and in the **Visualizations** pane, select **Super Filter**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image39.png)

1. In the **Data** pane, expand **\[Dim\] Date \> Date**, and add **Date** to **Category**. On the Super Filter, select the **Edit** (pencil) icon.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image40.png)

1. In the **Properties** pane, set **Filter by type** to **Date**. On the **General** tab, under **Global**, set **Type** to **Calendar**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image41.png)

1. In the Super Filter, select a **Start Date** and an **End Date**.

    The variance chart, grouped line chart, KPI card, and matrix update to show the selected period.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image42.png)

    >[!Tip] Use **Edit Interactions** on the **Intelligence** tab if you want a visual to ignore the Super Filter.


### Task 2: Add a dashboard header

1. On the **Intelligence** tab, select **Elements \> Text**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image43.png)

1. In the text box, enter `SaaS Enterprise - Financial Performance Dashboard`.

1. Select the text, and use the formatting toolbar to set the font size to **18** and apply **Bold**. Then drag the text box to the top of the canvas.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image44.png)


### Task 3: Add a comment

1. In the matrix, select a cell, for example a **Plan** value.

1. In the header, select **Comments \> Add new comment**.

1. Enter a comment, type `@` followed by a colleague's name to tag them, and then press **Enter** to post the comment.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image45.png)

    >[!Note] Comments in intelligence sheets are context-aware: they respect the current filter state and can be added at the cell, row, column, or report level. If **Add new comment** is unavailable, select **Save** first.


### Task 4: Review the dashboard in Reading view

1. Select **Save**. Then, in the header, open the **Editing** menu and select **Reading**.

1. Review the finished dashboard as your business users see it.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2008/media/image46.png)


### Outcomes

You now have an interactive enterprise dashboard that:
    - Connects an intelligence sheet in the **Financial-Plan** app to the **Enterprise Dataset** semantic model
    - Uses the **Intelligence** tab and the filter, bookmark, and parameter panes to configure the canvas
    - Includes an integrated variance bar chart with drill-down, scaling, and custom colors
    - Includes a grouped line chart, a KPI card with a product trellis, and a matrix with variance columns
    - Uses a Super Filter to update all visuals for a date range
    - Includes a dashboard header and comments with @mentions


## Summary

In this lab, you imported the Enterprise Dataset semantic model, created the **Financial-Plan** app, and connected it to the model. You created an intelligence sheet and explored its canvas controls, filter, bookmark, and parameter panes, and visualization types. You then built a financial performance dashboard with an integrated variance bar chart (Actuals vs. Forecast and Plan by region and country), a grouped line chart (Actuals vs. Forecast by month), a KPI card trellised by product, and a matrix with variance columns by product category. Finally, you connected all visuals to a date range with a Super Filter, added a header, collaborated with comments, and reviewed the dashboard in Reading view.
