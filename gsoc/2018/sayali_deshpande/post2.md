# Happy? Sad? Neutral?

13th May, 2018

While applying for GSoC, I had created an emotion recognition prototype. This could recognize 3 emotions: Happy, Sad, Neutral.
So, my first task was to create a new RoboComp component which would recognize these 3 basic emotions, using the code I had initially written.

## Creating the new component

**emotionrecognition.cdsl**

The first step is to create a CDSL  file, which describes the communications of the component with different interfaces and the language used for the component.
```
import "CameraSimple.idsl";
import "EmotionRecognition.idsl";

Component emotionrecognition
{
    Communications
    {
        requires CameraSimple;
        implements EmotionRecognition;
    };
    language Python;
};
```
The component communicates with the CameraSimple component to capture an image. It also implements the functions declared in the `EmotionRecognition.idsl`. The IDSL file describes the interface provided by the component, through which other components can access the emotion recognition data generated. As of now, the interface has only one function, that returns a list of emotions; each corresponding to a face present in the captured image.

**class SpecificWorker**

As the name suggests, the logic specific to this component is written in this class, in the `compute` method. This includes capturing an image using the CameraSimple component proxy, detecting faces and classifying each face into 3 emotion types. The compute method is called at regular time intervals and stores a list of `SEmotion` type of structure for each frame captured. Struct SEmotion contains the location (x,y) and size (width, height) of the face detected, along with the emotion exhibited. 
Other components can access this list with the help of the `getEmotionList()` method, which is implemented in the SpecificWorker. 

**The Convolutional Neural Network (CNN)**

In order to classify an input image into the 3 emotion classes, I had already trained a CNN during the GSoC application period. I used tensorflow for building and training the CNN on the FER2013 dataset. This trained CNN is stored as a frozen graph in `robocomp-robolab/components/emotionrecognition/pb/emotion_classifier.pb`. For adding more number of classes, I would simply have to replace this file and make a few small changes in specificworker.py.

## Using the EmotionRecognition Component

For testing purposes, I have created a component named `EmotionRecognitionClient`. This component communicates with the EmotionRecognition component to get the emotion data and displays it.

**Running the component**

*Note: Make sure Tensorflow and OpenCV are installed in order to run the component.*
```
cd robocomp/components/robocomp-robolab/components
```
Open 2 new terminals.

Terminal 1:
```
cd camerasimple
python src/camerasimple.py --Ice.Config=etc/config
```

Terminal 2: 
```
cd emotionrecognition
python src/emotionrecognition.py --Ice.Config=etc/config
```

Terminal 3: 
```
cd emotionrecognitionclient
python src/emotionrecognitionclient.py --Ice.Config=etc/config
```

Voila! You should be able to see the frames captured through your camera, with a bounding box around each face, and the corresponding emotion. It is not very accurate as of now, and will be improved further.

* * *
Sayali Deshpande
