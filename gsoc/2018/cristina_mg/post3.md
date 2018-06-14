# My third post
12th June, 2018
## Deepening in the state machine
On the second post I talk about the state machine to control the system. At this post I am going to talk about these states to show what they do and what I have done.
Firstly, I am going to explain certain things that I did not tell before. To create an artificial consciousness, the first thing we need is "memory", the basis on which we will support the entire subsequent implementation. As we have already said, in the ideal case, this memory would be an .xml file in which the system can save the data during the execution, but, since all these data are not compatible with our file, we have made a hybrid between this .xml file from *innermodel* and the five auxiliary lists of type TObject. This type is an structure like this:
```C++
struct TObject
	{
		QString name; // Object name in innermodel
		std::string type; // Object type from YOLO
		bool explained; // True if object exists in the real world
		int idx; // Object number id
		std::vector<QVec> bb; // Bounding box
		std::vector<QVec> projbb; // Bounding box projection 
		float intersectArea; // Intersection area with YOLO Bounding box
		QPoint error; // Error from bounding box to YOLO
		QVec pose; // Object position over the table
		bool assigned; // Bool to check if object is assigned 
		float prob; // Probability of been the object detected by YOLO
		RoboCompYoloServer::Box box; // 3D box that contains the object
	};
  ```
These list support the things that *innermodel* can not store, like the bounding box important to know where is the object on the scene. The five lists are:
1. *listObjects*: total objects list that the robot controls as existing in the world.
2. *listYoloObjects*: objects list that Yolo server detects after analyzing the camera captured image at a certain time.
3. *listCreate*: new objects list that appear in a capture, either because, previously, Yolo server had not detected them or not with a probability higher than 35%, or because someone external has put on the field of vision of the robot a new object. These objects will be created at the right time.
4. *listDelete*: objects list that, in some previous capture, existed and no longer. It could be because Yolo server detected an object where it did not really exist, someone removed an object from the field of view of the camera or the memory that we loaded at startup was not completely updated.
5. *listVisible*: visible objects list at a given moment, that is, objects that at a moment enter complete in the field of vision of the camera. If, for example, the robot sees only part of the object in the capture, it is not taken as visible.

After contextualize, I can continue with the post.

### *Predict* state
This is the first state, where the machine starts. It must 
This initial state goes through the complete list of objects *listObjects* and, for each of them, it checks if it is within the limits of the capture of the camera, that is, if the robot would see it at that moment. If this condition is met, the box that is printed later on the green image is created, as shown in the figure \ ref {fig: objDetec} and, in addition, the object is added to the list of visible objects.
