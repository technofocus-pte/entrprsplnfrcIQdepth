# Lab 09: Build and export a P&L statement with Intelligence Sheet

## Introduction

Finance teams often rebuild the profit and loss (P&L) statement in spreadsheets every month: they copy numbers from BI reports, add ratios such as gross margin and earnings per share by hand, hide sensitive lines before they share the file, and track approvals in email.

In this lab, you build the whole P&L statement in an **intelligence sheet** in Plan (preview) for Microsoft Fabric, directly on top of the **Enterprise Dataset** semantic model. You add a **Matrix** visual with the **Financial** template, extend the statement with calculated and data input rows that aren't in the semantic model, organize and format the report, add **Approval Status** and **Stakeholder** columns, add a branded header, and export the result to Excel and PDF.

## Scenario

You are a BI analyst at a SaaS company. Your finance leadership team needs a monthly P&L statement that:
- Compares **Actuals** with **Plan** for every account in the account hierarchy
- Shows **Gross Margin %** and **Earnings Per Share**, which aren't stored in the semantic model
- Keeps sensitive payroll lines, such as engineering salaries, out of the shared view
- Records who owns the report and whether it's approved
- Can be shared as a branded Excel workbook and PDF


### Objectives

After completing this lab, you will be able to:
- Create an intelligence sheet with a Matrix visual and apply the **Financial** template
- Add calculated rows (**Gross Margin %**, **Earnings Per Share**) and a data input row (**Shares Outstanding**)
- Group, hide, reorder, and format rows and columns
- Add **Approval Status** and **Stakeholder** columns for governance
- Add a report header with a title and logo
- Export the P&L statement to Excel and PDF


### Architecture

The **Financial-Plan** Plan app from Lab 08 connects to the **Enterprise Dataset** semantic model in your Fabric workspace. The **P&L Statement** intelligence sheet uses a Matrix visual to read the **Account Hierarchy** and the **Actuals** and **Plan** measures, and adds calculated rows, input rows, groups, formatting, and governance columns on top. The finished statement is exported to Excel and PDF for finance stakeholders.

![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image1.png)

| **Component**                           | **Role in the lab**                                                                                             |
|-----------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| Enterprise Dataset semantic model       | Source of the **Account Hierarchy**, **Date**, and the **Actuals** and **Plan** measures                        |
| Financial-Plan (Plan app)               | Hosts the intelligence sheet and connects to the semantic model                                                 |
| P&L Statement (intelligence sheet)      | Matrix visual with the Financial template                                                                       |
| Calculated and data input rows          | **Gross Margin %**, **Earnings Per Share**, and **Shares Outstanding** - rows that aren't in the semantic model |
| Groups and formatting                   | Hide the **Confidential** rows, group the **Metrics** columns, and apply colors, gridlines, and a header        |
| Approval Status and Stakeholder columns | Track the approval state and owner of each row                                                                  |
| Export                                  | Excel (with expand/collapse) and PDF                                                                            |

### Prerequisites

- Completion of Lab 08 - the **Enterprise Dataset** semantic model is imported into your workspace and the **Financial-Plan** app exists
- Permission to create and edit intelligence sheets
- A logo image for the report header, for example **Contoso.png** in **C:\LabFiles**


>[!Note] If you didn't complete Lab 08, follow Lab 08, Exercise 1, to import the **Enterprise Dataset** semantic model and create the Plan app first.

## Exercise 1: Create the P&L intelligence sheet

In this exercise, you create an intelligence sheet, add a Matrix visual, map the financial measures and account hierarchy, and apply the Financial template.

### Task 1: Create the intelligence sheet

1. In your workspace, open the **Financial-Plan** app. On the **Home** tab, select **New Intelligence Sheet**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image2.png)

    >[!Note] If you're working in a new Plan app that isn't connected to a semantic model yet, complete steps 2 and 3 first. If your app is already connected to **Enterprise Dataset** (from Lab 08), skip to step 4.

1. Under **Get data**, select **Semantic Model**, select the **Semantic Model** box, and then in the **OneLake catalog**, select **Enterprise Dataset** and select **Add**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image3.png)

    >[!Note] This screenshot is from a different environment. Select the **Enterprise Dataset** item in your own workspace.

1. Select **New Intelligence Sheet**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image4.png)

1. In the **New Intelligence Sheet** dialog box, enter `P&L Statement` as the **Name**, and then select **Create**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image5.png)


### Task 2: Add the Matrix visual and map the data

1. In the **Visualizations** pane, select **Matrix**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image6.png)

1. In the **Data** pane, map the following fields:

    | **Field well**       | **Field**                                                                    |
    |----------------------|------------------------------------------------------------------------------|
    | Rows                 | **\[Dim\] Account \> Account Hierarchy** (Account Level 1, Level 2, Level 3) |
    | Values (AC)          | **Measure \> Financial \> Base Measures \> Actuals**                         |
    | Compare to Plan (PL) | **Measure \> Financial \> Base Measures \> Plan**                            |

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image7.png)

    >[!Note] Leave **Columns** empty. The **Metrics** column group that you create later groups the **Plan** and **Actuals** columns.

1. On the **Matrix** tab, select **Templates**, and then under **Financial**, select the **Simple** template.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image8.png)

    The Financial template applies indentation, subtotals, and bold formatting to parent rows, such as **Revenue**, **Cost of Revenue / COGS**, and **Operating Expenses**.


## Exercise 2: Add calculated and input rows

In this exercise, you add rows that aren't part of the semantic model: **Gross Margin %**, **Shares Outstanding**, and **Earnings Per Share**.

### Task 1: Insert the Gross Margin % calculated row

1. On the **Data** tab, select **Insert Row \> Formula**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image9.png)

1. In the **Calculated Row** pane, enter the following details, and then select **Create**:

    | **Setting**    | **Value**                                                      |
    |----------------|----------------------------------------------------------------|
    | Title          | `Gross Margin %`                                           |
    | Row Type       | **Formula**                                                    |
    | Formula        | `([Revenue] - [Cost of Revenue / COGS]) / [Revenue]` |
    | Scaling Factor | **None**                                                       |

    >[!Tip] With **Suggestions** turned on, type `[` in the **Formula** box and select the row names from the list so that they match exactly.

1. Select the **Gross Margin %** row. On the **Matrix** tab, select the **%** button to format the row as a percentage.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image10.png)

    The row shows the gross margin, for example **69.57%** for **Plan**.


### Task 2: Insert the Shares Outstanding data input row

1. Select the row menu (gripper) next to **Revenue**, select **Insert**, and then select **Data Input**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image11.png)

1. In the **Static Row** pane, enter `Shares Outstanding` as the **Title**, keep the other settings, and then select **Create**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image12.png)

1. In the **Shares Outstanding** row, enter `100m` in the **Actuals** cell and `150m` in the **Plan** cell. Press **Enter** after each value.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image13.png)


### Task 3: Insert the Earnings Per Share calculated row

1. On the **Data** tab, select **Insert Row \> Formula**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image14.png)

1. In the **Calculated Row** pane, enter `Earnings Per Share` as the **Title**.

1. In the **Formula** box, enter `[Revenue] / [Shares Outstanding]`, and then select **Create**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image15.png)

    >[!Note] When you select the new input row from the suggestions, it might appear as **\[Shares Outstanding(New Row)\]**, and the screenshots show the row name as **Earnings Per ShareNew Row**. Both work the same way.

1. Select the **Earnings Per Share** row. On the **Matrix** tab, select the **\$€** button.

1. In the **Prefix / Suffix** dialog box, enter `$` in **Value prefix**, and then select **Apply**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image16.png)

    The row shows the earnings per share with a dollar sign, for example **\$ 35.29** for **Plan**.


## Exercise 3: Format and organize the report

In this exercise, you group and hide confidential rows, group the metric columns, reorder and highlight rows, and add gridlines.

### Task 1: Group and hide confidential rows

1. Under **Product & R&D Opex**, select **Engineering Salaries**, and then press **Ctrl** and select **Product Management**.

1. On the **Matrix** tab, select **Layout \> Group \> Create Group**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image17.png)

1. In the **Create Group** dialog box, enter `Confidential` as the **Group Label**, and then select **Apply**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image18.png)

1. Select the row menu next to the **Confidential** group, select **Actions**, and then select **Hide icon & children**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image19.png)

    The **Confidential** row shows the group total, but the individual salary rows are hidden.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image20.png)

1. Select the **Confidential** row. On the **Format** tab, select **Font Color**, enter `#9467BD`, and then select **Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image21.png)


### Task 2: Group the metric columns

1. Select the **Plan** column header, and then press **Ctrl** and select the **Actuals** column header.

1. On the **Matrix** tab, select **Layout \> Group \> Create Group**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image22.png)

1. In the **Create Group** dialog box, enter `Metrics` as the **Group Label**, and then select **Apply**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image23.png)

    A **Metrics** header appears above the **Plan** and **Actuals** columns.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image24.png)


### Task 3: Reorder and highlight rows

1. Select **Shares Outstanding**, **Earnings Per Share**, and **Gross Margin %** (press **Ctrl** to select multiple rows), and drag them above **Revenue**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image25.png)

1. Verify that the three rows now appear at the top of the statement, above **Revenue**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image26.png)

1. Select the **Earnings Per Share** row. On the **Format** tab, select **Fill Color**, enter `#9467BD`, and then select **Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image27.png)

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image28.png)


### Task 4: Add gridlines and theme colors

1. On the **Format** tab, select **Major Gridlines**, and then select **Single**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image29.png)

1. Optionally, use **Theme Colors** on the **Format** tab to apply your organization's report colors.


## Exercise 4: Add governance columns, a header, and export

In this exercise, you add **Approval Status** and **Stakeholder** columns, add a report header with a logo, and export the P&L statement.

### Task 1: Add the Approval Status column

1. On the **Data** tab, select **List \> Single Select**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image30.png)

1. In the **Data Input** pane, enter `Approval Status` as the **Title**.

1. Under **Options**, select **Presets**. In the **Add options from Preset** dialog box, select **Approval Status**, and then select **Apply**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image31.png)

    The options **Submitted**, **In Review**, and **Approved** are added, each with its own color.

1. Select **Create**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image32.png)


### Task 2: Add the Stakeholder column

1. On the **Data** tab, select **Person**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image33.png)

1. In the **Data Input** pane, enter `Stakeholder` as the **Title**, and confirm that **Input type** is **Person**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image34.png)

1. Select **Create**.

1. In a **Stakeholder** cell, type `@` followed by a user's name, and select the user from the organization directory to assign ownership.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image35.png)


### Task 3: Add a report header and logo

1. On the **Format** tab, select **Header & Footer**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image36.png)

1. On the **Header & Footer** tab, select **Presets**, and then select **Header**. Select the header layout with a title on the left and a logo on the right.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image37.png)

1. Select the title container and change the title to **Profit and Loss Statement**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image38.png)

1. Select the **Your Logo** container, and then select **Replace Image**.

1. In the **Upload Images** dialog box, select **Upload File**, select your logo file (for example, **C:\LabFiles\Contoso.png**), and then select **Save**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image39.png)

    The header shows the title and the logo above the matrix.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image40.png)

    >[!Note] Use a logo that your organization allows you to use.


### Task 4: Export the report to Excel and PDF

1. On the **Matrix** tab, select **Export**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image41.png)

1. In the **Export** dialog box, select **Excel**.

1. Under **Properties**, select **With Expand/Collapse** to keep the hierarchy expandable in Excel, select **Entire Matrix**, and then select **Export**.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image42.png)

1. When the download is ready, open or save the Excel file from your browser's **Downloads** notification.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image43.png)

1. Select **Export** again, select **PDF**, select **Entire Matrix**, and then select **Export**. Open or save the PDF file from the **Downloads** notification.

    ![](https://raw.githubusercontent.com/technofocus-pte/entrprsplnfrcIQdepth/refs/heads/main/Cloudslice/Labguides/Lab%2009/media/image44.png)


### Outcomes

You now have a P&L intelligence sheet that:
    - Uses the **Enterprise Dataset** semantic model with a Matrix visual and the **Financial** template
    - Includes the **Gross Margin %** and **Earnings Per Share** calculated rows and the **Shares Outstanding** data input row
    - Hides the **Confidential** payroll rows, groups the **Metrics** columns, and uses reordered, highlighted rows and gridlines
    - Includes **Approval Status** and **Stakeholder** columns
    - Includes a **Profit and Loss Statement** header with a logo
    - Is exported to Excel and PDF


## Summary

In this lab, you built a complete profit and loss statement on an intelligence sheet in Plan for Microsoft Fabric. You added a Matrix visual on the Enterprise Dataset semantic model and applied the Financial template. You extended the statement with the **Gross Margin %** and **Earnings Per Share** calculated rows and the **Shares Outstanding** data input row, which don't exist in the semantic model. You then grouped and hid confidential rows, grouped the metric columns, reordered and highlighted rows, and added gridlines. Finally, you added **Approval Status** and **Stakeholder** columns for governance, a branded header, and exported the finished statement to Excel and PDF.
