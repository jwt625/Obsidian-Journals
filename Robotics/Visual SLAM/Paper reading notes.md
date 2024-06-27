
# NICER-SLAM: Neural Implicit Scene Encoding for RGB SLAM

https://arxiv.org/pdf/2302.03594

Main question: Can we build a unified dense SLAM system with a neural implicit scene representation for both tracking and mapping from a monocular RGB video?

Challenge:
- depth ambiguity
- harder 3D reconstruction
- optimization convergence

Key ideas:
- coarse-to-fine hierarchical feature grids to model signed distance functions (SDFs) -> detailed 3D reconstructions and high fidelity renderings
- integrate additional signals for supervision
	- easy-to-obtain monocular geometric cues
	- optical flow
	- warping loss to enforce geometry consistency
- to fit sequential input, use a locally adaptive transformation from SDF to density

Contributions
- dense RGB-only SLAM, end-to-end optimizable for both tracking and mapping
	- high quality novel view synthesis
- hierarchical neural implicit encoding for SDF representations
- strong performances in mapping, tracking, and novel view synthesis on both synthetic and real-world datasets. Competitive with recent RGB-D SLAM

Dense visual SLAM
- view centric or world centric
	- view centric: represent 3D geom as depth maps for keyframes
	- world centric: surfels, occupancies/TSDF values in voxel grids
- 

