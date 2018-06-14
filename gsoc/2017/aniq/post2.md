# I command thee

May 16, 2017

The coding period hasn’t commenced yet but I needed to test my prototypes before I started working on the final code. I had already cloned the repositories required for the task while I was preparing my proposal for GSoC but I was not able to understand how to make components. I mailed my mentors to guide me on how to start working with the robocomp framework and they replied back, clearing all my doubts. ❤

Following the links in the reply, I started making the component in the robocomp directory to test my prototype code for the Event-based approach to programming the Learnbot. This itself has two subtasks: One is real-time and the other is script. I finished this the same night but I took two days to test and refine my code.

In this post, I’ll explain how I created and tested the components for the above mentioned tasks. To follow this, however, you need to install [RoboComp](https://github.com/robocomp/robocomp) .

## Real-time Terminal version

cd into the robocomp directory

`cd components/robocomp-robolab/components`

```
mkdir terminal_rt
cd terminal_rt
robocompdsl terminal_rt.cdsl
```

A dummy file is generated which we will modify as per our needs

`subl terminal_rt.cdsl`

You may use any other editor, sublime is my personal favorite. ❤

Change the terminal_rt.cdsl file to

```
import "/robocomp/interfaces/IDSLs/Laser.idsl";
import "/robocomp/interfaces/IDSLs/DifferentialRobot.idsl";

Component terminal_rt
{
    Communications
    {
        requires DifferentialRobot, Laser;
    };
    language Python;
};
```

Save the file and then
```
robocompdsl terminal_rt.cdsl build
cd build/src
subl specificworker.py
```
Check the code for specificworker.py [here](https://gist.github.com/Aniq55/111004c6ce47598d3889bd015f9657ef)

Open another terminal and type
```
cd robocomp/files/innermodel
rcis simpleworld.xml
```
This sets up the environment for simulation testing of your components.

Go back to the previous terminal. Make sure you are in the build directory, and type

`python src/terminal_rt.py --Ice.Config=etc/config`

If you get an error at this stage, you need to edit the config file:

`subl etc/config`

And change the first two lines to
```
DifferentialRobotProxy = differentialrobot:tcp -h localhost -p 10004
LaserProxy = laser:tcp -h localhost -p 10003
```
And run the command again.

The following is the list of valid commands, developed till now
```
start
halt
stop
restart
right
right <distance>
left
left <distance>
back
back <distance>
straight
straight <distance>
continue <time in seconds>
turn back
turn left
turn right
turn <angle in degrees>
what is speed
what is rotation
set speed to <speed>
set rotation to <angular speed in rad/s>
quit
exit
```
## Script file version

All the steps remain same as the terminal version. Just use a different name this time, I have used script_dsl
specificworker.py for this can be found [here](https://gist.github.com/Aniq55/8dcff59433b0fb5efae64401e7b7d6df)

To program the Learnbot,

`subl src/commandset.txt`

Write the commands and save the file, then run

`python src/script_dsl.py --Ice.Config=etc/config`

And see the bot follow your commands in the simulator window. 😀

I’ll commit these changes once the coding period begins. 😛

I’ve tested and debugged the components but if you find any bug while running this, please report it in the comments. Thank you 🙂


* * *
Aniq Ur Rahman