# Gazebo-RoboComp Integration Tool: gazeboserver

Currently RoboComp uses RoboComp Innermodel Simulator (`RCIS`), an inbuilt simulator, to check its applications and algorithms. It provides a lot of basic tools and features to easily test and verify an application developed by a developer. But besides its usefulness, more tools and features need to be developed to make it better. So, gazebo was chosen to serve as an secondary options to the developers to test their algorithms and fulfill their needs to the fullest.

So, here we are with a working model of the integration that uses ICE framework as a communication layer combined with the Gazebo's transport layer to have a seamless communication between `gazebo` simulator and any robocomp simulator. For the ones who don't what is Ice, Internet Communications Engine (Ice) is an object-oriented RPC framework that helps you build distributed server-client base applications with minimal effort. I will have a separate blog on it.

So, the model is not integrated with the robocomp's current code base. But I did test it with a robocomp component and it worked absolutely fine. 

Here is a demo video for the testing with I did with a ICE `client` and a `server`. 

[![Watch the video](https://github.com/ksakash/README/blob/master/gsoc18.png)](https://youtu.be/Sy3LZV5b3e8)

`server` instantiates a `LaserI` object which is responsible for connecting to the gazebo simulator. `client` will have the `proxy` to connect with the `server` to listen to its request. For if we do give signal to the `server` then it will print out the laser scan data of the laser sensor used in the simulator.

In the demo I first start the gazebo simulator with the `laser.world` from the project root directory, file which have the necessary information to initiate and populate a world. 
```gazebo --verbose laser.world```

Then to show the channel which on which communication is done:
```gz topic -l``` which lists all the channel published or subscribed by gazebo.

The information about the laser is getting published on a particular topic `/gazebo/gazebo_robocomp_laser/hokuyo/hokuyo/link/laser/scan`. To know what is being published we do:
```gz topic -e /gazebo/gazebo_robocomp_laser/hokuyo/hokuyo/link/laser/scan```

Next to intiate the `server` we go to `build` dir and iniate the server by `./server`. It starts printing the `Getting Callbacks`, that means it is receiving information on the gazebo topic that it `LaserI` has subscibed. 

Next we initiate the `client` by `./client 1`. This makes a request to the `server` to print all the laser data on the console.

This way we are able to communicate with gazebo with the help of ICE interface.

Of course the real tool, which I have named `gazeboserver` which will be very different from it and have presentation and logging techniques, but the idea is the same.
