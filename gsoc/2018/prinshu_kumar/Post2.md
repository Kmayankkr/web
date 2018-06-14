# Creating the tab structure similar to the tabs of the web browser
I have created the tabular structure where user can add robots by clicking on the "AddRobot" button. A new tab will be created similar to the tabs of the web browser.

## Change in the interface
I have changed the ``` interface.ui ``` file to fit the QTabWidget and also made an interface for the tab which will contain the information of the one robot. The user can click on the button provided to add the robots. Then using the ``` pyside-uic ``` the corresponding python file is generated.
A button namely ``` start(collaborative) ``` has also been added where user can start the collaborative robots. 

## Changes in the ``` Learnblock.py ``` file
I have created a file ``` guiTab.py ``` file which contains the class for the tabs and corresponding functions for the robot. Every time the user clicks on the ``` AddRobot ``` a new object of the tab is created. 
I have also kept the ``` Learnblock ``` class as the main class which starts the application. 

## Configuring the robots
Earlier the robots had the static configuration file. I have made it dynamic. There is a button named ``` setConfiguration ``` which sets the configuration dynamically. Once the button is clicked then a pop up window asking for the ip address and the port number of various sensors is executed. Once the user provides the correct information then the configuration file is dynamically created. 

## Using paho-mqtt for the collaborative robotics
I have planned to use the paho-mqtt library of the python for the collaborative robotics. The robots will be publishing and subscribing to the topics and hence they will be communicating with each other.
I will also be making the tutorial and the examples for the same.
