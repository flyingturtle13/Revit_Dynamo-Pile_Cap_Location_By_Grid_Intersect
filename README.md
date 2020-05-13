# Revit Dynamo - Pile Cap Grid Intersect Location & Offset
Revit does not have a grid intersect parameter for the structural foundation category. However, the structural column category has a parameter called Column Location Mark that indicates the associated nearest grid intersection. This script associates a structural column to the pile cap directly supporting it.  Also, the top of pile cap elevation from user selected reference level and sea level datum offsets are exported as each pile cap elevation vary due to grade elevation.  Ultimately, the pile cap type, grid line intersect, associated column, offsets, and pile cap coordinates from project base point get exported to the included Excel spreadsheet (Pile_Cap-Type&Location-Schedule.xlsx). This workflow and spreadsheet is beneficial during the design development phase when coordinating with the geotechnical engineer to design the pile cap foundation system.  As a bi-product, this script serves as a good QA/QC modeling tool making sure all pile caps are centered with associated column.  After the model data is exported, the structural engineer would need to add the loads at the base of the columns to the spreadsheet before distributing.    

## Getting Started
Environment setup regarding script development logistics.

* IDE:
  * Revit Dynamo

* Language:
  * Visual Programming / Python for Custom Node Creation

* Output File Type:
  * dyn

* Additional Library Packages Implemented: </br>
  * Clockwork

## Dynamo Script Development & Structure
#### Script features and specs.
* Software Required
  - Revit (any version with Dynamo 2.1 and above installed)
    
* User Interface
  - Run script through Dynamo add-in

* Setup Requirements and Capabilities
  - User to select target Excel file to export data through File Path node. 
  - Select reference Level to filter columns attached to associated pile caps and to calculate top of pile cap offset from level.
  - Per project, revise pile cap search String node based on project family type naming convention.
  - Per project, revise structural column search String node(s) based on project family type naming convention.
  
#### Dynamo Script Layout
Overall graph view
  <p align="center">
   <img src="https://user-images.githubusercontent.com/44215479/81774584-c55ae200-949f-11ea-86df-955061f08a28.png" width="1200">
  </p>

1. User Input
   * At the beginning of the project, select Excel file for exporting data and reference level that pile caps are associated to in the model.
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81775072-de17c780-94a0-11ea-8c33-974efff814cb.png" width="300">
   </p>

2. Structural Columns Backend Process & User Control to Filter Project Columns
   * This group of nodes takes all structural columns at specified reference level (pile cap associated level).
   * At the beginning of a project, user to update or add search String node(s) to get columns supported by a pile cap
     - Helps to eliminate duplicates (i.e. concrete encasement is modeled as a concrete column)
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81776124-13bdb000-94a3-11ea-951a-623908c2a1ab.png" width="800">
   </p>

3. Structural Backend Process that Gets Required Data to Be Exported
   * The data being retrieved and prepared for export are the x and y coordinates (feet from origin), Column Location Mark (grid intersect), and column family name and type
   * **The column coordinates is the key component used to match to associated pile cap**
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81777051-e6720180-94a4-11ea-9c7a-23e9efbc0ca2.png" width="600">
   </p>

4. Structural Foundation Backend Process & User Control to Filter Project Foundation Type
   * Monitors other available foundation parameters that can be exported if desired (user will need to setup output nodes if desired)
   * At the beginning of a project, user to update or add search String node(s) to get structural foundation type.  In this case, pile caps is part of the foundation system and uses the project naming convention to filter all pile caps.
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81777707-44ebaf80-94a6-11ea-8868-a296b6073257.png" width="600">
   </p>

5. Structural Foundation Backend Process to Get Pile Cap Coordinates for Column Mapping
   * **These are key nodes to get pile cap coordinates to use for matching to associated column**
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81778037-e7a42e00-94a6-11ea-9427-ff2aab895c34.png" width="600">
   </p>
   
6. Backend Process that Maps Column to Associated Pile Cap
   * **This is the process where column and associated pile cap are matched using their (x,y) coordinates**
   * The list index of the pile cap's matching coordinate values is stored as the value of the matching column coordinate list index in List.Map node.
   * If no match is found, List.Map records a value of '-1' for matching column coordinate list index.
   * '-1' is replaced by blank if no pile cap is matched.
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81778507-db6ca080-94a7-11ea-9c89-c585424343d5.png" width="600">
   </p>
   
7. Structural Foundation Backend Process Mapping Pile Cap Name & Preparing for Export
   * Reorganizes pile cap name to map to associated coordinates using List.Map result as template)
   * Replaces blank values with "NO MATCH" to be displayed to user
     - Helpful hint to user to check what is causing this
     - Possible that pile cap and column centerlines not aligned (requires model clean-up)
     - Possible that pile cap is supporing a wall
     - Possible that column is supported by a larger pile cap (similar to a mat foundation)
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81779459-9e091280-94a9-11ea-8d6c-29f37117a75e.png" width="600">
   </p>
   
8. Structural Foundation Backend Process that Gets Required Data for Offset Distance Exports
   * Exports "Offset" distance from Sea Level Datum and from Reference Level selected by user in (1.).
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81780412-58e5e000-94ab-11ea-82ae-8bc6c0f166d5.png" width="600">
   </p>

9. Backend Process that Consolidates All Export Data Per Each Pile Cap
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81780781-f3462380-94ab-11ea-88c0-abd275b63830.png" width="600">
   </p>
   
10. Output Process to Selected Excel Spreadsheet (Pile_Cap-Type&Location-Schedule.xlsx)
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81781140-897a4980-94ac-11ea-81fb-4400c6642253.png" width="600">
   </p>
  
#### Mapping Script to UI
1. UI associated to (Dynamo Script Layout 2.)
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81620527-f6082200-93a0-11ea-9574-f8088ccf2598.png" width="300">
   </p>

2. UI associated to (3.) resulting from user input from (Dynamo Script Layout 2.) and information from (Dynamo Script Layout 1.).
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81617573-8b53e800-939a-11ea-9cbd-500da098e4f4.png" width="300">
   </p>

3. Resulting Output after applying Revit structural elments to column layer and brace layer associated to (Dynamo Script Layout 5.).
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81617857-0cab7a80-939b-11ea-8f89-c6af9464e659.png" width="600">
   </p>
       
## Running Script & User Implementation Instructions
1. Clone or download project. </br>
2. In own Revit project Link CAD in example 3D wireframe model (3D_Wireframe-Example.dwg) in Model Examples folder or own wireframe model.
3. User to load desired structural families prior to running script.
4. Open Dynamo Player and select folder with 3DWireframe_To_RevitModel_Converter.dyn to display script (DYN).
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81618624-99a30380-939c-11ea-9b06-330fc01e681b.png" width="600">
   </p>
5. Follow user prompt and select linked CAD file to process
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81618864-2483fe00-939d-11ea-86ac-0ced8ec76c46.png" width="600">
   </p>
6. Follow next user prompt to auto model to 3D wireframe
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81619185-d8858900-939d-11ea-8d82-66e4c6b33ee8.png" width="600">
    <img src="https://user-images.githubusercontent.com/44215479/81619367-4631b500-939e-11ea-9639-4b78e9c18375.png" width="300">
   </p>
7. The following is the result of the input in (6.).
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81619502-9ad53000-939e-11ea-99f6-36ce9b2f8cd1.png" width="600">
   </p>
8. Reapeat steps (4.) - (6.) until all desired elements are modeled.
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81619897-6f9f1080-939f-11ea-9186-430a4e7f78ac.png" width="600">
    <img src="https://user-images.githubusercontent.com/44215479/81619926-7f1e5980-939f-11ea-88c2-96035d71af86.png" width="600">
   </p>
9. Below is what the completed example should look like.  Strutural engineer or modeler can now clean-up and update model as needed in Revit.
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81620165-14b9e900-93a0-11ea-9b99-7759930f09fc.png" width="600">
   </p>


