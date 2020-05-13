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
    <img src="https://user-images.githubusercontent.com/44215479/81784696-1b388580-94b2-11ea-8c21-6dd609911271.png" width="800">
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
    <img src="https://user-images.githubusercontent.com/44215479/81780412-58e5e000-94ab-11ea-82ae-8bc6c0f166d5.png" width="400">
   </p>

9. Backend Process that Consolidates All Export Data Per Each Pile Cap
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81780781-f3462380-94ab-11ea-88c0-abd275b63830.png" width="300">
   </p>
   
10. Output Process to Selected Excel Spreadsheet (Pile_Cap-Type&Location-Schedule.xlsx)
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81781140-897a4980-94ac-11ea-81fb-4400c6642253.png" width="300">
   </p>
  
#### Mapping Script to UI
<p align="center">
 <img src="https://user-images.githubusercontent.com/44215479/81782871-4ff70d80-94af-11ea-9150-3c93c02cecfc.png" width="600">
</p>

1. Dynamo Script Layout 3. - The Column Location Mark
2. Dynamo Script Layout 6. - Performing mapping process
3. Dynamo Script Layout 1. - User to select pile cap associated level as reference level in User Input
4. Dynamo Script Layout 8. - Gets reported out

## Running Script & User Implementation Instructions
1. Clone or download project. </br>
2. In own Revit project or Pile Cap Location & Offset Export example in Model Example folder, open Dynamo and open *Structural-Pile_Cap_Location_And_Offset.dyn*.
3. Ensure Clockwork package is installed. If not, use Package --> Search for Package --> Clockwork in Dynamo.
4. As described in ***Dynamo Script Layout 1.***, use 'File Path' node to select included Excel file (*Pile_Cap_Schedule-Dynamo.xlsx*) and select a reference level in *Level* node (select level pile caps are associated to in model).
5. Modify or add search 'String' node(s) for pile cap foundations and column types as described in ***Dynamo Script layout 2.*** and ***Dynamo Script layout 4.***
6. Select *Run* and check Excel spreadsheet output.</br>
   a. Raw output from Dynamo script specified sheet in User Input.
      <p align="center">
       <img src="https://user-images.githubusercontent.com/44215479/81785613-58e9de00-94b3-11ea-9017-8a953f7dd090.png" width="600">
      </p>
   b. Auto-generated values for distribution and for structural engineer to add loads at top of pile caps.
      <p align="center">
       <img src="https://user-images.githubusercontent.com/44215479/81785906-c5fd7380-94b3-11ea-8c5f-207235d3d3bc.png" width="1000">
      </p>
   c. QC check for correct number of pile caps and types being reported for quantity takeoff.
      <p align="center">
       <img src="https://user-images.githubusercontent.com/44215479/81786055-0826b500-94b4-11ea-8761-f2781384420b.png" width="600">
      </p>

