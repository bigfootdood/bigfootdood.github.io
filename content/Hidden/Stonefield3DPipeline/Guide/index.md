---
title: "Stonefield 3D Visual Pipeline Guide"
date: 2025-04-15
draft: false
description: "Project Links for Stonefield"
tags: ["project","stonefield"]
---

# Civil3D to Revit Workflow Guide v0.02

This tutorial demonstrates how to use Autodesk Revit to quickly generate supplemental 3D visuals for existing CAD drawings created in Civil 3D/AutoCAD. As an example, it walks through creating enhanced graphics for a Cut/Fill diagram.

---

### Workflow Overview:

![[Workflow.png]]

### 1. Extract Basemap & Satellite Imagery from Autodesk Forma

- Create a project basemap in Forma and import it into Revit
    
- Apply stylization to the basemap in Revit using template files
    
- Prepare the map for integration with CAD data
    

### 2. Extract Surfaces from Civil 3D Project

- Isolate and extract terrain surfaces for use in Revit
    
- Import surfaces into Revit and convert them into Toposolids
    
- Position the surfaces on the basemap and apply textures
    

### 3. Create Multi-View Sheet in Revit

- Set up multiple views to clearly communicate the site context
    
- Lay out a sheet
    
- Export the model for use in other software such as Blender or Unreal Engine
    

---

# Part 1: Forma

### Setup:

To get started, visit the [Autodesk Forma Homepage](https://www.autodesk.com/products/forma/overview) and click **"Access your Forma Hub."**

Click **"Create New Site"** in the top-left corner. In the popup window, with your appropriate hub selected (or the default), press **"Create New Project."**

Name the project and press **Create.**

Type in the site address and use the bounding box to crop the desired basemap:  
_Example Project Address: 48 Orchard Place, Morganville, New Jersey 07751, United States_  

![[Pasted image 20250707190411.png]]

> **Note:** Extend the map well beyond the property boundary, as this cannot be extended later—only cropped.

Once the project loads, you should see the **Order Data** page:  

![[Pasted image 20250707191413.png]]

> _If not, access it via the globe icon on the left and select **"Browse Contextual Data."**_

**Add the following layers:**

- **Buildings** from Open City Model, OSM Buildings, or USA Structures
    
- **Satellite Image** via Mapbox
    
- **Terrain** via ESRI
    
- **Parcels** via Regrid _(may require payment)_
    

![[Pasted image 20250707193006.png]]

Click **"Create Site Limit"** on the right side of the map and trace the property boundary using parcel data:  

![[Pasted image 20250707194106.png]]

### Optional: 3D Vegetation

Install the **Thicket Extension** from the Extensions panel:

![[Pasted image 20250707204334.png]]

Create a new **"Tree Area"** to group multiple tree varieties:

![[Pasted image 20250707204945.png]]

Click **"+"** to add tree types.

> _To save time: `ThicketTrees.json` is in the assets folder. Import it from the top of the Thicket window._

Click **"Place"** to draw tree areas on the map:  

![[Pasted image 20250707205534.png]]

![[Pasted image 20250707205718.png]]

## Exporting

Install the **Revit Add-In** via the following tab:  

![[Pasted image 20250707210222.png]]

Back in Forma, press **"Send to Revit Add-In":**  

![[Pasted image 20250707211750.png]]

Open Revit and create a new project.

In the **Massing & Site** tab, click **"Load Proposal"**:  

![[Pasted image 20250707210541.png]]

> _Note: You must be logged into the **same Autodesk account** in both Revit and Forma._

Configure the import via **Options**:  

![[Pasted image 20250707221508.png]]

Click the **house icon** to open the 3D view:  

![[Pasted image 20250707213459.png]]

Change the visual style via the cube icon:  

![[Pasted image 20250707213647.png]]

Switch to **Consistent Colors**:  

![[Pasted image 20250707213741.png]]

## Cleanup

Hide the red parcel and building layers (Right-click → Hide in View → Elements).

> **Tip:** Use the blue light bulb icon to view hidden elements:  
> ![[Pasted image 20250707214436.png]]

### Fixing Roads

Imported roads are _subdivisions_ of the basemap **Toposolid** and must be adjusted to appear properly:

1. **Select only the roads:**  
    Use **Modify > Filter** and select only **Subdivisions**  
    
    ![[Pasted image 20250707221147.png]]
    
2. **Change road color:**  
    Set the **Toposolid Type** to **Asphalt**
    
3. **Raise road elevation:**  
    Set **Sub-Divide Offset** to `1'` in the **Properties** panel  
    
    ![[Pasted image 20250707222634.png]]
    

Your basemap is now ready for site data:  

![[Pasted image 20250707230546.png]]

---

# Part 2: Exporting Site Data

With the basemap ready, export the following from Civil 3D:

1. **Cut/Fill Heatmap** (as 3D solids)
    
2. **Existing Surface** (survey data as a Toposolid)
    
3. **Proposed Surface** (as a second Toposolid)
    

## Cut/Fill Heatmap

Select the Cut/Fill surface → **Surface Tools > Extract from Surface > Extract Objects > Elevations**:  

![[unnamed.png]]  
![[unnamed-1.png]]

Right-click the surface in the **Prospector Tab > Edit Surface Style**:  

![[Pasted image 20250715143902.png]]

## Exporting Surfaces

Because this Revit model is for visual supplement only, use the `WBLOCK` method instead of linking:

1. Select the surface and run `EXPLODE`
    
2. Run `WBLOCK` to export each surface as an individual `.dxf`
    

![[Pasted image 20250715150001.png]]

> _Note: `.dxf` offers better compatibility with software like Blender, Illustrator, or Unreal._

Make sure the **Existing Surface** is trimmed to the property boundary:  

![[Pasted image 20250715160815.png]]

You should now have 3 `.dxf` files:  
![[Pasted image 20250715161803.png]]

---

# Part 3: Revit

> _Tip: Group the basemap objects for easier visibility management._  
> ![[Pasted image 20250715172238.png]]

### Importing & Aligning the Existing Surface

Go to **Insert > Import CAD**, choose the `.dxf` file, and set **Positioning** to **Auto - Center to Center**:  
![[Pasted image 20250715173731.png]]

Use the **Move Tool** to align the CAD surface with the basemap:  

![[Pasted image 20250715173850.png]]  
![[Pasted image 20250715173954.png]]

### Creating a Toposolid from CAD

In **Massing & Site > Toposolid > Create from Import > Create from CAD**:  

![[Pasted image 20250715174420.png]]  
![[Pasted image 20250715174543.png]]  
![[Pasted image 20250715174637.png]]

Click **OK** to create the Toposolid.

Hide the CAD file (Right-click → Hide in View).

Select the Toposolid and set **Height Offset From Level** to `-3'` or as needed:  

![[Pasted image 20250715175836.png]]

Double-click the basemap terrain → **Modify > Excavate** → Select the new Toposolid → Finish:  

![[Pasted image 20250715180353.png]]

The property now contains accurate survey topography:  

![[Pasted image 20250715180854.png]]

## Building Views

Right-click and duplicate the default 3D view:  

![[Pasted image 20250715192504.png]]

### Custom Graphics Per View

Create a new view and isolate terrain for profile comparison:

![[Pasted image 20250717162522.png]]

Import the Cut/Fill Heatmap: **Insert > Import CAD (Center to Center)**

Align using **Offset From Workplane** in **Properties**, and **Move Tool**:  

![[Pasted image 20250717163318.png]]

Switch to _Front_ view via the navigation cube. Disable contour lines in **Visibility/Graphics Overrides**:  
![[Pasted image 20250717163552.png]]  
![[Pasted image 20250717163905.png]]

### Section Box

Enable **Section Box** for scene cropping:  

![[Pasted image 20250717165158.png]]

Duplicate the Cut/Fill Heatmap to isolate _Cut_ and _Fill_ volumes:  

![[Pasted image 20250717165634.png]]  
![[Pasted image 20250717170008.png]]

### Displace Tool

To make heatmaps appear in front of terrain **per view**, use the **Displace Tool**:  

![[Pasted image 20250717171837.png]]  
![[Pasted image 20250717171621.png]]  
![[Pasted image 20250717172151.png]]  
![[Pasted image 20250717173123.png]]

### Additional CAD Overlays

Import full CAD project → Align using **Offset From Workplane** and **Move Tool**:  

![[Pasted image 20250718122241.png]]

Use **Query Tool** to delete unnecessary layers:  

![[Pasted image 20250718123040.png]]  
![[Pasted image 20250718131310.png]]

## Assembling a Sheet

Right-click the **Sheets** tab in **Project Browser** → **New Sheet**:  

![[Pasted image 20250718132627.png]]

Import a title block or use the default:  

![[Pasted image 20250718132813.png]]

Drag views from **Project Browser** into the sheet. Adjust **View Scale** in **Properties**:  

![[Pasted image 20250718133014.png]]

Disable section box visibility via **Visibility/Graphics Overrides**:  

![[Pasted image 20250718133659.png]]

With customized views and layouts, you can now create compelling visuals to supplement CAD drawings:  

![[Pasted image 20250718152227.png]]

# Exporting to Blender/Twinmotion

Export as `.OBJ` or `.FBX`:  

![[Pasted image 20250718153219.png]]

> _OBJ tends to preserve materials and colors better._