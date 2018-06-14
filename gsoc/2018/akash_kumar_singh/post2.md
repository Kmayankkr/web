# Gazebo : Introduction

## why do we need simulators at all

Simulator plays an important role in robotics. With rapidly increase in the demand for AI powered robots, it becomes very important to have a place where we could our robot's logic and grab any error as soon as possible and fix it immeaditely. Through simulation we can save valuable time and resources to test our algorithms. Often robotics require expensive sensors and hardware which is not accessible to everyone. Moreover, via a well integrated simulator, we can design and verify the algorithms in virtually any environment. As complex robots are built on frameworks like ROS, a framework-simulator integration is a powerful tool for the future of robotics. 

Simulation modeling solves the real world problems safely and efficiently. It provides a way of analysis whose results can be easily verified, communicated and understood. Apart from the real world, the most basic element in a virtual is bits not atoms. Simulation enables experimentation on a valid and near to real representation of a system. It also allows us to create situations which doesn't even exist in the real world and test our experiment on that.

Gazebo creats a computer-based simulated environment, which can be populated by many many artificially generated models of real objects and provides a way to communicate with each component. When models interact with the environment, it produces a perceptual stimuli to the model, which can manipulate the stimulate the stimuli to imitate the real world.

Simulation is also important in the cases when conducting  a real experiment might seem a little bit impratical or even impossible, having a space where you can change the rule and run the experiment over a no. of iterations as you want. You can see change the scenarios in a no. of different situations and find a most efficient way to do any task.

Simulation modeling provides a safe to test and explore various "what-if" scenarios. Virtual experiments with simulation models are less expensiive and take less timethan experiments with real assets. A simulation model can capture many more details than an analytical model, providing more accuracy.

## Quite a Journey

20 or 30 years back no one would have imagined that we can do something like this. The exponential increase in the computational capabilities have enaled us to create a world within a world through which we can do whatever we want. A world like ours, governing on the same laws of motion, where we can the creators and we change the rules as we want. A virtual world not only helps in getting a place in which we can only create something but also a place we can see the various components in actions, interacting with each other, binded by the same rule. 

The rise in digitalization has been so much that in near future might not be able to differentiate between what is real and what is artificial and this will help in creating even better representation of the world.

The astonishing power of current multi-core processors and graphics chipsets has enabled us to do a realistic physical rendering. A great example is gaming. The way gaming graphics have changed with increase in computation power shows that how real can these virtual worlds can be. Virtual world allows us to break the rule of representation and interaction.

## Why should it work in the real world?

It is very fare to ask that, if we are going to use a robot in real world, so how using a virtual simulator going to make our robot ready for real world applications? Well, real world might be the best place to test our algorithms and checking the behaviour of a robot, but also poses a lot problems. If we have don't have the required resources or equipment on our robot to test our algorithm then we hit a dead end, and we don't have anywhere else to go. It is also not very pratical to every time do the entire setup and making our robot ready for testing then to click a button and do whatever we can. Simulators also give freedom to be creator of our own world. We can decide how we want our algorithm to be tested and we can create a world according to that. We can include trees, sun, winds. We can switch on and off the laws of physics like gravity, friction, etc.

If we are making a robot to be used in a real world, then we want the model to be also like a real world, not exactly but as near as possible. In real world we have equations of motion which determines whatever happens in this universe, like that in the smulation also models should follow the laws of physics to imitate the real world. We don't our robot to be piercing a wall where it should have collided or gaining a instantaneous velocity of 100m/s. If a simulator doesn't have a good physics engine if looses the purposes of its use.

## Gazebo: A simulator

Gazebo is a simulator in which we can create a virtual world and deploy a model to test. Gazebo is designed to accurately reproduce the dynamic environments a robot may encounter. All simulated objects have mass, velocity, friction, and numerous other attributes that allow them to behave realistically when pushed, pulled, knocked over, or carried. Gazebo is far from being the only choice for a 3D dynamics simulator. It is however one of the few that attempts to create realistic worlds for the robots rather than just human users. It's open-source status and so near to perfect depiction of the real world with a large user community and developers makes it one of the best 3d simulator out there.As more advanced sensors are developed and incorporated into Gazebo the line between simulation and reality will continue to blur, but accuracy in terms of robot sensors and actuators will remain an overriding goal.

## Applications of gazebo

Gazebo offers a rich environment to quickly develop and test multi-robot systems in new and interesting ways. It is an effective, scalable, and simple tool that has also potential for opening the field of robotics research to a wider community; thus, for example, Gazebo is being considered for use in undergraduate teaching. Data visualisation, simulation of remote environments, and even reverse engineering of black-box systems are all possible applications.

## Dependencies

![Dependency tree for gazebo](http://library.isr.ist.utl.pt/docs/roswiki/attachments/gazebo(2f)Version_1(2e)0_Design_Specification/gazebo_dependency_graph.png)

Gazebo has a major feature to easily create new robots, actuators, sensors, and arbitrary objects. As a result, Gazebo maintains a simple API for addition of these objects, which we term models, and the necessary hooks for interaction with client programs. Gazebo uses a number of third party libraries to account for physics, visualizaton and rendering. Gazebo uses [ODE (Open Dynamics Engine)](http://ode.org/), [Bullet](https://pybullet.org/wordpress/), [Simbody](https://simtk.org/projects/simbody) and [Dynamic Animation and Robotics Toolkit (DART)](http://dartsim.github.io/) to simulate the dynamics and kinematics associated with articulated rigid bodies. [OpenGL](www.opengl.org) and [GLUT](https://www.opengl.org/resources/libraries/glut/) are used as the default visualisation tools. In order have communication between different components in gazebo, it uses open source [Google Protobuf](https://github.com/google/protobuf) for the message serialisation and [boost::ASIO](https://www.boost.org/doc/libs/1_66_0/doc/html/boost_asio.html) for the tranport mechanism. Gazebo uses [OGRE](https://www.ogre3d.org/) as a rendering library for rendering 3D scenes to both GUI and sensor libraries. Gazebo provides Qt for users to interact to interact with simulation. 

## But how can I manipulate the objects in a simulation?

Gazebo provides a very simple easy way for developers to access the resources of gazebo using plugins. A plugin is a chunk of code that is compiled as shared library and inserted into the simulation. There are generally different types of plugins for controlling different aspect of a simulator. There are 6 kinds of plugins: World, Model, Sensor, System, Visual and GUI. Each plugin provides access to a different part of the model for example: Model plugin controls every aspect of a model and Sensor controls every aspect of a sensor. 

Though plugins are the easisest and best way to access the resources, but there is a different way also. We can also use the apis of gazebo and have access to the simulator as gazebo has. We can do the exact things that gazebo does at the time of startup and create pointers to every element. But that method requires a very clear level of understanding of how gazebo works and uses its APIs. 

## Gazebo Components

There are a couple of essential components in a gazebo simualtion without which it can't run properly. There are 7 major components: `World` Files, `Model` Files, `Environment` Variables, `Server`, `Client`, `Server + Client` and `Plugins`.

1. `World` files contains description about all the elements in simulation, including robots, lights, sensors, and static objects. This file is formatted using [SDF library](https://www.sdformat.org), and typically has a `.world` extension.

2. A `model` file contains all the information about a particular entity which are connected and behave as a single body. This file is also formatted using `SDF`.

3. Gazebo uses a couple of `environmen variables` to know about the files which it going to load at the time of startup like model files, world files, plugins, etc.

4. The `server` is the workhorse of Gazebo. It parses a world description file given on the command line, and then simulates the world using a physics and sensor engine.

5. The graphical `client` connects to a running `gzserver` and visualizes the elements. This is also a tool which allows you to modify the running simulation.

6. The `gazebo` command combines `server` and `client` in one executable.

* * *
*Akash Kumar Singh*

