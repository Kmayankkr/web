# JavaScript Support

13th June 2017

# INTRODUCTION


My name is Love Mehta. I am a Bachelor of Technology (B.Tech), Sophomore Year, Computer
Science and Engineering undergraduate student at Indian Institute of Technology, Ropar

I want to work in open source because I believe in software freedom. I use Ubuntu for my day-to-day
work and software development. I also use several other open source services like Libreoffice,
Moodle, Mozilla-Firefox etc. Open-Source development has introduced me to a whole new world of
software development which I was unaware of previously. It has been a great experience to learn
new frameworks (like Phaser, Cordova, Ionic etc.) and to work on building something new using
them from my sophomore year itself. I have enjoyed my journey of getting to know people on gitter,
irc and mailing lists while applying for GSoC and learning from them. I think it would be the best
utilization of my summer holidays to work on an open-source project.
In my understanding to the depth, this project aims to extend the current DSL based component code generator
to also generate RoboComp components that can be run in the browser. The new components
generated using js and as an html file should be able to deployed in the browser and also be able to
communicate with the non JS components already developed earlier (laser, keyboardcontroller,
joystick etc.). It is an interesting diversion from current robotics technologies based on C++/ python
to use JavaScript to code some highly concurrent components. 
The RoboComp component model is inspired by ORCA and has evolved to fit our needs along the
years. Components need to communicate with each other in order to run simultaneously and provide
a real world simulation or a utility or an interface. For this communication between the components
RoboComp uses a middleware called Ice. This project is a big step in making the RoboComp
middleware agnostic. The components use interfaces defined using IDSL which is a subset of Ice’s
Slice Interface Language. The CDSL defines the component by importing the interfaces, stating the
target language and the communication dependencies required with other components. Using this
cdsl file and our code generation tool robocompdsl RoboComp generates a component code which
is ready to be compiled and run.
The robocompdsl tool has templates defined for both cpp and python. For adding js functionality,
templateJS has to be added. So that when robocompdsl is working on parsing the cdsl it can
generate JavaScript code if the language var has been set to javascript.
The parsecdsl.py and parseidsl.py files parse the input cdsl and collects the useful data like imports,
communication dependencies, language variable to form a component tree. This tree is given to the
robocompdsl.py which later makes a function call to the cogapp with a specific template depending
upon the language variable (cpp or python). These templates are defined in separate folders in the
robocompdsl tool folder. We have to provide the js template as a part of this project and edit the
robocompdsl.py accordingly to make it work properly for js component code generation. This
components when after being generated should be able to deployed on the browser using certain
provided commands. These commands will run a nodejs server and deploy the component. The
server will be equipped to be able to communicate with the non js components and other js
components. As nodejs provides a server side communication and data streaming framework, it is
the most suitable framework to be used for this project Their are various implementations available
to connect the nodejs server or client with other webrtc clients or peers. In my understanding this
project requires exploration on these domains to finally settle with a completely working modified
communication system or js components and a new version of robocompdsl.
Technologies (programming languages, etc.) used :
Mainly this project will be based on changing the robocompdsl and cog. This requires me to work on
python and to have a thorough understanding of the cog module by Ned Batchelder. Also we are
going to use node.js as a tool for server side operations while deploying the js components. Node.js
is a very powerful free open-source framework built on Google Chrome’s JavaScript V8 Engine. We
can develop I/O intensive web applications which require continuous data streaming. This will be a
perfect tool for the non js components using ice framework to communicate with the frontend and the
server.

Currently I have been working on acquanting myself with the code base and with IDSL, CDSL, code generation and the ZeroCIce Framework.
Initially we plan on replicating a basic python component to JavaScript before the first evaluations, for example we can do one that connects to the camera component and shows images in the browser. 
Then we can generalize the code for components and integrate it in the generator. 

* * *

Love Mehta