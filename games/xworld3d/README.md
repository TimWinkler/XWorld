# XWorld3D
## <img src="../../doc/xworld3d.png">

## Game description
In XWorld3D, a virtual agent learns language and vision abilities. The agent sees the world in a first-person view, listens to the commands by a virtual teacher, and takes navigation actions to receive rewards. It should learn to understand the teacher's language from scratch by navigating to the correct target places. It learns simultaneously the visual representations of the world, the language, and the action control.

## Install

#### Dependencies

XWorld3D has additional dependencies. Because it uses [EGL](https://en.wikipedia.org/wiki/EGL_(API)) for rendering, currently XWorld3D can only be run on NVIDIA GPUs.

Install [FFmpeg](https://www.ffmpeg.org/) and [TinyXML](http://www.grinninglizard.com/tinyxml/):

```bash
sudo apt install ffmpeg libtinyxml-dev
```
For installing ffmpeg on Ubuntu 14.04, you need to do:
```bash
sudo add-apt-repository ppa:mc3man/trusty-media
sudo apt-get update
sudo apt-get install ffmpeg
```

Roboschool requires gcc 5.x. On Ubuntu 16.04, this requirement is already satisfied. On Ubuntu 14.04, you need to do
```bash
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo apt-get install gcc-5 g++-5
```
Note: you don't have to set gcc 5.x as the default compiler.

#### Build

To enable the build of XWorld3D, you need to turn on the option to CMake:
```bash
cmake -DWITH_XWORLD3D=ON ..
```

#### Test
```bash
cd <xworld_path>/examples; ./test_xworld3d
```
or
```bash
cd <xworld_path>/python/examples; ./test_xworld3d.py
```

## Create
* Python name: ```xworld3d```
* C++ constructor name: ```X3Simulator```
* Name for the unified C++ simulator interface: ```xworld3d```

## Flags
|**Name**|**Description**|
|:-------|:---------------|
|```pause_screen```|Pause the screen when show_screen() is called, until any key is pressed. (Default: false)|
|```x3_conf```|The JSON file for configuring XWorld3D. (Default: "")|
|```curriculum```|A scalar flag reserved for the purpose of curriculum learning; the user can decide what to do with it. (Default: 0.0)|
|```x3_training_img_width```|The width of the training input image. (Default: 84)|
|```x3_training_img_height```|The height of the training input image. (Default: 84)|
|```x3_move_speed```|The velocity of the agent during navigation. (Default: 25.0)|
|```x3_turning_rad```|The turning speed in radian of the agent. (Default: PI/8)|
|```x3_big_screen```|Whether use a big screen or not. A big screen is usually used for visualization. (Default: 0)|
|```color```|Whether use color (1) or grayscale (0) images for training. (Default: 1)|
|```context```|How many consecutive frames are used to represent the current sensor input. (Default: 1)|
|```visible_radius```|The visible radius of the agent. For a visible radius of N>1, the agent sees an NxN area in front of it. If a visible radius of 0, the whole environment map will be the training input image. (Default: 0)|

## How to define your own XWorld3D tasks
This process is exactly the same with defining new tasks in [XWorld2D](https://github.com/PaddlePaddle/XWorld/tree/master/games/xworld).

## How to add new objects
Each object in XWorld3D is represented by an .obj file. You can download new .obj files and put them in ```<xworld_path>/games/xworld3d/models_3d```. After this, call ```<xworld_path>/games/xworld3d/obj_normalize.py``` to rescale the object to a unit size. By default, the new objects will be randomly sampled, together with the existing objects, when initializing environment maps. You can also specify the .obj path in your Python class that defines the environment map.

## XWorld3D is flexible
As a Python interface embedded in C++, teacher can dynamically change the environment at every time step, potentially according to the agent's performance and/or behaviors, which is important if you want to implement curriculum learning.

The teacher's sentences can be generated by a context-free grammar (```<xworld_path>/python/context_free_grammar.py```) at each time step of each task. You can define the grammar in an easy way and decide when to generate what sentence in a task.