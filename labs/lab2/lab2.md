# Power BI Project (PBIP) and AI

TODO

## 🛠️ Prerequisites

* Enable the following **Power BI Desktop** preview features:
  * Power BI Project (.pbip) save option
  * Store semantic model using TMDL format
  * Store reports using enhanced metadata format (PBIR)
* Ensure you have the following Visual Studio Code extensions
  * [TMDL extension](https://marketplace.visualstudio.com/items?itemName=analysis-services.TMDL)
  * [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot)
  * [GitHub Copilot chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat)

## 1. Explore TMDL view

✅ **Goal**: Learn how to use **TMDL view**

### Steps

1. Open [Sales.pbix](../lab1/resources/Sales.pbix) in **Power BI Desktop**.
2. Go to **File > Save As**, 
3. Choose a folder (e.g. `c:\temp\lab2`) and select **Save as type**: `Power BI Project Files (*.pbip)`
4. Name it: `Sales.pbip`
5. Open **TMDL view** tab
   ![tmdlview](resources/img/tmdlview-tab.png)
6. Script the expression `Environment` which is a Power BI parameter by dragging it from the model explorer to the code editor.
    ![tmdlview-dragexpression](resources/img/tmdlview-dragexpression.png)
7. Change the current value of the expression from "DEV" to "PRD"
   
   The script should look like this:

   ```tmdl
    createOrReplace

        expression Environment =
                "PRD" meta [
                    IsParameterQuery = true,
                    List = {"DEV", "QUAL", "PRD"},
                    DefaultValue = "DEV",
                    Type = "Text",
                    IsParameterQueryRequired = true
                ]
            lineageTag: 64edd943-1a90-4438-b62f-bb95a9da1510

            annotation PBI_ResultType = Text
   ```
8. Click **Preview** to display a code diff of the impact to the semantic model before executing the script.  
9. Click **Apply** to apply the change to the semantic model 
> [!TIP]
> **TMDL view**, unlike PBIP, which follows a code-behind mental model, is based on a **scripting mental model**. In TMDL view, you execute TMDL scripts using the `createOrReplace` command to define or update one or more semantic model objects. This means that scripts created in TMDL view are not automatically updated when you make changes in Model view. This separation is useful - TMDL view can serve as a kind of backup mechanism: before applying a batch of changes to your semantic model, you can save a script version, and easily revert by reapplying that earlier script if needed.

10. Notice that you modified a Power Query expression (the expression parameter), Power BI Desktop did not forced a data refresh. This behavior can be very useful when you want to update model queries without triggering a local refresh immediately.
11. Copy the TMDL script from the [Time intelligence calculation group](https://community.fabric.microsoft.com/t5/TMDL-Gallery/Time-intelligence-calculation-group/td-p/4770878) **TMDL Gallery** entry and paste it on a new **TMDL view** tab.
12. Execute the script and notice that a new calculation group `Time Intelligence` got created.
> [!TIP]
> **TMDL view** can be very useful to easily share semantic model objects between developers. Either from public galleries but also from internal locations such as BI team SharePoint site.
13.   Save your PBIP, open the PBIP with **Visual Studio Code** and notice that all the TMDL scripts you create get saved in the `Sales.SemanticModel/TMDLScripts` folder.

Learn more about **TMDL view** in [documentation](https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-tmdl-view).

## 2. Apply changes with TMDL view with 

✅ **Goal**: Learn how to use **TMDL view**

### Steps

1. Open [Sales.pbix](../lab1/resources/Sales.pbix) in **Power BI Desktop**.