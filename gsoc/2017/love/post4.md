Working on the Robocompdsl Tool
-------------------------------

The core of the coed generation mechanism are the COG and pyparse libraries. Robocompdsl tool uses these to generate its own components using the predefined cdsl and idsl. 

The tool takes both idsl and cdsls as input and generates corresponding files in cpp or python. But our aim is to generate some code content in js language when given as input.
So as our preliminary goal let us try to implement the js functionality to process cdsl files first.

We start by quick intro of a cdsl file and the process to generate it.

Writing CDSL files is easy --their structure is simple and the number of reserved words is very limited-- robocompdsl can generate template CDSL files to be used as a guide when writing CDSL files. Start by creating a new directory for your component named, for instance, mycomponent. Then run the code generator:

    robocompdsl path/to/mycomponent/mycomponent.cdsl


This will generate a CDSL file with the following content:

    import "/robocomp/interfaces/IDSLs/import1.idsl";
    import "/robocomp/interfaces/IDSLs/import2.idsl";
    Component CHANGETHECOMPONENTNAME
    {
	Communications
	{
		implements interfaceName;
		requires otherName;
		subscribesTo topicToSubscribeTo;
		publishes topicToPublish;
	};
	language Cpp;
	gui Qt(QWidget);
	};
Let's change the template file above by something like this,

    import "/robocomp/interfaces/IDSLs/DifferentialRobot.idsl";
    import "/robocomp/interfaces/IDSLs/Laser.idsl";
    Component MyFirstComp{
    Communications{
        requires DifferentialRobot, Laser;
    };
    gui Qt(QWidget);
    language Cpp; //language Python;
    };
 and save it as mycomponent.cdsl. Now run again robocompdsl with the CDSL file as first argument and the directory where the code should be placed as the second argument.

From the component's directory:

    cd path/to/mycomponent
    robocompdsl mycomponent.cdsl .
So this process generates a demo component in the directory. The whole process on how to use this component is give [here](https://github.com/robocomp/robocomp/blob/master/doc/robocompdsl.md). Please go through the link once for a better understanding.

Now we have to generate similar functionality for js.
For this we have to make some changes in the robocompdsl's code.

The code that I added to the robocompdsl.py file is given below. And I will explain it below how it helps us generate a js component.

    elif component['language'].lower() =='js':
		#
		# Check output directory
		#
		if not os.path.exists(outputPath):
			creaDirectorio(outputPath)
		# Create directories within the output directory
		try:
			creaDirectorio(outputPath+"/etc")
			creaDirectorio(outputPath+"/src")
			creaDirectorio(outputPath+"/src/UI")
		except:
			print 'There was a problem creating a directory'
			sys.exit(1)
			pass
		#
		# Generate regular files
		#
		files = ['src/component.js','src/UI/UI.html','src/makefile']
		specificFiles = ['src/UI/UI.html', 'src/component.js', 'src/makefile']
		for f in files:
			ofile = outputPath + '/' + f
			if f in specificFiles and os.path.exists(ofile):
				print 'Not overwriting specific file "'+ ofile +'", saving it to '+ofile+'.new'
				ofile += '.new'
			ifile = "/opt/robocomp/share/robocompdsl/templateJS/" + f
			if component['gui'] != 'none':
				print 'Generating', ofile, 'from', ifile
				run = "cog.py -z -d -D theCDSL="+inputFile + " -D theIDSLs="+imports + ' -D theIDSLPaths='+ '#'.join(includeDirectories) + " -o " + ofile + " " + ifile
				run = run.split(' ')
				ret = Cog().main(run)
				if ret != 0:
					print 'ERROR'
					sys.exit(-1)
				replaceTagsInFile(ofile)


So the first line checks if the language is set as js in the cdsl file. These variable like the language variable are already parsed using the cdslparsing code. Language key gives the language in which the component is required. Now after that it is pretty evident in the code that we have first generated the required directories. And after that we generate the files required for the component. 

For doing this we have to provide some template files to be parsed by the cog tool in order to generate the component in js.
It can be seen in the code that the parser looks for the template for each particular file to generate a new file for the component.

I will explain the templates I have added for js in the templateJS folder.

1. The [UI.html](https://github.com/lovemehta/RoboComp-JS/blob/master/tools/robocompdsl/templateJS/src/UI/UI.html) in the src/UI folder
This is the basic UI as explained in the last post. It contains the cog code to enumerate the names of the interfaces required and also to print the name of the component as the title.

2. The component.js file
	

/
	
	
	/*
	[[[cog
	import sys
	sys.path.append('/opt/robocomp/python')
	
	import cog
	def A():
	    cog.out('<@@<')
	def Z():
	    cog.out('>@@>')
	def TAB():
	    cog.out('<TABHERE>')
	
	from parseCDSL import *
	includeDirectories = theIDSLPaths.split('#')
	component = CDSLParsing.fromFile(theCDSL, includeDirectories=includeDirectories)
	]]]
	[[[end]]]
	 *    Copyright (C)
	[[[cog
	A()
	import datetime
	cog.out(' ' + str(datetime.date.today().year))
	Z()
	]]]
	[[[end]]]
	 by YOUR NAME HERE
	 *
	 *    This file is part of RoboComp
	 *
	 *    RoboComp is free software: you can redistribute it and/or modify
	 *    it under the terms of the GNU General Public License as published by
	 *    the Free Software Foundation, either version 3 of the License, or
	 *    (at your option) any later version.
	 *
	 *    RoboComp is distributed in the hope that it will be useful,
	 *    but WITHOUT ANY WARRANTY; without even the implied warranty of
	 *    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
	 *    GNU General Public License for more details.
	 *
	 *    You should have received a copy of the GNU General Public License
	 *    along with RoboComp.  If not, see <http://www.gnu.org/licenses/>.
	 */
	
	
	var http = require('http');
	var fs = require('fs');    
	var opn = require('opn');
	var Ice = require("ice").Ice;
	
	[[[cog
	    for namea, num in getNameNumber(component['requires']):
	        if type(namea) == str:
	            name = namea
	        else:
	            name = namea[0]
	        if communicationIsIce(namea):
	            cog.outl("var RoboComp"+name+"= require(\"./"+name+"\").RoboComp"+name+";")
	
	]]]
	[[[end]]]
	
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
	    var base = ic.stringToProxy("");
	[[[cog
	    for namea, num in getNameNumber(component['requires']):
	        portNumber=1185
	        if type(namea) == str:
	            name = namea
	        else:
	            name = namea[0]
	        if communicationIsIce(namea):
	            cog.outl("var "+name.lower()+"_proxy = RoboComp"+name+"."+name+"Prx.checkedCast(base);\n return "+name.lower()+"_proxy.then(\n" +
	                '<TABHERE>'+"function("+name.lower()+") {\n"+
	                '<TABHERE>'+"//add specific functionality here\n"+
	                '<TABHERE>'+"console.log('Promise Accepted');\n"+
	                '<TABHERE>'+"console.log('Connection successful to' + "+name.lower()+");\n")
	            cog.outl( "server.listen(%d);" % portNumber)
	            cog.outl("console.log('server listening on %d');" % portNumber)
	            cog.outl("opn('http://127.0.0.1:%d');\n" %portNumber)
	            cog.outl("});\n }).finally(\nfunction() {\n    if (ic) {\n    console.log('Promise completed.');\n        return ic.destroy();\n    }\n}).catch(\nfunction(ex) {\n    console.log('Promise Rejected');\n    console.log(ex.toString());\n    process.exit(1);\n});)")
	        portNumber= portNumber+1;
	
	]]]
	[[[end]]]`

So this is a cog based file in which the cog tool expands the code written betwen the [[[ and ]]] brackets. We are just iterating over the imports and generating different connections with each of them over ports iterating one by one from 1185 to 1185 + n.

We use a makefile in the end whose structure is given below. In this we do the same thing and generate a makefile for the component by iterating over the imports.

    [[[cog
    import sys
    sys.path.append('/opt/robocomp/python')
    import cog
    def A():
    cog.out('<@@<')
    def Z():
    cog.out('>@@>')
    def TAB():
    cog.out('<TABHERE>')
    from parseCDSL import *
    includeDirectories = theIDSLPaths.split('#')
    component = CDSLParsing.fromFile(theCDSL, includeDirectories=includeDirectories)
    ]]]
    [[[end]]]
    [[[cog
	for namea, num in getNameNumber(component['requires']):
		if type(namea) == str:
			name = namea
		else:
			name = namea[0]
		if communicationIsIce(namea):
			cog.outl(name+".js: ${ROBOCOMP}/interfaces/IDSLs/"+name+".idsl")
			cog.outl('<TABHERE>'+"slice2js ${ROBOCOMP}/interfaces/"+name+".ice")
			cog.outl('<TABHERE>'+"npm install ice")
			cog.outl('<TABHERE>'+"npm install opn")
			cog.outl('<TABHERE>'+"npm install fs")
			]]]
			[[[end]]]
We have used the environment variable here to access the interface directory. We are also installing the dependencies for the node server to avoid unnecessary typing in the end.

I also made some small changes in the pyparse files to also include js in the system. This included adding js variable everywhere along with the cs and python.

I will be sharing in the next post how to use this tool to generate a component and then work on it to make it have a specific function.
