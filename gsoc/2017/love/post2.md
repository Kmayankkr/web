**The Baby steps to a Superpower in Making**
--------------------------------------------

**Hello**, if you have read my last blog post you might be knowing about my project in detail. I would restate my objective here for the first timers. My goal is to provide JavaScript support to the current version of RoboComp. The code generation that we have now is only equipped with code generation for python or cpp based components. I have to add the functionality of JavaScript code generation to the current framework.

----------

**Now,** I will explain in brief why I chose to call the JavaScript support a superpower. Just go to this [link](https://insights.stackoverflow.com/survey/2017#most-popular-technologies). It is evident from the link that the number of JavaScript programmers is approximately three times the number of programmers who use Python or C++. So currently as a open source community, RoboComp is open to only around 25 percent of the developers on stackoverflow but addition of JavaScript support will make it open to a huge pool of developers to participate in. 


----------
**So** now I elaborate the work I have done till now and the difficulties faced by me during the journey till now and how I overcame them. I will specially thank Marco Sir, for being a very understanding, supportive and resourceful mentor, helping me a lot in dealing with my mistakes and failures.

*The **current repository** of my work is [here](https://github.com/lovemehta/RoboComp-JS). Go here directly if you wish to install the new version of robocomp integrated with Ice 3.6.3 alongwith instructions for installation for JS distributions and dependencies on Ubuntu 17.04. Please leave a comment or mail at lovemehta.me@gmail.com for issues while installing.*

I started with understanding the codebase of robocomp. Trying to understand the cdsl and idsl  and code generation part. I also gave some time to brushing up my nodejs skills and learning and experimenting with the Ice platform later on in the process.

Now as I had to work with the newer version of Ice I started to install the new version of Ice without removing the older one - this lead to a system error in ubuntu every time I tried that is why the new installation instructions I wrote, clearly state that the old version of zeroc-ice has to be purged before installing our new version. It took a lot of time for me to figure this out and I had to install with a fresh distribution of Ubuntu. Still I am unclear of what the problem might be but purging old binaries and files sorts out the path for us.

Now we install new distribution of Ice for all the platforms one by one.
The instructions are clearly given on this [link](https://github.com/lovemehta/RoboComp-JS) of the new repo.

Now installation of the Ice distribution for Python and JS is quite tricky and will not work for us, if we follow the zeroc website as they do not describe the dependencies and permission adjustments of npm, pip, node etc. on their website. Follow strictly the instructions on the above link, that I have created. These instructions have been made clear by trying to install and then finding out the problems and tackling these problems one by one. It was a time consuming process and getting the things to work hand in hand in the global environment was really the work of patience. 

Also after properly installing Ice for all the required platforms we start on installing RoboComp. I removed the old dependencies for ice 3.5 that we were installing earlier and installed robocomp in the normal way. But as the linkages were not correct I faced a lot of repetitive errors and it was a totally messed up situation while compiling RoboComp. So finally I discussed this issue with Marco Sir and he figured out the problem and changed the cmake files and also suggested some changes in the .bashrc to make it work. So finally we had a running version of RoboComp with Ice 3.6 ready to support individual JS components. In this process I faced a number of difficulties and ended up figuring out a bug in Zeroc Ice documentation and demo files.

Also throughout this time I was continuously posting on zeroc forums and getting replies from the members of the zeroc community to help me with the integration. 

Here are some of the posts I posted on zeroc forums [1](https://forums.zeroc.com/discussion/46528/cannot-find-module-icejs-error-while-running-client-js-with-node#latest),[2](https://forums.zeroc.com/discussion/46527/no-command-slice2js-found-error-when-trying-to-compile-helloworld-application#latest),[3](https://forums.zeroc.com/discussion/46526/cannot-use-ice-demos-on-ubuntu-16-04-urgent-help#latest),[4](https://forums.zeroc.com/discussion/46524/installing-slice2js-gives-errors#latest),[5](https://forums.zeroc.com/discussion/46523/using-ice-3-6-with-robocomp-instead-of-ice-3-5-gives-errors#latest). As it is evident from the posts that the staff is really helpful and is great at suggesting solutions.

**Soon** this week, I will be posting another post in which I will be showing a tutorial for how to write a JS component yourself that can connect with another robocomp component.

**Later**, we integrate JS code generation in the code generator. 
 
Till then, adiós amigos.

