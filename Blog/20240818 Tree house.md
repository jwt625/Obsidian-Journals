
# 3D print the tree


## Converting the model
2024-08-18T20:33:09-07:00
Got the 3D scanning results from Bryan.

The `.obj` file actually opens in the WIndows 3D viewer:
![[Pasted image 20240818203502.png]]
Could even put the model in mixed reality lol:
![[Pasted image 20240818203530.png]]

Convert to stl/step:
![[Pasted image 20240818203451.png]]

In blender:
![[Pasted image 20240818203614.png]]

Exported to stl. Now it is 28 MB big.

In bambu studio:
![[Pasted image 20240818203728.png]]
![[Pasted image 20240818203711.png]]
![[Pasted image 20240818203912.png]]![[Pasted image 20240818203919.png]]

2024-08-18T20:46:14-07:00
Going to trim it in Blender:

## Trimming
### Step 1: Enter Edit Mode

1. Open your model in **Blender**.
2. Select the model and press `Tab` to enter **Edit Mode**.
![[Pasted image 20240818204901.png]]

### Step 2: Select and Delete Branches or Leaves
b
1. **Select Branches/Leaves**: Use the `B` key to box-select vertices, edges, or faces of the branches and leaves you want to trim. You can also use `C` for circle selection or `L` to select linked vertices.
    
    - **Tip**: Use `X-ray Mode` (press `Alt+Z`) to select through the model, making it easier to grab all relevant geometry.
2. **Delete the Selection**: After selecting the parts you want to remove, press `X` and choose **Vertices**, **Edges**, or **Faces** to delete the selected geometry.
    

### Step 3: Simplify the Mesh

1. **Use the Decimate Modifier**:
    - If your tree model is still too complex, you can apply the **Decimate Modifier** to reduce the polygon count.
    - Go to the **Modifier Properties** panel, add a **Decimate Modifier**, and adjust the **Ratio** slider to simplify the mesh.

### Step 4: Check and Clean Up

1. **Remove Doubles**: In Edit Mode, press `M > Merge by Distance` to remove overlapping vertices.
![[Pasted image 20240818205803.png]]
- This merge is the best

1. **Check for Non-Manifold Edges**: Go to `Select > Select All by Trait > Non-Manifold` to find and fix any problematic edges that might cause issues during printing.

2024-08-18T21:24:52-07:00
Trimmed:
![[Pasted image 20240818212454.png]]

Before:

![[Pasted image 20240818212714.png]]
After:
![[Pasted image 20240818212735.png]]

Now it looks better in bambu:
![[Pasted image 20240818213148.png]]

\

## Fixing selection in Blender:
To select everything inside the selection box, including elements hidden behind other geometry, you'll need to enable **X-ray mode** or disable **Limit selection to visible**. Here's how to do it:

### Method 1: Enable X-ray Mode

1. Press `Alt+Z` to toggle **X-ray mode** on. This mode makes the model semi-transparent, allowing you to select geometry through the model.
2. Now, when you use `B` for box selection or `C` for circle selection, Blender will select everything within the selection area, including hidden geometry.

### Method 2: Disable Limit Selection to Visible

1. If you prefer not to use X-ray mode, you can disable the **Limit selection to visible** option.
2. In the top bar of the 3D Viewport, next to the selection tools, there's a small icon that looks like two overlapping squares (in older versions of Blender, it's a cube with dots). Click this icon to disable it.
3. With this option disabled, your selection will include geometry hidden behind other parts of the model.

Now you should be able to select everything inside the selection area, even if it’s not directly visible from your current viewpoint.


## Print
2024-08-18T21:39:20-07:00
Started printing.


## Print not working

Many disconnected layers...
Tried to fix it with the following suggested by ChatGPT. Not working:
### Step 3: Repair the Non-Manifold Edges

There are several methods to repair these edges, depending on the problem:

#### Method 1: Fill Open Edges

1. If you find open edges (edges that should be connected to form a closed mesh):
    - Select the edges or vertices around the open edge.
    - Press `F` to create a new face that closes the gap.
    - Repeat this for all open edges.

#### Method 2: Merge Vertices

1. If the non-manifold issue is due to vertices that should be merged:
    - Select the vertices you want to merge.
    - Press `M` and choose **At Center** or **By Distance** to merge them. **By Distance** is useful for automatically merging vertices that are close together.

#### Method 3: Remove Internal Faces

1. If the non-manifold issue is caused by internal faces (faces inside the model that shouldn't exist):
    - Select the internal faces manually or use `Select > Select All by Trait > Interior Faces`.
    - Press `X` and choose **Faces** to delete them.

#### Method 4: Bridge Edge Loops

1. If you have two open edge loops that need to be connected:
    - Select the two edge loops (you can do this by holding `Alt` and right-clicking the edges).
    - Press `Ctrl+E` and select **Bridge Edge Loops** to connect them with new faces.

### Step 4: Check for Further Issues

1. After repairing the non-manifold edges, recheck your model by repeating `Select > Select All by Trait > Non-Manifold`.
2. If no more issues are selected, your model is now manifold and should be good for 3D printing.

### Step 5: Final Cleanup

1. **Remove Doubles**: If you suspect there are overlapping vertices, use `M > Merge by Distance` to clean them up.
2. **Recalculate Normals**: Press `Ctrl+N` (or `Shift+N` in some versions) to recalculate the normals, ensuring that all faces are oriented correctly.