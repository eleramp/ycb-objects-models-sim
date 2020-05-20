# pybullet-object-models

This repository contains a collection of object models for simulation purposes. In particular, they have been tuned and tested in [pybullet](https://github.com/bulletphysics/bullet3/tree/master/examples/pybullet) for robotic manipulation tasks.

The list of available objects includes:

  - The YCB Video dataset ([posecnn](https://rse-lab.cs.washington.edu/projects/posecnn/), [YCB_Video_toolbox](https://github.com/yuxng/YCB_Video_toolbox)).

  - A few other YCB objects.

  - Custom objects with a superquadric (superellipsoid) shape. These objects are generated by using the [pygalmesh tool](https://github.com/nschloe/pygalmesh). The scripts to generate new object models are also provided (details of usage in the following).

Each object has the following files:

  - Textured mesh for visualization (`.obj`).

  - Simplified mesh for collision (`.obj`). These meshes can be basic shapes or a V-HACD decomposition of the textured mesh, depending on the complexity of the object shape. The decomposition have been done by using the [kmammou/v-hacd](https://github.com/kmammou/v-hacd) library for Blender.

  - URDF file to load the object into the simulation.

Furthermore, there are 3 SDF files, each loading some YCB objects arranged according to a specific layout. The layouts reproduce the layouts of the [GRASPA-benchmark](https://github.com/robotology/GRASPA-benchmark).

## Setup

The repo requires a **python version >= 3**. It can be installed with pip by doing:

```bash
$ git clone https://github.com/eleramp/pybullet-object-models.git
$ pip3 install -e pybullet-object-models/
```

It has been tested only on Ubuntu 16.04 and 18.04 LTS.

If you want to generate your own object models by using the [pygalmesh tool](https://github.com/nschloe/pygalmesh) you need first to install the tools by doing:

```bash
$ conda create -n env_pygalmesh python=3.8
$ conda activate env_pygalmesh
$ conda install -c conda-forge pygalmesh
```

Using conda to install the tool is not probably the ideal option for some people but it is the best and easiest way at the moment, as pointed out in its [github repo](https://github.com/nschloe/pygalmesh).

## Usage

### Import the objects in python
We provide information on how to import the objects in your Python code.
In general you can do:

```python
import os
import pybullet_object_models
from pybullet_object_models import ycb_objects
from pybullet_object_models import graspa_layouts
from pybullet_object_models import superquadric_objects

# Get the path to the objects inside each package

print(ycb_objects.getDataPath())
print(graspa_layouts.getDataPath())
print(superquadric_objects.getDataPath())

# With this path you can access to the object files, e.g. its URDF model

obj_name = 'YcbBanana' # name of the object folder

path_to_urdf = os.path.join(ycb_objects.getDataPath(), obj_name, "model.urdf")
```

Here is a Python example to load the objects inside the pybullet simulation:

```python
import os
import time
import pybullet as p
import pybullet_data
from pybullet_object_models import ycb_objects

# Open GUI and set pybullet_data in the path
    p.connect(p.GUI)
    p.resetDebugVisualizerCamera(3, 90, -30, [0.0, -0.0, -0.0])
    p.setTimeStep(1 / 240.)

    # Load plane contained in pybullet_data
    planeId = p.loadURDF(os.path.join(pybullet_data.getDataPath(), "plane.urdf"))

    flags = p.URDF_USE_INERTIA_FROM_FILE
    obj_id = p.loadURDF(os.path.join(ycb_objects.getDataPath(), 'YcbBanana', "model.urdf"), [1., 0.0, 0.8], flags=flags)

    p.setGravity(0, 0, -9.8)

    while 1:
        p.stepSimulation()
        time.sleep(1./240)
```

### Generate objects with pygalmesh
We provide information about how to generate custom objects by using the pygalmesh tool.

TODO.
