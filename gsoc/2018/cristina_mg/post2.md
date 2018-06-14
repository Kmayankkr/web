# My second post
9th June, 2018
## Introduction: Visual Detection Mechanism in Mobile Robots
The main idea of this project is to add robots a mechanism that they can use to be placed in space and know
everything that happens in its environment. To do that, I use different technologies apart from RoboComp. 

Firstly, I use OpenCV (Open Source Computer Vision Library) an open source computer vision and machine learning 
software library. It was designed for computational efficiency and with a strong focus on real-time applications 
which we care much, because our system must react as soon as possible. 

Secondly, we need an object visual detector that allows the robot to know where are placed the object in the real 
world and what they are. So, we implant YOLO (You only look once) a state-of-the-art, real-time object detection
system. By passing an image it detects where are the objects in the scene, and it gives us the coordinates of the 
object, what the object is and the probability of been what it detects the object is. If you're interested in it, 
you can visit they [web site](https://pjreddie.com/darknet/yolo/) where you will find the information to install 
and test it.

As well, I use RoboComp. I create a new component called "ObjectDetection2" that I will explain on the next 
section.

## First approximation
As I said before, the main objective we have proposed was to study, design and implement a predictive mechanism of visual attention for Shelly, the assistance robot of RoboLab, with a basic consciousness system with which you can place an object and yourself in space and predict simple facts. This mechanism allows the robot to maintain a state of preconsciousness about its immediate environment, making it sensitive to any change that occurs. For this, the robot will maintain a model of its environment with which it will make predictions in the immediate future. The contrast of these predictions with the reality perceived through an algorithm based on deep neural networks (YOLO) will allow us to make decisions about the changes that have occurred.

This great objective is divided into several specific parts listed below:
1. Install RoboComp and create the new component. To help you can see RoboComp documentation [right here](https://github.com/robocomp/robocomp/blob/highlyunstable/doc/README.md).
2. From an ".xml" file, previously created, that represents the real world in a synthetic way, obtain the robot's real position and the objects that surround it, with respect to the coordinate axis of the world.
3. Capture the scene perceived by the robot through a RGBD camera and process it to obtain the objects it sees and its position in space.
4. Compare the scene that is perceived from the real world with which Shelly would be perceiving in the synthetic world, for it, both worlds should be as similar as possible.
5. Update the objects position that have changed in a time interval in the synthetic world. Add new objects that appears on the scene and delete the ones that disappears.
6. This state of attention would keep the robot immobile, only updating the scene it perceives, but not the rest of the world. To solve this, we provide mobility to the robot with two motors in the neck: 
- Yaw: the robot can move its head (camera) up and down.
- Pitch: allows robot to move its head (camera) from right to left.
7. Do not take into account the images perceived while the motors of the neck of our robot are rotating, because, these images are not well defined and could introduce errors in our system.

So, the first approximation we do from this premises was a state machine to control all the system. I explain it on [State Machine](#state-machine) section.

## Previous components we have
In our system appear physical actuators like the two engines located in robot's neck that allow the movements of the camera towards the sides, upwards and downwards. To work with them, we use the component *dynamixel*, which has everything necessary to connect our program with physical engines and allows us to control them.

In addition, we have the main sensor, the RGBD camera, for which there is another component called *rgbd*. This allows us to get camera's captures in real time and in a useful format to be able to process them.

Then, to process the images and extract all the information that we are going to use, the YOLO server is needed. This is deployed by starting the component *yoloServer*, which starts the server and connects it to the system.

To emulate the memory we rely on .xml files of the class *innermodel*, which provides us the simulated world's information. This simulated world represents the real laboratory where the robot is located, the research apartment. This simulation is loaded through two components:
* *base*: this component loads the 3D simulation of the robot itself.
* *faulhaber*: this component loads the simulation of the apartment, where the robot is located.

Therefore, using both at the same time we obtain the simulation of the base of the robot inside the simulated research apartment, which allows us to work with the real world.

*Innermodel* is an internal representation system based on .xml files. These files store trees of coordinate transformations, planes and meshes that, together, form the world or the simulation with which we are going to work.

The component *ObjectDetection* is the component created by me to add visual attention to the automaton, it will be explained later.

Finally, there are three main components that allow us to operate the entire system:
1. *commonJoint*: is the central component that connects real and simulated engines, and, through them, the system with the simulation, which allows the program to work correctly. If any of the above components has not been executed or started well, this component can not work.
2. *rcnode (IS)*: is the component that allows communication between all the interfaces of the components and, after all, the components themselves. He is in charge of initializing the Ice middleware.
3. *rcRemote*: manages and monitors the organized execution of the robot component network. It is the first that must be started and does not depend on any other component. The *rcremote* toolset consists of two tools: *rcremoteserver* and *rcremote*, server and client, respectively. The server runs on each of the computers that the robot has and the client must be invoked by *rcmanager*. When a command is executed remotely using *rcremote*, the server uses a tab in *yakuake* (Ubuntu terminal) for each of the components executed, so that the developers can see the textual result. Since the server runs locally, there are no limitations with respect to graphical output.

To help on the system deployment I use *rcManagerSimple* a component that allows you to see the dependences graph and start every component. In this case, our graph was:
![Dependences graph](https://www.overleaf.com/docs/15285048vmbygprtzckx/atts/83866171)

## State machine
A state machine is a system behavior model with inputs and outputs, where the outputs depend, not only on the current input signals, but also on the previous ones.

In this implementation, it is applied to the main control software. There are different states according to the conditions in which the system is at a certain moment.

In our case, the state machine is of binary type, which means that the outputs of the different states are binary, there can be at most two outputs for each state.

The states that make up the state machine are:
* *Predict*: initial state that captures the image and checks where the camera is looking to compare the real world and the synthetic world later. It changes to the *YoloInit* state when it finishes its tasks.
* *YoloInit*: intermediate state that sends the captured image to the Yolo server to process it and obtain the information from it. It goes to *YoloWait* state.
* *YoloWait*: intermediate state that obtains the already processed Yolo image and, thus, labels the different objects that appear in the scene. It goes to *Compare* state.
* *Compare*: compares what it obtains in the first state of reading the synthetic world to what it obtains by the Yolo server, that is, compares the simulated world with the real one. It goes to the *Stress* state.
* *Stress*: if no change has occurred between both worlds, it does nothing. But, if, on the contrary, there has been some change in the position of an object or some change, the .xml file is updated with the perceived novelties. It goes to the *Predict* state.
* *Moving*: is an special state that can be skipped from any other state. For this, the main thread continuously checks if the engines of the "neck" of our robot are moving, if so, it goes to state *Moving* and stays in it until the movement of the engines finishes. From it, the state machine always passes to *Predict* because, at the end of the movement of the camera, the robot has to recalculate where it is looking in order to obtain the data correctly.

In the following figure, a simple representation of the state machine that controls the flow of the program is shown:
![State machine](https://www.overleaf.com/docs/15285048vmbygprtzckx/atts/92361095)

To see the code [visit this link](https://github.com/ljmanso/objectDetection2/commit/a649fbb454e05b6d70e6e5fa67d8ec16655fb47c).
