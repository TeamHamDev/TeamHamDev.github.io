---
title: "Quick-Start Guide"
permalink: /docs/quick-start-guide/
toc: true
page_nav: false
---

## 1. Smart Flying Navigation Tutorial
Place an `OctreeVoxelVolume` Actor in your level. This actor defines the base volume for the 3D navigable space.
A cube, visible upon placement, indicates the actor's presence and initial size, visualizing the boundaries for agent navigation.


![image.png](/assets/images/image_1.png)


![image.png](/assets/images/image_2.png)

Detailed configuration is done in the dedicated Pathfinding Tuner UI. When you select the `OctreeVoxelVolume` in the level, an 'Open Tuner' button appears in its Details panel, providing access to all related settings. Clicking the button opens a UI for centralized control over navigation parameters and actions.


![image.png](/assets/images/image_3.png)


![image.png](/assets/images/image_ui.png)

This UI contains several setting sections for precise control over the plugin's behavior. The meaning and performance impact of each section are described below.
For detailed properties of the `OctreeVoxelVolume`, refer to the [Class Reference](/docs/component-reference/#octreevoxelvolume).

## 2. Baking Navigation Data
Once the core parameters are configured, you perform the bake process. Baking converts the static environment geometry into an optimized voxel grid, saves it as a `BakedOctreeDataAsset`, and ensures runtime pathfinding performance.

### 2.1 How to Perform a Bake
- **Configure Bake Settings:** In the Pathfinding Tuner, you can specify a [`VoxelObstacleProfile`](/docs/component-reference/#octreevoxelvolume) ***(UVoxelObstacleProfile*)*** to select objects for baking based on specific criteria. If no profile is set, all objects with the `WorldStatic` collision type will be baked by default. You should also set the `BakedDataSaveDirectory` to be inside the `Content` folder, e.g., `Content/BakeData`.
- **Prepare the Environment:** Place static meshes and other obstacles to be included in the navigation data within the bounds of the `OctreeVoxelVolume`.
- **Execute Bake:** Select the `OctreeVoxelVolume` in the level and click the 'Generate data asset from selected volumes' button.
![image.png](/assets/images/image_ui_bake.png)

The plugin will then process the geometry, create a `BakedOctreeDataAsset`, and save it to the specified directory.

## 3. Visualizing Navigation Data
After baking, entering Play In Editor (PIE) mode will automatically visualize the baked voxel navigation data in the viewport. Empty space is omitted, and only identified obstacles are displayed as colored voxels.


You can also visualize the bounds of the `OctreeVoxelVolume` itself.
![image.png](/assets/images/image_5.png)
- **Draw Debug Box:** Displays the volume’s primary bounding box as a wireframe. This visualization is visible in the editor and during Play-In-Editor (PIE) mode, but will not be included in a packaged game. Note that rendering the volume has a minor performance cost.
- **Debug Line Width:** Adjusts the thickness of the bounding box lines, useful for visibility in complex scenes or from a distance.
![image.png](/assets/images/image_6.png)

## 4. Setting up the Agent
Once the navigation volume is defined and baked, place an Actor that will operate in the navigable space. This Actor is equipped with a `UPathfindingComponent` and uses the pre-calculated data to navigate the environment.
![image.png](/assets/images/image_7.png)

Selecting an actor that owns a UPathfindingComponent displays that actor’s agent-specific Details panel within the Pathfinding Tuner. 

![image.png](/assets/images/image_4.png)

### 4.1 Agent Settings Parameters
To set a pathfinding goal, you will define a `TargetActor`.
1. Place a destination actor (e.g., `BP_Dest`) in the level.
2. In the Tuner UI, assign this actor to the `TargetActor` field.

![image.png](/assets/images/image_11.png)

Clicking the selection button for the field will display a list of relevant actors in the level to choose from.

For additional agent-related properties, refer to the [Class Reference](/docs/component-reference/#pathfindingcomponent).

## 5. Executing a Pathfinding Test  
After completing the setup, you can verify the pathfinding in PIE mode.

### Prerequisites:
- **Bake Octree Data:** Bake the navigation volume's data into its designated Data Asset.
- **Assign Data Assets:** In the Tuner, assign both the `VoxelObstacleProfile` (StaticObjectProfile) and the baked `OctreeDataAsset`.
- **Set Target:** Specify a `TargetActor` (e.g., `BP_Dest`) and ensure it is located within an active navigation volume.

### Test Steps:
1. Enter Play-in-Editor (PIE) mode.
2. In your Blueprint graph, call one of the following nodes on your `PathfindingComponent`:
    - `Request Path to Target Actor`
    - `Request Path to Location`

![image.png](/assets/images/image_10.png)

Upon calling either, the agent will move from its current location toward the specified target, following the path calculated from the baked octree data.

![image.png](/assets/images/image_9.png)
