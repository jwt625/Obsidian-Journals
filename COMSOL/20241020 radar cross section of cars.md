
2024-10-20T08:59:55-07:00
Looking for a model for Tesla Cybertruck. And maybe a comparison to Ford 150.

Tried a few models, either has faces cannot be oriented, or does not forma solid properly. Probably still fine for scattering sim.

Trying out this model:
- https://fetchcfd.com/view-project/1570-tesla-cybertruck-3d-cad-model-for-aerodynamics-study

At least the size looks about right:
![[Pasted image 20241020090321.png]]

Okay:
![[Pasted image 20241020090518.png]]

Ok maybe it is time to read:
- https://www.comsol.com/model/stl-import-tutorial-series-30951

2024-10-20T09:32:25-07:00 Merged the mesh to 0.1 m in Blender:
![[Pasted image 20241020093216.png]]
Great:
![[Pasted image 20241020093250.png]]
- https://www.comsol.com/forum/thread/34342/self-intersection
The legend Ivar is always there:
![[Pasted image 20241020093425.png]]

Ok it at least imports if I do not simplify the mesh:
![[Pasted image 20241020093501.png]]
![[Pasted image 20241020093512.png]]
![[Pasted image 20241020093541.png]]
2024-10-20T10:09:13-07:00
Ok used absolute tolerance and it worked.
![[Pasted image 20241020101654.png]]
![[Pasted image 20241020101902.png]]
Ok ignored that surface and fixed
![[Pasted image 20241020102156.png]]
![[Pasted image 20241020102359.png]]
![[Pasted image 20241020102411.png]]
LOL look at this:
![[Pasted image 20241020102438.png]]
![[Pasted image 20241020102714.png]]

2024-10-20T10:45:38-07:00
Going back to the first model.
Found the imported mesh is in the global definitions:
![[Pasted image 20241020104600.png]]
- and there are more settings like relative repair tolerance. Using 3e-3 instead of 1e-8.
![[Pasted image 20241020104918.png]]
- found `minimal` boundary partition is not working when importing later in the geom node

2024-10-20T18:54:00-07:00
More errors
![[Pasted image 20241020185358.png]]
Found I could play with different mesh import relative tolerance to get different errors at Form Union or mesh.
- 0.01 mesh import relative tolerance work up to Form Union
2024-10-20T19:14:13-07:00
Finally meshed, cheated with convex hull:
![[Pasted image 20241020191449.png]]
![[Pasted image 20241020191424.png]]
![[Pasted image 20241020191435.png]]

Found the COMSOL tutorial is using scattered wave formulation:
![[Pasted image 20241020193743.png]]
- uhh waves are inside the car. Need to remove it...
https://www.comsol.com/support/knowledgebase/983
![[Pasted image 20241020203756.png]]
![[Pasted image 20241020205922.png]]




























































