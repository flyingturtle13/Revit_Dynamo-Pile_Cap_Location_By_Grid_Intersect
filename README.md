# Revit Dynamo - Pile Cap Grid Intersect Location & Offset
Revit does not have a grid intersect parameter for the structural foundation category. However, the structural column category has a parameter called Column Location Mark that indicates the associated nearest grid intersection. This script associates a structural column to the pile cap directly supporting it.  Also, the top of pile cap elevation from user selected reference level and sea level datum offsets are exported as each pile cap elevation vary due to grade elevation.  Ultimately, the pile cap type, grid line intersect, associated column, offsets, and pile cap coordinates from project base point get exported to the included Excel spreadsheet (Pile_Cap-Type&Location-Schedule.xlsx). This workflow and spreadsheet is beneficial during the design development phase when coordinating with the geotechnical engineer to design the pile cap foundation system.  After the model data is exported, the structural engineer would need to add the loads at the base of the columns to the spreadsheet before distributing.    

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
   <img src="https://user-images.githubusercontent.com/44215479/81614036-4cbb2f00-9394-11ea-9b2a-2e84bdc03692.png" width="1000">
  </p>

1. User Input
   * Uses Data Shapes package to create input types and to be used in MultipleInputForm++ (obtain user input)
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81614113-68bed080-9394-11ea-9d35-36fa42638d04.png" width="600">
   </p>

2. Example of Backend Process Per User Input
   * This process makes the selected linked CAD file easily accessed for processing by removing nested lists
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81614285-ac193f00-9394-11ea-80ee-41c54d201a2e.png" width="600">
   </p>

3. User Input
   * Uses Data Shapes package to create input types and to be used in MultipleInputForm++ (obtain user input)
   * Creates consolidated list taking user selection from (2.) and model family type information from (1.).
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81615475-82f9ae00-9396-11ea-8b5c-f72cbfe847e6.png" width="600">
   </p>

4. Example of Backend Process Per User Input
   * Each process accesses user input by associated list index
     * The selected layer will be used to access 3D lines on that layer to be converted to curves (using BimmorphNodes - CAD.CurvesFromCADLayers node)
     * The selected structural frame or structural column type is assigned to curve if user has checked the box for type to be modeled
     * The selected level is assigned for element level association.
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81615475-82f9ae00-9396-11ea-8b5c-f72cbfe847e6.png" width="600">
   </p>

5. Revit Model Element Created
   * StructuralFraming.BeamByCurve or StructuralFraming.ColumnByCurve Node is used to take the user input from (4.) to generate a Revit model element.
   * This is repeated for all curves filtered by user selected CAD Layer.
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81616796-03b9a980-9399-11ea-9cee-2acacfb1cf87.png" width="600">
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


