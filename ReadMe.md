# A dummy Azure DevOps Workitems dataset for learning purposes.

Here are the issues that I faced with clumsy CSV preparaton. Everytime Azure DevOps points out some creepy errors without giving the reason against the rule which is responsible for failure.

## Issue:1 - My CSV has parent but Azure Devops removes them while validation {#issue1---my-csv-has-parent-but-azure-devops-removes-them-while-validation .Rkj-Heading-1-v2}

To successfully import parent-child relationships via CSV in Azure
DevOps, you need to structure your file correctly
using **indentation** of the Title column or by specifying
the **Parent field ID**. Azure DevOps validation may remove parents if
the file format is incorrect. 

**Method 1: Using Indentation (Recommended for creating new
hierarchies)**

Azure DevOps interprets hierarchy during import based on the indentation
of the Title column. 

1.  **Prepare your CSV file:**

    - The parent work item must appear *immediately* before its children
      in the file.

    <!-- -->

    - Do not include the ID field for the new work items you are
      creating (Azure DevOps assigns these).

    <!-- -->

    - Indent the child items by using a different column name for their
      titles, such as Title 2. The parent\'s title should be under the
      original Title column.

> **Example CSV structure:**
>
> csv
>
> Work Item Type,Title,Title 2,Assigned To
>
> User Story,My Parent Story,,user@example.com
>
> Task,,Child Task 1,user@example.com
>
> Task,,Child Task 2,user@example.com

- In the parent row, the Title 2 column is blank.

<!-- -->

- In the child rows, the Title column is blank, and the task name is
  in Title 2.

2.  **Import the file:**

    - In Azure DevOps, go to **Boards** \> **Queries**.

    <!-- -->

    - Click on **Import work items** and select your CSV file.

    <!-- -->

    - The import process should preview the work items in a hierarchical
      view.

    <!-- -->

    - Click **Save items** to finalize the import and create the links. 

**Method 2: Using the Parent Field ID (For linking
to *existing* parents)**

If the parent work items already exist in Azure DevOps, you can
reference their IDs directly. 

1.  **Get the Parent IDs:** Note the unique IDs of the existing parent
    work items.

2.  **Prepare your CSV file:**

    - Include a Parent column in your CSV file.

    <!-- -->

    - For each child work item row, enter the corresponding parent\'s ID
      in the Parent column.

> **Example CSV structure:**
>
> csv
>
> ID,Work Item Type,Title,Parent
>
> ,Task,Child Task 1,12345
>
> ,Task,Child Task 2,12345

- The ID column is left blank for new child tasks. The parent ID is in
  the Parent column.

3.  **Use the Excel Add-in:**

    - Native CSV import may not support direct mapping of a Parent ID
      column. For this method to work reliably, you often need
      the **Azure DevOps Office Integration 2019** add-in for Microsoft
      Excel.

    <!-- -->

    - Connect Excel to your project using the **Team** tab.

    <!-- -->

    - Paste the data into Excel and use the add-in\'s features to
      publish the changes, ensuring the Parent column maps correctly to
      the work item\'s parent attribute. 

If the validation continues to remove parent information, it may be due
to:

- **Incorrect field name:** Ensure the column header exactly matches the
  field name expected by your process template (e.g., Parent might not
  be the exact field name in all configurations; exporting a sample
  first helps verify this).

- **Process limitations:** Azure DevOps native CSV import has some
  limitations compared to the Excel add-in. The indentation method
  (Method 1) is generally more robust for direct CSV import. 

## Issue:2 - Step-by-Step Guide: Connecting Excel to Azure DevOps {#issue2---step-by-step-guide-connecting-excel-to-azure-devops .Rkj-Heading-1-v2}

[(25) 🔧 Step-by-Step Guide: Connecting Excel to Azure DevOps \|
LinkedIn](https://www.linkedin.com/pulse/step-by-step-guide-connecting-excel-azure-devops-austin-christian-uy8gf/)

**🔷 Azure DevOps Integration with Excel**

**🔧 Step-by-Step Guide: Connecting Excel to Azure DevOps**

**🔹 Step 1: Download and Install the Excel Add-In**

- Go to:
  [**[https://visualstudio.microsoft.com/downloads]{.underline}**](https://visualstudio.microsoft.com/downloads)

- Expand **\"Other Tools, Frameworks, and Redistributables\"**

## Issue:3 - Azure DevOps Open in Excel {#issue3---azure-devops-open-in-excel .Rkj-Heading-1-v2}

[Azure DevOps Open in Excel - Visual Studio
Marketplace](https://marketplace.visualstudio.com/items?itemName=blueprint.vsts-open-work-items-in-excel)

## Issue:4 - While importing csv work items, feature state should not have what value {#issue4---while-importing-csv-work-items-feature-state-should-not-have-what-value .Rkj-Heading-1-v2}

When importing new work items (including **Features**) into Azure DevOps
via CSV, the **State** field should **not** have any value specified in
the CSV file. All imported new work items are created in a default
\"New\" or initial state. 

Attempting to assign a specific state, such as \"Done\", \"In
Progress\", or any other non-initial state, will cause the import to
fail, as the new work items must conform to the field rules for the
initial state. 

**How to Manage States During Import**

If you need work items to be in a specific state other than the initial
default, you must use a two-step process: 

1.  **Exclude the State field** from your initial import CSV file. The
    items will be imported in their default initial state (e.g.,
    \"New\", \"Proposed\", or \"To Do\", depending on your process
    template).

2.  **Export the newly imported items** to a new CSV file (they will now
    have assigned IDs).

3.  **Update the State field** to the desired value in the exported CSV
    file.

4.  **Re-import the updated CSV** file. This time, because the items
    have IDs, the process updates the existing work items, allowing you
    to change their state. 

For more details on the process and specific guidelines, you can refer
to the official [[Microsoft Learn
documentation]{.underline}](https://learn.microsoft.com/en-us/azure/devops/boards/queries/import-work-items-from-csv?view=azure-devops). 

## Issue:5 - Import, update, and export bulk work items with CSV files in Azure Boards {#issue5---import-update-and-export-bulk-work-items-with-csv-files-in-azure-boards .Rkj-Heading-1-v2}

[Import, update, and export bulk work items with CSV files - Azure
Boards \| Microsoft
Learn](https://learn.microsoft.com/en-us/azure/devops/boards/queries/import-work-items-from-csv?view=azure-devops)

**Import new work items**

To import work items in bulk, your CSV file must include the **Work Item
Type** and **Title** fields. You can include more fields as needed.
Follow these guidelines to import a CSV file:

- **Exclude the ID field:** Don\'t include the **ID** field in your CSV
  file.

- **Remove project-specific fields:** If the CSV file was exported from
  a different project, remove fields specific to the source project,
  such as **Area Path** and **Tags**. For a list of default fields,
  see [[Work Item Field
  Index]{.underline}](https://learn.microsoft.com/en-us/azure/devops/boards/work-items/guidance/work-item-field?view=azure-devops).

- **Include the Test Steps field:** When importing test cases, include
  the **Test Steps** field. For more information, see [[Bulk Import or
  Export Test
  Cases]{.underline}](https://learn.microsoft.com/en-us/azure/devops/test/copy-clone-test-items?view=azure-devops).

- Don\'t include **Assigned To**, **Changed Date**, **Created By**,
  or **State** fields.

- **Validate required fields:**

  - Ensure the **Work Item Type** and **Title** fields are present in
    the CSV file.

  - Confirm that the **Work Item Type** corresponds to a valid type in
    the target project.

  - Verify that all fields in the CSV file match the fields for the work
    item types in the target project.

- **Handle invalid values:** If the imported CSV file contains work
  items with invalid values, you must edit and correct these work items
  after import before they can be saved.

** Note**

You can import up to 1,000 work items at a time. If you have more than
1,000 work items to import, break them into multiple files and import
them separately.

## Issue:6 - Rules to prepare CSV file for importing Work Item in Azure DevOps {#issue6---rules-to-prepare-csv-file-for-importing-work-item-in-azure-devops .Rkj-Heading-1-v2}

To import work items into Azure DevOps using a CSV file, adhere to
specific formatting rules regarding required fields, structure, and data
values. The most important rule is that every new work item must have
a **Work Item Type** and a **Title**. 

**Mandatory Requirements**

- **Required Fields**: The CSV file **must** include the column
  headers Work Item Type and Title.

- **Exclude the ID Field for New Items**: Do not include the ID column
  for new work items, as Azure DevOps will assign unique IDs
  automatically upon import. If you are updating existing work items,
  the ID field is required.

- **Field Name Matching**: Ensure all column headers in your CSV exactly
  match the field names used in your target Azure DevOps project. To be
  sure of the correct names, it is best practice to export a sample of
  existing work items first and use that file as a template.

- **Data Validation**: All field values must be valid for the work item
  type and project\'s process template (Agile, Scrum, CMMI). Invalid
  values will be highlighted as errors after import and must be
  corrected before saving. 

**Optional Fields and Specific Data Rules**

- **Include other fields as needed**: You can include any other optional
  fields relevant to your work items
  (e.g., Priority, Description, Assigned To, Area Path, Iteration Path).

- **Identity Fields (Assigned To, Created By)**: When importing identity
  fields, use the format \"Display Name \<email\>\" (e.g., \"Jamal
  Hartnett fabrikamfiber4@hotmail.com\").

- **State Field**: New work items are assigned a default State (usually
  \"New\") upon import. You cannot specify a different state in the
  initial import CSV.

- **HTML Fields**: For fields that support rich text
  (like Description or Acceptance Criteria), include the appropriate
  HTML tags within the data to preserve formatting.

- **Parent-Child Links**: To create a hierarchy (e.g., linking tasks to
  a user story), use multiple title columns (e.g., Title 1, Title 2) to
  represent the hierarchy level through indentation in the file.

- **Test Cases**: If importing test cases, you must specifically include
  the Test Steps field. 

**Limitations and Best Practices**

- **Import Limit**: You can import up to 1,000 work items at a time
  using the web interface\'s CSV import feature.

- **Unsupported Types**: Certain work item types, such as Test Plans and
  Test Suites, are not supported for CSV import via the Queries page.

- **Validation occurs on import**: The CSV import process does not
  validate field types and values beforehand; validation happens when
  you run the import in the web portal.

- **Use the Microsoft documentation**: For detailed and up-to-date
  guidance, refer directly to the [[Microsoft Learn documentation on
  importing work items from
  CSV]{.underline}](https://learn.microsoft.com/en-us/azure/devops/boards/queries/import-work-items-from-csv?view=azure-devops). 

## Issue:7 - Tell me all the rules to follow and all the do\'s and don\'ts while preparing CSV file to bulk import work items in Azure DevOps. {#issue7---tell-me-all-the-rules-to-follow-and-all-the-dos-and-donts-while-preparing-csv-file-to-bulk-import-work-items-in-azure-devops. .Rkj-Heading-1-v2}

Preparing a CSV for bulk importing work items into Azure DevOps requires
strict adherence to field naming and formatting rules to avoid \"Invalid
Workitem Field\" errors.

**Essential Rules & Formatting**

- **Mandatory Fields:** Every row must include **Work Item
  Type** and **Title**.

- **Encoding:** Save your file as **CSV UTF-8** (Comma delimited). Azure
  DevOps does not support semicolon-separated files natively.

- **Quotation Marks:** Wrap fields like *Title*, *Description*,
  and *Tags* in double quotes (\") to prevent commas within the text
  from being misinterpreted as column breaks.

- **Rich Text (HTML):** For fields like *Description* or *Acceptance
  Criteria*, use HTML tags (e.g., \<b\>, \<br\>) to preserve formatting.
  Plain text will lose line breaks during import.

- **Character Limits:** Titles are typically limited to 255 characters.

**Do\'s and Don\'ts**

| **Do\'s**                                                                                                                                 | **Don\'ts**                                                                                                                                                     |
|-------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Export a Sample First:** Create one work item manually, export it to CSV, and use that file as your template to get exact column names. | **Don\'t Include IDs for New Items:** Leave the ID column empty or exclude it entirely when creating new items. Assigning a dummy ID will cause an error.       |
| **Use Semicolons for Tags:** If a work item has multiple tags, separate them with a semicolon (e.g., \"Frontend; Beta; Sprint1\").        | **Don\'t Import \>1,000 Items:** The limit per import is 1,000 work items. Split larger datasets into multiple files.                                           |
| **Match Identity Names:** Ensure names in \"Assigned To\" match the exact Display Name or Email address in your Azure DevOps directory.   | **Don\'t Specify \"State\" for New Items:** Most processes default new items to \"New\" or \"To Do.\" Trying to import them directly as \"Closed\" often fails. |
| **Validate Area/Iteration Paths:** Ensure the paths exist in Project Settings. Use the full path: ProjectName\Area\SubArea.               | **Don\'t Forget Test Steps:** If importing Test Cases, you **must** include the *Test Step*, *Step Action*, and *Step Expected* columns.                        |

**Handling Parent-Child Relationships**

To create a hierarchy (e.g., a User Story with multiple Tasks) during a
single import:

1.  **Parent Row:** List the Parent item first.

2.  **Child Rows:** Place the child items immediately below the parent.

3.  **The \"Parent\" Column:** In the child rows, include a column
    named Parent and fill it with the ID of the parent work item (this
    only works if the parent already exists) **OR** use the \"Tree\"
    view method in Excel for new hierarchies.

**Pro-Tip for \"State\" Changes**

If you must import items in an \"Active\" or \"Resolved\" state:

1.  Import them in the default \"New\" state first.

2.  Once imported, use the **Bulk Edit** feature in the Queries results
    page to change the State for all items at once.

Would you like me to generate a **blank CSV template** with the correct
headers for a specific work item type like User Stories or Tasks?

This [[video on importing requirements from
Excel/CSV]{.underline}](https://www.youtube.com/watch?v=psNk0DT7lz8) provides
a visual walkthrough of the mapping process and how to handle common
data errors during the preview stage.

[Azure DevOps - Import Requirements from Excel and CSV (Map to
\...]{.underline}

[Dani Kahil · 18k views]{.underline}

## Issue:8 - Tell me all the rules to follow and all the do\'s and don\'ts while preparing CSV file to bulk import work items in Azure DevOps. {#issue8---tell-me-all-the-rules-to-follow-and-all-the-dos-and-donts-while-preparing-csv-file-to-bulk-import-work-items-in-azure-devops. .Rkj-Heading-1-v2}

**To successfully bulk import work items into Azure DevOps using CSV,
you must follow strict formatting rules and avoid common pitfalls. The
most critical points: always include a** Work Item Type **column, ensure
field names exactly match Azure DevOps field names, and avoid
unsupported fields like attachments or history. Misaligned headers or
invalid values will cause the import to fail.**

**✅ Do's: Best Practices for Preparing CSV Files**

**Required Columns**

- **Work Item Type**: Must be specified (e.g., User Story, Task, Bug).

- **Title**: Every work item requires a title.

- **State**: Optional but recommended (e.g., New, Active, Closed).

- **Area Path & Iteration Path**: Use valid paths from your project
  hierarchy.

- **Tags**: Comma-separated values are supported.

**Formatting Rules**

- **Exact Field Names**: Column headers must match Azure DevOps field
  names exactly (case-sensitive).

- **Unique Identifiers**: Use ID only when updating existing work items.
  Leave blank for new items.

- **Hierarchy Support**: Use Parent column with valid IDs to establish
  parent-child relationships.

- **Date Fields**: Must be in ISO format (yyyy-MM-ddTHH:mm:ssZ).

- **Effort/Story Points**: Numeric values only.

- **Boolean Fields**: Use True or False.

**Import Process**

- **Permissions**: You must be a member of the **Project
  Administrators** or **Team Foundation Administrators** group to
  import.

- **Validation**: Test with a small batch before importing large
  datasets.

- **Consistency**: Ensure all values (states, paths, tags) exist in your
  project configuration.

**❌ Don'ts: Common Mistakes to Avoid**

- **Don't use unsupported fields**: Attachments, links to external
  artifacts, or history notes cannot be imported via CSV.

- **Don't mismatch field names**: For example, Assigned To must be
  written exactly, not Assignee.

- **Don't include formulas or formatting**: Only raw values are allowed.

- **Don't use invalid paths**: Incorrect Area Path or Iteration Path
  will cause errors.

- **Don't duplicate IDs**: When updating, each ID must be unique and
  valid.

- **Don't exceed limits**: Large imports may fail; break into smaller
  files if necessary.

- **Don't forget encoding**: Save CSV in **UTF-8** to avoid character
  issues.

**📋 Quick Reference Table**

| **Rule Type** | **Do's**                                                        | **Don'ts**                                       |
|---------------|-----------------------------------------------------------------|--------------------------------------------------|
| **Columns**   | Include Work Item Type, Title, State, Area Path, Iteration Path | Omit required fields or use incorrect names      |
| **IDs**       | Leave blank for new items, use valid IDs for updates            | Duplicate or invalid IDs                         |
| **Hierarchy** | Use Parent column with valid IDs                                | Try to import attachments or history             |
| **Values**    | Use ISO dates, numeric effort, True/False for booleans          | Use free text for numeric/boolean fields         |
| **Paths**     | Ensure valid Area/Iteration paths                               | Use non-existent paths                           |
| **Encoding**  | Save as UTF-8                                                   | Use other encodings causing character corruption |

**⚠️ Risks & Tips**

- **Risk of data corruption**: Incorrect field mapping can overwrite
  existing work items. Always back up before bulk updates.

- **Tip**: Export a sample CSV from Azure DevOps first, then use it as a
  template for imports.

- **Tip**: Start with small imports to validate formatting before
  scaling up.

Would you like me to **create a sample CSV template** (with realistic
backlog items like User Stories, Tasks, and Bugs) that you can directly
use for your Azure DevOps practice? That way, you'll have a
ready-to-import file aligned with all these rules.
