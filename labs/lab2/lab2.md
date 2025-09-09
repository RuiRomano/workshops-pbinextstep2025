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
* GitHub Copilot subscription, sign up for a trial [here](https://github.com/github-copilot/pro).

## 1. Explore TMDL view

✅ **Goal**: Get confortable with **TMDL view**.

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
> * **TMDL view** can be very useful to easily share semantic model objects between developers. Either from public galleries but also from internal locations such as BI team SharePoint site. 
13.    Save your PBIP, open the PBIP with **Visual Studio Code** and notice that all the TMDL scripts you create get saved in the `Sales.SemanticModel/TMDLScripts` folder.

Learn more about **TMDL view** in [documentation](https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-tmdl-view).

## 2. Batch changes with TMDL view + GitHub Copilot

✅ **Goal**: Learn how to use TMDL view together with generative AI for batch edits for model documentation and measure generation.

### Steps

1. Open [Sales.pbip] in **Power BI Desktop**.
2. Using **TMDL view** script the table `Sales` and rename the script tab to `Sales_Script`
3. Save your PBIP
4. Open **Visual Studio Code** and the PBIP folder
5. Navigate to the TMDL script file `Sales.SemanticModel/TMDLScripts/Sales_Script.tmdl` 
6. Open [**GitHub Copilot chat**](https://docs.github.com/en/copilot/how-tos/use-chat/use-chat-in-ide) by clicking the Copilot icon next to the search bar or pressing `CTRL+SHIFT+I`
    ![githubcopilot-open](resources/img/githubcopilot-open.png)
7. Ensure the mode is set to **Edit** to ensure that Copilot only edits the opened file.
   ![githubcopilot-editmode](resources/img/githubcopilot-editmode.png)
> [!TIP]
> Learn about **Agent mode** where Copilot can analyze all your code folder and edit multiple files. Learn more about Agent mode in the [documentation](https://code.visualstudio.com/blogs/2025/02/24/introducing-copilot-agent-mode)
8. Type `set descriptions on all descriptions and measures` and execute.
   ![copilot-description-chat](resources/img/copilot-description-chat.png)   
9.  **GitHub Copilot** may produce inaccurate or entirely incorrect TMDL scripts - such as the example below. This behavior is expected, as current Large Language Models (LLMs) do not yet fully understand the semantics of TMDL and its scripting language.  
    ![copilot-description-badoutput](resources/img/copilot-description-badoutput.png)    
10. Undo or don't accept **Copilot** changes to go back to the original version of the script.
11. Copy the file [`resources/copilot-instructions.md`](resources/copilot-instructions.md) into the `.github` directory within your PBIP folder.

    ```text
    PBIP/
    ├── .github/
    |   └── copilot-instructions.md
    ├── Sales.Report/
    ├── Sales.SemanticModel/        
    └── Sales.pbip
    ```    
> [!IMPORTANT]
> While you can improve **Copilot** output by making your prompts more specific, it requires you to remember to do so every single time. A more reliable approach is to include a custom instructions file in your project, which gives Copilot additional context about PBIP and TMDL.

12. Repeat **step #8** and notice the difference now with context file. 

    **Copilot** should tell you that is using `copilot-instructions.md` file as reference.

    ![copilot-instructions-reference](resources/img/copilot-instructions-reference.png)

    And this time it should produce valid TMDL and following the guidelines in `copilot-instructions.md`. Notice how its using references to COMPANY details.

    ![copilot-description-goodoutput](resources/img/copilot-description-goodoutput.png)

> [!TIP]
> Take a moment to review the `copilot-instructions.md` file and notice that we don't need to be very specific and its similar as explaining concepts to a colleague. 

13. Copy the TMDL script from **Visual Studio Code** and paste it back in **TMDL view** as a new tab.
14. Click **Apply** in TMDL view to run the script
15. Confirm that all columns and measures from the `Sales` table now have descriptions.

    ![copilot-description-test](resources/img/copilot-description-test.png)
    
> [!IMPORTANT]
> **TMDL view** enables you to easily script objects from the semantic model into code that AI can manipulate. Understanding this is essential to unlocking your efficiency - because with the right context, you can achieve virtually anything. Moreover, the same principle applies directly to the PBIP TMDL files, allowing for seamless integration and automation at the file level.

16. Script `Product` table in **TMDL view** and rename the script tab to `Product_Script`
17. Open `Sales.SemanticModel/TMDLScripts/Product_Script.tmdl` with **Visual Studio Code**
18. Open **GitHub Copilot chat**, type `create base measures for columns` and execute.
    
    Notice that **Copilot** will create the measures respecting the guidelines of `copilot-instructions.md`

    ![copilot-measures-output](resources/img/copilot-measures-output.png)


  
