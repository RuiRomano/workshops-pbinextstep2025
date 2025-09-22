# Power BI Project (PBIP) and AI

This lab shows how AI can assist in your Power BI development - whether it's using AI chats to modify a TMDL script and apply it back to the semantic model, or leveraging AI agents to understand and update your Power BI Project files.

## 🛠️ Prerequisites

* Enable the following **Power BI Desktop** preview features:
  * Power BI Project (.pbip) save option
  * Store semantic model using TMDL format
  * Store reports using enhanced metadata format (PBIR)
* Ensure you have the following Visual Studio Code extensions
  * [TMDL extension](https://marketplace.visualstudio.com/items?itemName=analysis-services.TMDL)
  * [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot)
  * [GitHub Copilot chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat)
* GitHub Copilot subscription, sign up for a trial [here](https://github.com/github-copilot/pro).

## 1. Explore TMDL view

✅ **Goal**: Get confortable with **TMDL view**.

### Steps

1. Open [lab2/resources/Sales.pbix](/resources/Sales.pbix) in **Power BI Desktop**. (**Note:** it's a different file from **Lab1**.)
> [!IMPORTANT]
> * If you downloaded the repo code as a ZIP file, the PBIX file won't be included because it's managed with Git LFS. To get the PBIX, please download it directly from the repository and replace the local file. [Download Lab 2 Sales.pbix](https://github.com/RuiRomano/workshops-pbinextstep2025/raw/refs/heads/main/labs/lab2/resources/Sales.pbix?download=)
2. Go to **File > Save As**, 
3. Choose a folder (e.g. `c:\temp\lab2`) and select **Save as type**: `Power BI Project Files (*.pbip)`
4. Name it: `Sales.pbip`
5. Open **TMDL view** tab
    
    ![tmdlview](resources/img/tmdlview-tab.png)

6. Script the expression `Environment` - which is a semantic model parameter - by dragging it from the model explorer into the code editor.
    
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
8. Click **Preview** to display a code diff of the impact to the semantic model before executing the script. **Preview** is very helpful to let you better understand the impact of the script to the model before execution.
9.  Click **Apply** to apply the change to the semantic model 
> [!TIP]
> **TMDL view** follows a **scripting mental model**. In TMDL view, you execute TMDL scripts using the `createOrReplace` command to define or update one or more semantic model objects. This means that scripts created in TMDL view are not automatically updated when you make changes in others Power BI Desktop views. 
1.  Notice that you modified a Power Query expression (the expression parameter), Power BI Desktop did not forced a data refresh. This behavior is by design and can be very useful when you want to update model queries without being forced to refresh your model. 
2.  Create a new **TMDL view** tab.
3.  Open the [Time intelligence calculation group](https://community.fabric.microsoft.com/t5/TMDL-Gallery/Time-intelligence-calculation-group/td-p/4770878) **TMDL Gallery** entry, copy the code and paste it on the new tab.
4.  Execute the script and notice that a new calculation group `Time Intelligence` got created.
5.  Save and open the PBIP with **Visual Studio Code**, notice that all the TMDL scripts you created are saved in the `Sales.SemanticModel/TMDLScripts` folder.

> [!TIP]
> * **TMDL view** can be very useful to ease collaboration and sharing of semantic model objects between developers and community. Either from public galleries such as [TMDL gallery](https://community.fabric.microsoft.com/t5/TMDL-Gallery/) or private locations such as BI team SharePoint site.

## 2. Batch changes with TMDL view + GitHub Copilot

✅ **Goal**: Learn how to use TMDL view together with generative AI for batch edits for model documentation and measure generation.

### Steps

1. Open `Sales.pbip` in **Power BI Desktop**.
2. Open **TMDL view**, drag the table `Sales` to the code editor and rename the script tab to `Sales_Script`.
3. Save your PBIP.
4. Open the PBIP folder with **Visual Studio Code**.
5. Navigate to the TMDL script file `Sales.SemanticModel/TMDLScripts/Sales_Script.tmdl`.
6. Open [**GitHub Copilot chat**](https://docs.github.com/en/copilot/how-tos/use-chat/use-chat-in-ide) by clicking the Copilot icon next to the search bar or pressing `CTRL+SHIFT+I`
    ![githubcopilot-open](resources/img/githubcopilot-open.png)
7. Ensure the mode is set to **Edit** to ensure that Copilot only edits the opened file.
   ![githubcopilot-editmode](resources/img/githubcopilot-editmode.png)
8. Type `set descriptions on all columns and measures` and execute.
   ![copilot-description-chat](resources/img/copilot-description-chat.png)   
9.  **GitHub Copilot** may produce inaccurate or entirely incorrect TMDL scripts - such as the example below. This behavior is expected, as current Large Language Models (LLMs) do not yet fully understand the semantics of TMDL and its scripting language. 
     
    ![copilot-description-badoutput](resources/img/copilot-description-badoutput.png)    

10. Click **Undo** to discard **GitHub Copilot** changes and go back to the original version of the script.
11. Copy the file [`resources/copilot-instructions.md`](resources/copilot-instructions.md) into the `.github/` directory within your PBIP folder.

    ```text
    Lab2/
    ├── .github/
    |   └── copilot-instructions.md
    ├── Sales.Report/
    ├── Sales.SemanticModel/        
    └── Sales.pbip
    ```    
> [!IMPORTANT]
> While you can improve **GitHub Copilot** output by making your prompts more specific, it requires you to remember to do so every single time. A more reliable approach is to include a custom instructions file in your project, which gives **GitHub Copilot** additional context on how to work with PBIP files and TMDL language.

12. Repeat **step #8** and notice the difference now with context file. 

    **Copilot** should tell you that is using `copilot-instructions.md` file as reference.

    ![copilot-instructions-reference](resources/img/copilot-instructions-reference.png)

    And this time it should produce valid TMDL, because it's following the guidelines in `copilot-instructions.md`. Notice how its using references to COMPANY details in the description.

    ![copilot-description-goodoutput](resources/img/copilot-description-goodoutput.png)

> [!TIP]
> Take a moment to review the `copilot-instructions.md` file and notice that we don't need to be very specific and its similar as explaining concepts to a colleague. 

13. Copy the TMDL script from **Visual Studio Code** and paste it back in **TMDL view** as a new tab.
14. Click **Apply** in TMDL view to run the script
15. Confirm that all columns and measures from the `Sales` table now have descriptions.

    ![copilot-description-test](resources/img/copilot-description-test.png)
    
> [!IMPORTANT]
> **TMDL view** enables you to easily script objects from the semantic model into code that AI can edit/generate. Understanding this is essential to unblock your efficiency - because with the right context, you can achieve virtually any automation scenario. 

16. Open **TMDL view**, drag the table `Product` into the code editor and rename the script tab to `Product_Script`.
17. Save the PBIP.
18. Open `Sales.SemanticModel/TMDLScripts/Product_Script.tmdl` with **Visual Studio Code**
19. Open **GitHub Copilot chat**, type `create base measures for columns` and execute.
    
    Notice that **Copilot** will create the base measures and follow the guidelines in  [`copilot-instructions.md`](resources/copilot-instructions.md) for measure creation.

    ![copilot-measures-output](resources/img/copilot-measures-output.png)

## 3. PBIR + GitHub Copilot

✅ **Goal**: Learn how to apply a batch update to a Power BI Report using GitHub Copilot.

### Steps

1. Open [Sales.pbip] in **Power BI Desktop**.
2. Swap the [Power BI theme](https://learn.microsoft.com/en-us/power-bi/create-reports/desktop-report-themes) to one of the themes available in [lab1/resources/themes](../lab1/resources/themes/).
   
   Notice that the bottom visuals do not change the colors, because this report is not following best practices of using theme colors.

   ![copilot-pbir-reportheme](resources/img/copilot-pbir-reportheme.png)

3. Open the PBIP folder with **Visual Studio Code**.
4. Open [**GitHub Copilot chat**](https://docs.github.com/en/copilot/how-tos/use-chat/use-chat-in-ide) by clicking the Copilot icon next to the search bar or pressing `CTRL+SHIFT+I`
5. Ensure the mode is set to **Agent** to ensure that Copilot has visibility and can edit the opened PBIP folder
   ![copilot-agent-mode](resources/img/copilot-agent-mode.png)
> [!TIP]
> In [**Agent mode**](https://code.visualstudio.com/blogs/2025/02/24/introducing-copilot-agent-mode) **GitHub Copilot** can analyze all your code folder and apply edits to your files.
6. Type in the chat `Analyze the position property of ALL the visual.json files of the Power BI report in Sales.Report folder. And build a SVG wireframe with name Wireframe.svg and color the shapes with different colors by visualType.` and execute.
   
    **GitHub Copilot** will read all the visual.json position configuration and build a wireframe of the report as SVG:

    Example of `visual.json` position property:

    ```json
        {
            "$schema": "https://developer.microsoft.com/json-schemas/fabric/item/report/definition/visualContainer/2.1.0/schema.json",
            "name": "5acb1caf298449a8acb4",
            "position": {
                "x": 32.031666460447411,
                "y": 209.89399398443555,
                "z": 3000,
                "height": 272.23165252121817,
                "width": 428.15776335496753,
                "tabOrder": 2000
            }
            ...
        }
    ```
    ![copilot-pbir-wireframe](resources/img/copilot-pbir-wireframe.png)

> [!TIP]
> Generating report wireframes can be extremely helpful for documentation or diagnosing performance issues.

7. Now let’s use **GitHub Copilot** to fix our report by resetting all visuals to use theme colors instead of static hex codes.
8. Copy the file [`resources/instructions-pbir-theme-fix.md`](resources/instructions-pbir-theme-fix.md) into the PBIP folder.
9. Close all opened files in **Visual Studio Code**
10. In **GitHub Copilot** chat, click on **Attach Context** and select the file `instructions-pbir-theme-fix.md`. You can also drag-drop the file to the chat.
11. Type in the chat `Follow attached instructions and replace the static colors in Sales.Report for theme colors.` and execute.
    
    ![copilot-pbir-themecolors-prompt](resources/img/copilot-pbir-themecolors-prompt.png)

    **GitHub Copilot** should replace all the HEX colors for a theme color, and it may prompt you to review the mapping.

    ![copilot-pbir-themecolors-confirm](resources/img/copilot-pbir-themecolors-confirm.png)

    ![copilot-pbir-themecolors-diff](resources/img/copilot-pbir-themecolors-diff.png)

12. Close **Power BI Desktop**
13. Re-open the `Sales.pbip`.
14. Try to change the report theme and notice that nowcolors of all visuals change.
> [!TIP]
> This is a simple, educational example with a small report - and doing it manually would have been easier. But the same approach scales to 100+ reports with +10 pages each, where automation really pays off. 

## ✅ Wrap-up

You’ve now:

* Learned how to use easily and effectively use AI chats to support your semantic model development together with **TMDL view**.
* Learned how to use **GitHub Copilot Agent mode** to inspect and modify your Power BI project files.

## Useful links

* [TMDL view docs](https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-tmdl-view)
* [GitHub Copilot Overview](https://code.visualstudio.com/docs/copilot/overview)
* [GitHub Copilot Agent Mode](https://code.visualstudio.com/blogs/2025/02/24/introducing-copilot-agent-mode)
