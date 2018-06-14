# RoboComp-Gazebo Integration : Introduction

May 2, 2018

## About me

I am a sophomore in the Department of Electrical Engineering at the Indian Institute of Technology, Kanpur (IITK), India. I am an AI enthusiast and love to build stuff. I always wanted to work in a environment which allowed me to learn new and exciting things and how to implement them in a practical way. Generally things are very different when you read about them and implement them. Robotics is such a field where we see all the physical laws governing motion of all the objects in action through an artificial machine made out of nuts and bolts. It teaches how the mathematical equation translates into real actions. I am part of the AUV-IITK team at my institute which focuses on, underwater robotics. My experiences as a member of this team has made me familiar with several  Robotics framework and libraries like ROS, and OpenCV. I believe I have plethora of experience in using these frameworks to design software architecture for robots in order to achieve an intelligent behavior. With RoboComp I will have the opportunity to learn more about robotics and improve my skills to better contribute towards open source and robotics.

## About the Project

Simulation plays an important role in robotics. Through simulation we can save valuable time and resources to test our algorithms. Often robotics require expensive sensors and hardware which is not accessible to everyone. Moreover, via a well integrated simulator, we can design and verify the algorithms in virtually any environment. RoboComp uses RoboComp Innermodel Simulator (RCIS), a lightweight non-physics simulator based on OpenSceneGraph, to check its applications and algorithms. It provides a lot of basic tools and features to easily test and verify an application developed by a developer. Though it is extremely useful for various purposes, it has a lot of scope for improvement. Currently RCIS doesn't incorporate physics simulations and does not provide as many tools and features as needed to depict complex environments. In order to work on more demanding robots such as hexapods and drones we need a simulator which can incorporate physics simulation and provide complex environment.

This brings us to choice of integrating the framework with Gazebo. Gazebo is designed for creating a 3D dynamic multi-robot environments capable of rendering the physics and complexity of real world. With the integration between RoboComp and Gazebo, the developers will have access to a more wider variety of applications to test on the simulator with more complex environment and more accurate physical models. Having gazebo as an option for simulations with RoboComp will help grow the possibilities that framework can offer to developers.

The project aims to use the various APIs provided by gazebo to export its resources to other frameworks. We will be mainly using plugins for Gazebo, which act as a shared library, providing one or several of the RoboComp low-level hardware interfaces.

* * *
*Akash Kumar Singh*
