**Writing our first ever JavaScript based component and trying to explore the new beginnings of RoboComp JS Functionality**
--------------------------------------------

**Hello**, So in the last post we managed to have a new version of integrated with ice-3.6 and we were ready to develop our first js component. Now today we are going to develop a component that has a UI, and connects to a simple non-js component, here we take CameraV4lComp component for our example. We are going to execute this task by getting a brief understanding of what does a component needs in order to function as required. 

----------
Let us setup the directory structure. We create a folder named cameraJs in our components directory. Open the terminal and type in the following commands:

    $ cd ~/robocomp/components/robocomp-robolab/components/
    $ mkdir cameraJS
    $ cd cameraJS

The first step in creating our JavaScript component is to compile our Slice definition of the interface to generate JavaScript proxies. You can compile the definition as follows:

    $ slice2js ../../../interfaces/RGBDbus.ice

The slice2js compiler produces a single source file, RGBDBus.js, from this definition. The exact contents of the source file do not concern us for now — it contains the generated code that corresponds to the RGBDBus interface that was defined in RGBDBus.ice.
Now we start writing our component in JavaScript :
in the same terminal type:

    $ gedit cameraCompJS.js
Now, the code  for our basic JS component is shown below in full:

    var http = require('http');
    var fs = require('fs');    
    var opn = require('opn');
    var Ice = require("ice").Ice;
    var RoboCompRGBDBus = require("./RGBDBus").RoboCompRGBDBus;
    
    var server = http.createServer(function(req, res) {
	    if (req.method.toLowerCase() == 'get') {
	        displayUI(res);
	    } else if (req.method.toLowerCase() == 'post') {
	        processRequest(req, res);
	    }
    });
    
    function displayUI(res) {
	    fs.readFile('UI.html', function(err, data) {
	        res.writeHead(200, {
	            'Content-Type': 'text/html',
	            'Content-Length': data.length
	        });
	        res.write(data);
	        res.end();
	    });
	 }
	
	function processRequest(req, res) {
	     //function to process a post request
	     }
	 
	 var ic;
	 
	 Ice.Promise.try(
    function() {
        
        ic = Ice.initialize();
        var base = ic.stringToProxy("rgbdbus:tcp -h localhost -p 10004");
        var rgbdbus_proxy = RoboCompRGBDBus.RGBDBusPrx.checkedCast(base);

        return rgbdbus_proxy.then(
            function(bus) {

                console.log("Connection successful to " + bus);

                server.listen(1185);
                console.log("server listening on 1185");
                opn('http://127.0.0.1:1185');
            });
    }).finally(
    function() {
        if (ic) {
            return ic.destroy();
        }
    }).catch(
    function(ex) {
        console.log(ex.toString());
        process.exit(1);
    });

The program begins with require statements that assign modules from the npm packages, Ice run time and the generated code to convenient local variables. (These statements are necessary for use with NodeJS. Browser applications would omit these statements and load the modules a different way.)

In the beginning of the program we are creating a node server so that we can serve data to our browser and make an node application, if required.
We define two methods displayUI and processRequest which owe to get and post requests respectively and get triggered as required.

The displayUI function triggers when a get request is observed and it sends the html file(which we are about to setup) to be displayed or rendered by the browser.

The processRequest is a function that gets triggered at a post request and is really useful in implementaions where we are accepting requests from the client(here we are talking about the client of the nodejs server - the user using the browser).

Then the program begins with a call to Ice.Promise.try to launch a chain of promises (or futures) that handles the asynchronous nature of Ice invocations with a structure that resembles synchronous code.

The function passed to try is executed immediately. The body of this function begins by calling Ice.initialize to initialize the Ice run time. The call to initialize returns an Ice.Communicator reference, which is the main object in the Ice run time.

The next step is to obtain a proxy for the remote RGBDBus interface. We create a proxy by calling stringToProxy on the communicator, with the string "rgbdbus:tcp -h localhost -p 10004". Note that the string contains the object identity and the port number that were used by the server. (Obviously, hard-coding object identities and port numbers into our applications is a bad idea, but it will do for now)

The proxy returned by stringToProxy is of type Ice.ObjectPrx, which is at the root of the inheritance tree for interfaces and classes. But to actually talk to our RGBDBus interface, we need a proxy for a RoboCompRGBDBus.RGBDBus interface, not an Object interface. To do this, we need to do a down-cast by calling RoboCompRGBDBus.RGBDBusPrx.checkedCast. A checked cast sends a message to the server, effectively asking "is this a proxy for a RoboCompRGBDBus.RGBDBus interface?" If so, the call returns a proxy of type RoboCompRGBDBus.RGBDBusPrx; otherwise, if the proxy denotes an interface of some other type, the call returns null.

The checkedCast function involves a remote invocation to the server, which means this function has asynchronous semantics and therefore it returns a new promise object.

We call then on the promise returned by checkedCast and supply a "success" function, meaning the code that's executed when checkedCast succeeds. This inner function accepts one argument, bus, representing a proxy to the newly-downcasted object, or null if the remote object doesn't support the RGBDBus interface.

Inside the success function, we now have a live proxy in our address space and can call the server.listen method, passing it the port 1085 (why? because that number is lucky).

The function passed to finally is executed after the try block has completed, whether or not it completes successfully. If we created a communicator in the try block, we destroy it here. Doing this is essential in order to correctly finalize the Ice run time: the program must call destroy on any communicator it has created; otherwise, undefined behavior results. The destroy function has asynchronous semantics, so we return its promise to ensure no subsequent code is executed until destroy completes.

Lastly, the function passed to catch is the default exception handler for this entire promise chain.
 
Given is the content for our basic UI. Make a html file first as follows.
 
 `$ gedit UI.html &`
 
 Now add the following HTML content to the UI.html file. I am using w3 css here we can use our own custom UI(It's a vast field. Experiment and get your own best UI.)

    <!DOCTYPE html>
    <html>
    <title>Demo UI</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css">
    <body>
    <div class="w3-container w3-teal">
    <h1>Welcome to your first ever RoboComp JS Component</h1>
    </div>
    <div class="w3-container">
      <p>You are seeing this message because your component has been successfully connected to the interface and has started.</p>
    </div>
    <div class="w3-container w3-teal">
        <p>This is a default page. Please edit UI.html.</p>
    </div>
    <script type="text/javascript" src="cameraCompJS.js"></script>
    </body>
    </html>

Also, change the config file in the CameraV4lComp/etc folder so that it is according to as feeded in our js component.


    CommonBehavior.Endpoints=tcp -p 10003
    # Endpoints for implemented interfaces
    RGBDBus.Endpoints=tcp -p 10004
    # This property is used by the clients to connect to IceStorm.
    TopicManager.Proxy=IceStorm/TopicManager:default -p 9999
    Ice.Warn.Connections=0
    Ice.Trace.Network=0
    Ice.Trace.Protocol=0
    Ice.ACM.Client=10
    Ice.ACM.Server=10

Now run the CameraV4l providing the above config file. 

    $ cd ~/robocomp/components/robocomp-robolab/components/cameraV4lComp
    $ mkdir build
    $ cd build
    $ cmake ..
    $ make
    $ cd ..
    $ ./build/CameraV4l --Ice.Config=etc/config

This starts the webcam of your system and shows the image in a window. As given below- 
![The CameraV4l component running](https://preview.ibb.co/hNk8n5/camera_V4lrunning.png)

Now we start our new JS component. But before that we install the node modules required in our component. And then we run the component.

    $ npm install opn
    $ node cameraCompJS.js
   
   This starts the node server as shown below and blows up the UI in your default browser:

![The cameraJSComp running](https://preview.ibb.co/bMdTn5/camera_Comp_JS.png)

Browser:

![Demo UI preview](https://preview.ibb.co/kXeeuk/demoUI.png)


**Soon** this week, I will be posting another post in which I will be adding some functionality to these component which uses the functions provided by the interface. 

**BIG ANNOUNCEMENT: ZeroC just released a new stable version of Ice - Ice 3.7. And in next blog post we will also include a short instruction set for integrating the new version with our robocomp framework. **

**Later**, we integrate JS code generation in the code generator. 
 
Till then, adiós amigos. 
Winter is Coming!

