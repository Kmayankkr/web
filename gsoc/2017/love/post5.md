Robocompdsl : Using Component Code Generation Tool for js Component Code generation
------------------------------------------------------------------------

 This is a major blog post as the robocompdsl tool is the core of this project.
In the past weeks I have been studying about the tool. It uses COG and pyparse to pass through cdsl and idsl files and generate code for components using COG tool. 

The code was mainly based on python and also required a bit understanding of cog tool syntax. The COG tool can auto generate code and can be read about on this [webpage](https://nedbatchelder.com/code/cog/). 

So this is the process in which I tackled this problem. 
I decided on making basic functionality in which we can generate a  component by giving it the interfaces it uses in the cdsl file. So a basic cdsl file can be generated as given below.
Go to a folder and type the following commands to make a directory and generate a component  :

    mkdir myComponent
    cd myComponent
    robocompdsl myComponent
This generated a dummy file named myComponent.cdsl. Modify the contents of the file to this:

    import "../../interfaces/IDSLs/RGBDBus.idsl";
    Component myComponent{
    Communications{
        requires RGBDBus;
    };
    language js; 
    gui html;
    };
Now this file defines our new component in the most minimal way possible. In the first line we define the imports. I have imported the RGBDBus interface here by providing its address according to my current folder location. All the interfaces can be found in robocmop/interfaces/IDSLs folder. In the second line we have defines the name of our component. Then we have defined the communications our component will be using. In this we have added that our component requires the RGBDBus interface. In the language we have set the language to js here. By default it is cpp or python. I have changed the robocompdsl tool and I will be adding another post to show the changes I made and explain the code I have written. Right now we are considering to understand how to run the tool and what gets generated when producing a js tool.
In the gui section you can also fill 'none' if the gui is not needed and the component is only server based backend component.

So after this we can compile the component using the following command :

    robocompdsl myComponent.cdsl .

  So this generates two new folders :

 1. etc
 2. src

The src folder contains a folder and two files:

 1. UI (folder)
 2. component.js
 3. makefile

So now our basic component is ready.
To make the component we sue the makefile.
Just type the following commands:

    cd src
    make

As explained in the blog post number 3 we have the file index.html in the UI folder. The component.js file contains the code for server and other functionalities as we made in tutorial post number 3 earlier.
 The UI file automatically displays the list of interfaces the code is using so that we can make sure our component is correctly generated.
The makefile contents are given below:

    
    RGBDBus.js: ${ROBOCOMP}/interfaces/IDSLs/RGBDBus.idsl
    	slice2js ${ROBOCOMP}/interfaces/RGBDBus.ice
    	npm install ice
    	npm install opn
    	npm install fs

So this defines the name of the slice compiled file to be used by the component automatically and automatically provides the address of the idsl files. You just have to use the make command to compile the content.
The proxy string is to be provided to the component
If you want to know how all of these files are generated please consider the last post which explains the working of the robocompdsl tool and how to write code to add new functionalities to it.


