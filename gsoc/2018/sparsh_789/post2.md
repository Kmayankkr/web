## Creating New node

**In this post**, I will elaborate the work done till now and difficulties faced by me and how I resolved them. I want to thank Pablo sir, for being a very supportive mentor, for providing help and explaining me files involved.

The current repository of my work is [here](https://github.com/sparsh789/robocomp). If you want to use Robocomp InnerModel editor to create new nodes and edit old nodes in innerModel files you can install my fork of robocomp. For any installation related issues you can contact me at ksparsh30@gmail.com .

While using Robocomp InnerModel Editor at first, I was greeted by a segmentation fault. By investigating and indulging with it for approx. one week, I finally found out that there was a problem with version of OpenSceneGraph then installed in my PC. Version compatible with robocomp and that I had installed were different. So, I tried to remove wrong version of OpenSceneGraph and install correct one and installed robocomp from beginning. But now errors came in compiling Rc innermodel editor and it was a totally messed up situation. So after I fed up, I finally started with fresh distribution of ubuntu 16.04. Now, I was able to compile and run RcinnerModelEditor.

During the process of solving errors, I became familiar with most of the codebase of RCinnerModelEditor and also I got handy with Qt. So, after I have running RCInnerModelEditor I was able to see some bugs in tool and resolved them.

In talking with my mentor Pablo Sir, he told me to add feature for creating new nodes in RcInnerModelEditor. So for that first I started with GUI part where user will enter properties. And then by using those properties it will create new nodes in scene. In GUI part user have to enter new node Id and its parent ID and type of node. According to type of node a property tab will open. After entering property and saving the file new node will be entered. I also made reload button by which user can reload the same file for checking changes. For demo video click [here](https://drive.google.com/file/d/1o2SdhleIWfdH-z-5DfuABGhPlvHCaXGN/view?usp=drivesdk).

After writing code there were few problems like opening a new file after saving current file were giving errors. This was a problem due to breaking of signals function in between. For that I disconnected signals before opening new files. There were some other small bugs also that I resolved.

### Current Work

**Now**, currently I am working on editing of nodes by simple mouse based positioning. For that I am getting familiar with innermodel libraries and codebase.
