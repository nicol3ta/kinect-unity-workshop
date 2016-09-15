# kinect-unity-workshop
This is a sample app demonstrating how you can leverage Kinect with Unity3D 


##Adding the Kinect plugin 

* 	Download the Unity Pro packages from developer.microsoft.com/windows/kinec/tools
*	After you finish downloading the zip file, extract it to a known location
*	The folder contains three Kinect plugins (the basic one, face recognition, gesture builder), each wrapping functionality from the Kinect v2 SDK
*	Open Unity and create a new project
*	Add the Kinect plugin into your new project by selecting Assets from the top menu and then Import Package | Custom Package …
*	Drag and drop the KinectView samle project into the Assests directory in your project folder
*	Select KinectView and then double click on the MainScene scene contained inside it. This should open up your Kinect-enabled scene inside the game window. Click on the single arrow near the top center of the IDE to see your application in action. The Kinect will automatically turn on and you should see a color image, an infrared image, a rendering of any bodies in the scene and finally a point cloud simulation.

##Enabling motion tracking with Kinect

*	Create a new GameObject by clicking GameObject -> Create Empty
*	Rename it to BodySourceManager
*	Select it and add Body Source Management as component 
*	Create again a new GameObject by clicking GameObject -> Create Empty
*	Rename it to Object1 
*	In Assets create a new folder and call it Script
*	In this folder create a C# script and name it 'DetectJoints' 
*	Drag and drop it on Object1
*	Now let’s create a particle system we will be able to move with our hand. Right click on Object1, create 3D Object, and give it a particle system
*	Double click on the 'DetectJoints' script will open Visual Studio 
*	In the Visual Studio solution add the import statement: `using.Windows.Kinect;`
*	Asign the Body Source Manager game object that we just created to the script by adding a public variable `public GameObject BodySrcManager;` and assigning the `GameObject` to `Object1` in Unity. 
*	Create a `JointType` enum to enable selection of the joint you want to track 
`public JointType TrackedJoint;`
*	Save the script and go back to Unity and select from the drop down menu of TrackedJoint the joint you want to track. For this example, I chose HandLeft. 
*	Add a private variable that takes care of the data that we get from the Body Source Manager component from the script. 
`private BodySourceManager bodyManager;`
We assign it once and grab the data from there. We take care of that in the `Start()`-Method

```c#
// Use this for initialization
void Start () {

 if (BodySrcManager == null)
 {
   Debug.Log("Asign Game Object with Body Source Manager");
 }
   else
 {
   bodyManager = BodySrcManager.GetComponent<BodySourceManager>();
 }
}
```
*	With every frame we want to check if `BodySrcManager` is still null. In this case we return
*	Otherwise we grab the data

```c#
// Update is called once per frame
void Update () {
	
 if(BodySrcManager == null)
 {
return;
 }
 bodies = bodyManager.GetData();
 if(bodies == null)
 {
       return;
 }
 foreach (var body in bodies)
 {
 if(body == null)
 {
 continue;
 }
 if (body.IsTracked)
 {
  var pos = body.Joints[TrackedJoint].Position;
  gameObject.transform.position = new Vector3(pos.X, pos.Y);
 }
}
```
* Kinect can track up to six bodies so we need to assign it to an array that will differentiate between the different bodies. 
* Declare the array `private Body[] bodies;`
* We save the script and run the game. As you can see the object doesn’t move very far. That’s why we need a multiplier. 
* Let’s create one 
`public float multiplier = 10f;`
and multiply it to the position of the object
` gameObject.transform.position = new Vector3(pos.X * multiplier, pos.Y*multiplier);`
	
* Let’s start the program again. You can see the movement has now a bigger impact 

