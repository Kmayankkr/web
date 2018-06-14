# Gazebo Plugins

## what are plugins?

So, plugins are technically a chunk of code that is compiled as a shared library and inserted into the simulation. The plugin has direct access to all the functionality of gazebo through the standard c++ classes. This allows the developers to have control over almost every aspect of Gazebo. It is also the easisest way to communicate with gazebo. Plugins allows us to programmatically alter the simulation and test our algorithms in the simulation.

There are mainly 6 types of plugins: `world`, `model`, `sensor`, `system`, `visual` and `GUI`. Each plugin type manages a different component of Gazebo. For example, a `Model plugin` is attached to and controls a specific model in Gazebo. Similarly, a `World plugin` is attached to a world, and a `Sensor plugin` to a specific sensor. The System plugin is specified on the command line, and loads first during a Gazebo startup. This plugin gives the user control over the startup process. These plugins are loaded at startup by the gazebo `server` by looking at the environment variable `GAZEBO_PLUGIN_PATH`, which contains paths to all the plugin present in the system. `Pointers` having access to the corresponding element in the simulator are passed to the plugin, in this way plugins have direct access to the components in a simulation. 

## How plugins obtain access to the components?

`SDF` is an `XML` format that describes objects and environments for robot simulators, visualization, and control. SDF is capable of describing all aspects of robots, static and dynamic objects, lighting, terrain, and even physics. SDF can acurately describe all aspects of a robot using SDF, whether the robot is a simple chassis with wheels or a humanoid. In addition to kinematic and dynamic attributes, sensors, surface properties, textures, joint friction, and many more properties can be defined for a robot. 

SDF library is used to parse xml files in order to create sdf element pointers for the respective elements in the xml file. So, when the sdf is parsed at the time of startup the pointer of sdf element containing the plugin filename is passed to the plugin. It has all the information about the elements it includes and can access it. It can also provide essential info like `World Name` and the `Model Name` and their properties. There is a whole lot of elements that can be nested inside a `sdf` file. It is avaible in [sdf's documentation](http://sdformat.org/spec).

```
<?xml version="1.0" ?>
<sdf version="1.5">
  <world name="default">
    <physics type="ode">
      ...
    </physics>
    <scene>
      ...
    </scene>
    <model name="box">
      ...
    </model>
    <model name="sphere">
      ...
    </model>
    <light name="spotlight">
      ...
    </light>
  </world>
</sdf>
```

## Basic components of a plugin 

All the plugins have a bunch of members functions and data members that are common to each plugin. `Constructor` and `Desctructor` are obvious, except that we have a `Load` function, called by the server during startup, which loads all the information related to the plugin and through this function the pointers are passed and those pointers contains all the relevant of the component of simulator. 

```
void GazeboRoboCompCamera::Load(sensors::SensorPtr _parent, sdf::ElementPtr _sdf)
{
    CameraPlugin::Load(_parent,_sdf);
    std::cout << "Load: " << " " <<  this->parentSensor->Camera()->Name()<< std::endl;

    this->parent_sensor_ = this->parentSensor;
    this->width_ = this->width;
    this->height_ = this->height;
    this->depth_ = this->depth;
    this->format_ = this->format;
    this->camera_ = this->camera;
}
``` 

This is the function which we can overload whatever we want to do. Like storing the pointers and intialising all the transport communication. Also all the sensor plugin comes with a fuction like `OnNewLaserScans`, `OnNewFrame`, `OnNewDepthFrame`, etc. depending on the type of sensor given in the `.sdf` file. This functions are called whenever there is an update to the sensors, These are already binded with the update `event` so we don't have to do them ourselves. We only need to overload the functions to behave it our way. For example, we can store all the sensor data and broadcast it to somewhere else where it is needed.

```
void OnNewFrame(const unsigned char *_image, unsigned int _width, unsigned int _height, unsigned int _depth, const std::string &_format)
{
  if (seed == 0)
  {
    image_.create(_height, _width, CV_8UC3);
  }
  memcpy((unsigned char *) image_.data, &(_image[0]), _width*_height * 3);
  cv::imshow("Display Window", image_);
  std::cerr << "Showing Image" << std::endl;
}
```

Also one of the greatest features of gazebo is that we can easily bind any function to a event. Gazebo provides a number of such event connectors. It is the same way in which all the update functions in a `sensor` plugin are called. It is done in this way:

```
this->updateConnection = event::Events::ConnectWorldUpdateBegin(std::bind(&ModelPush::OnUpdate, this));
``` 

The function `OnUpdate` will be called everytime when the simulation is updated strating from the beginning of the world simulator. Another most commonly used `event` connectors are `connectUpdated` used in sensors, `connectJointUpdate` used in joint controllers, `animationConnection` used to update an animation, etc.

Inorder to communicate with the APIs outside the gazebo plugin we need some way through which we can listen to the request of external `APIs` and can get all the data gathered by the sensor in the data. In this case we can use `gazebo::tranport` library inorder to send data in and out of the plugin. For this we need to initiate a `node` inside the plugin which can be used to declare `subscriber` and `publisher` in that node.

```
gazebo_node_ = transport::NodePtr(new transport::Node()); 
gazebo_node_->Init(world_name_);
sub_ = this->gazebo_node_->Subscribe(sub_topic_name_, &GazeboRoboCompDiffDrive::OnMsg, this);
pub_ = this->gazebo_node_->Advertise<gazebo::msgs::Pose>(pub_topic_name_);
``` 

`Subscriber` is attached to any `callback` function which is called when something is published on the topic. Gazebo allows us to our own custom `messages` inorder to have special data types according to our own convenience. Gazebo uses open source `google::protobuf` library for as data types for communication over a channel. A typical `.proto` is like this:

```
package gazebo.msgs;

message Vector3d
{
  required double x = 2;
  required double y = 3;
  required double z = 4;
}
```

This is a message type which is used to represent anything which have 3 `double` values. We have include in `CMAKE` file inorder to generate the header included to use that message.

## Some Insights into the working of plugins

There are certain shortcuts which gazebo provides which further eases the procedure to access the gazebo simulator. One of them is that, as soon the gazebo loads a world it start publishing to the topics by default which can then subscribed outside the plugin in an API instead of redirecting it through the plugin.

There are basically two major components in a gazebo simulation: `Model` and `Sensor`. `Model` can include all the parts in a simulation moodel which is physical and has a structure and shape like `links`, `joints` or `models`. On the other hand gazebo sensor is not something which can be seen with the eyes like a model. It is something which can binded to a part of a model.

Gazebo also a lot of inbuilt plugins from which we can inherit and build more complex and customised plugin: `CameraPlugin`, `IMUPlugin`, `ContactSensorPlugin`, `ModelPlugin`, `WorldPlugin`, `DepthSensorplugin` and many like this.
* * *
*Akash Kumar Singh*
