
# Installation

2024-12-19T22:46:36-08:00
Installing following
- https://genesis-world.readthedocs.io/en/latest/user_guide/overview/installation.html
Wow it ran smoothly, I'm amazed.
![[Pasted image 20241219224712.png]]

Skipping all the optional ones for now.

# Test run

2024-12-19T22:50:00-08:00
Running the examples:
- https://genesis-world.readthedocs.io/en/latest/user_guide/getting_started/hello_genesis.html
Error:
![[Pasted image 20241219225028.png]]



Going to check if trimesh is a separate module or one of the options.

Hmm already satisfied??
![[Pasted image 20241219225233.png]]

Full error message:


```json
{
	"name": "NameError",
	"message": "name 'trimesh' is not defined",
	"stack": "---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
File /Users/wentaojiang/Documents/GitHub/PlayGround/20241219_genesis_world/main.py:3
      1 #%%
      2 import trimesh
----> 3 scene = gs.Scene(show_viewer=True)
      4 plane = scene.add_entity(gs.morphs.Plane())
      5 franka = scene.add_entity(
      6     gs.morphs.MJCF(file='xml/franka_emika_panda/panda.xml'),
      7 )

File /Library/Frameworks/Python.framework/Versions/3.11/lib/python3.11/site-packages/genesis/utils/misc.py:27, in assert_initialized.<locals>.new_init(self, *args, **kwargs)
     25 if not gs._initialized:
     26     raise RuntimeError(\"Genesis hasn't been initialized. Did you call `gs.init()`?\")
---> 27 original_init(self, *args, **kwargs)

File /Library/Frameworks/Python.framework/Versions/3.11/lib/python3.11/site-packages/genesis/engine/scene.py:148, in Scene.__init__(self, sim_options, coupler_options, tool_options, rigid_options, avatar_options, mpm_options, sph_options, fem_options, sf_options, pbd_options, vis_options, viewer_options, renderer, show_viewer, show_FPS)
    133 self._sim = Simulator(
    134     scene=self,
    135     options=self.sim_options,
   (...)
    144     pbd_options=self.pbd_options,
    145 )
    147 # visualizer
--> 148 self._visualizer = Visualizer(
    149     scene=self,
    150     show_viewer=show_viewer,
    151     vis_options=vis_options,
    152     viewer_options=viewer_options,
    153     renderer=renderer,
    154 )
    156 # emitters
    157 self._emitters = gs.List()

File /Library/Frameworks/Python.framework/Versions/3.11/lib/python3.11/site-packages/genesis/vis/visualizer.py:26, in Visualizer.__init__(self, scene, show_viewer, vis_options, viewer_options, renderer)
     24     self._context = None
     25 else:
---> 26     self._context = RasterizerContext(vis_options)
     28 # try to connect to display
     29 try:

File /Library/Frameworks/Python.framework/Versions/3.11/lib/python3.11/site-packages/genesis/vis/rasterizer_context.py:37, in RasterizerContext.__init__(self, options)
     34 self.render_particle_as = options.render_particle_as
     35 self.n_rendered_envs = options.n_rendered_envs
---> 37 self.init_meshes()

File /Library/Frameworks/Python.framework/Versions/3.11/lib/python3.11/site-packages/genesis/vis/rasterizer_context.py:52, in RasterizerContext.init_meshes(self)
     42 self.camera_frustum_shown = False
     44 self.world_frame_mesh = mu.create_frame(
     45     origin_radius=0.012,
     46     axis_radius=0.005,
   (...)
     49     head_length=0.03,
     50 )
---> 52 self.link_frame_mesh = trimesh.creation.axis(origin_size=0.03, axis_radius=0.025, axis_length=1.0)
     53 self.link_frame_mesh.visual.face_colors[:, :3] = (0.7 * self.link_frame_mesh.visual.face_colors[:, :3]).astype(
     54     int
     55 )
     56 self.link_frame_mesh.vertices *= self.link_frame_size

NameError: name 'trimesh' is not defined"
}
```



Ok it is from these lines in `rasterizer_context.py`:
```python
try:

from genesis.ext import pyrender, trimesh

from genesis.ext.pyrender.jit_render import JITRenderer

except:

pass

from genesis.utils.misc import tensor_to_array
```
- more specifically from `from genesis.ext import pyrender, trimesh`

Claude sugests `brew install ffmpeg`.
- it did a whole lot of stuff. Ok let's see now...

2024-12-19T23:10:08-08:00
Yes now it runs, but there is no viewer.
![[Pasted image 20241219231318.png]]
Potential issue:
- `[Genesis] [23:09:50] [WARNING] Non-linux system detected. In order to use the interactive viewer, you need to manually run simulation in a separate thread and then start viewer. See `examples/render_on_macos.py`.`
Example: https://github.com/Genesis-Embodied-AI/Genesis/blob/main/examples/render_on_macos.py
- hmm this example ran fine but still no rendering.

2024-12-19T23:15:36-08:00
Well let's try saving the video.
- ok building the scene takes the longest. Should not have killed it.
- why is the frame rate so slow: `[38;5;159m[Genesis] [23:24:55] [INFO] Running at [38;5;121m0.06[0m[38;5;159m FPS.[0m`
- it was ~ 6000 FPS when running in terminal.

2024-12-19T23:34:22-08:00
Ok now the example_render.py for macOS actually opens up the genesis window, and I could pan in it.
- after including the `-v` argument
![[Pasted image 20241219233542.png]]

So the visualization part just does not run on macOS directly
- https://genesis-world.readthedocs.io/en/latest/user_guide/getting_started/visualization.html
- The full code script does not run, and would get stuck
```python
import genesis as gs

gs.init(backend=gs.cpu)

scene = gs.Scene(
    show_viewer = True,
    viewer_options = gs.options.ViewerOptions(
        res           = (1280, 960),
        camera_pos    = (3.5, 0.0, 2.5),
        camera_lookat = (0.0, 0.0, 0.5),
        camera_fov    = 40,
        max_FPS       = 60,
    ),
    vis_options = gs.options.VisOptions(
        show_world_frame = True,
        world_frame_size = 1.0,
        show_link_frame  = False,
        show_cameras     = False,
        plane_reflection = True,
        ambient_light    = (0.1, 0.1, 0.1),
    ),
    renderer=gs.renderers.Rasterizer(),
)

plane = scene.add_entity(
    gs.morphs.Plane(),
)
franka = scene.add_entity(
    gs.morphs.MJCF(file='xml/franka_emika_panda/panda.xml'),
)

cam = scene.add_camera(
    res    = (640, 480),
    pos    = (3.5, 0.0, 2.5),
    lookat = (0, 0, 0.5),
    fov    = 30,
    GUI    = False,
)

scene.build()

# render rgb, depth, segmentation, and normal
# rgb, depth, segmentation, normal = cam.render(rgb=True, depth=True, segmentation=True, normal=True)

cam.start_recording()
import numpy as np

for i in range(120):
    scene.step()
    cam.set_pose(
        pos    = (3.0 * np.sin(i / 60), 3.0 * np.cos(i / 60), 2.5),
        lookat = (0, 0, 0.5),
    )
    cam.render()
cam.stop_recording(save_to_filename='video.mp4', fps=60)

```
The above script get stuck at:
![[Pasted image 20241219233742.png]]




