Unity 3D Scripting Cookbook
---------------------------

## Rotating Objects

```csharp
using UnityEngine;
using System.Collections;

public class rotate_object : MonoBehaviour {
	// behaviour code goes here, inside class

	// this is a comment
	
	/* this is a comment
	 * that spans
	 * multiple lines
	 */
	
	// this is a variable
	// its public, meaning it's accessible outside of this class
	// it is of type float
	// it has a default value of 10
	public float speed = 10f;

	// this is a function 
	// it has a special name that is executed once at the start
	void Start ()
	{
		// log the speed to the console
		Debug.Log (speed);
	}

	// this is a function
	// it has the special name 'update' which means it runs on every frame
	void Update ()
	{
		transform.Rotate(Vector3.up, speed * Time.deltaTime);
	}
}
```

## Changing Colour on Keyboard Interaction

```csharp
using UnityEngine;
using System.Collections;

public class change_colour_cs : MonoBehaviour {

	// Initialization
	void Start () {
		gameObject.GetComponent<Renderer>().material.color = Color.red;
	}
	
	// On every frame
	void Update () {
		if(Input.GetKeyDown(KeyCode.R))
		{
			gameObject.GetComponent<Renderer>().material.color = Color.red;
		}
		if(Input.GetKeyDown(KeyCode.G))
		{
			gameObject.GetComponent<Renderer>().material.color = Color.green;
		}
		if(Input.GetKeyDown(KeyCode.B))
		{
			gameObject.GetComponent<Renderer>().material.color = Color.blue;
		}
	
	}
}
```

# Gaze base interaction

1. Import CardboardSDKforUnity.unitypackage, if you haven't already.
2. Create an empty object in your scene, rename it to  "Virtual Me". Position it at {0,1.70,0}, this is the middle of the scene at eye level. 
3. Drag CardboardMain into this empty game object.
4. Select the Main Camera that is still in the scene, disable it by unchecking the top left checkbox in inspector. Also disable the Audio listener for this Main Camera.
5. Inside your "Virtual Me" object, select CardboardMain -> `Add Component` -> `Physics Raycaster`. We need this components to detect the direction of our gaze.
6. From Project/Cardboard/Prefabs/UI drag `CardboardReticle` into your VR `Main Camera`.
7. Download and Import [this skybox package](https://www.assetstore.unity3d.com/#!/content/4042), we'll use these later for the interaction.
8. Drag an object into your scene or select an existing object you want to define interaction for. Add component -> new script, call it `GazeBehaviour`. Open the script and copy the following code:

```csharp
using UnityEngine;
using UnityEngine.EventSystems;
using System.Collections;

public class GazeBehaviour : MonoBehaviour {
  public Renderer skybox;
  public Material sky1;
  public Material sky2;
  public Material sky3;
  public Material sky4;
  private int currentSky = 0;

  void Start () {
    
  }

  public void staringAt() {
    switch (currentSky) {
      case 0:
        skybox.material = sky2;
        currentSky++;
        break;
      case 1:
        skybox.material = sky3;
        currentSky++;
        break;
      case 2:
        skybox.material = sky4;
        currentSky++;
        break;
      case 3:
        skybox.material = sky1;
        currentSky = 0;
        break;
    }
  }
}
```

With the code saved, we now have access to the 5 public variables in Unity.

* First, drag a `Skyball_WithoutCap` from SkySphere_V1/Meshes into your scene. Position it at {0,0,0} with an X rotation of -90.
* Now drag the `Skyball_WithoutCap` from your scene into the slot for the public variables that are part of the "Gaze Behaviour" script inside your object's inspector.
* Then drag 4 materials of your choice into the slots for Sky 1,2,3,4.

Next we want to trigger the behaviour on the object, we need an “Event Trigger” to actually call the `staringAt()` function. 

* With your object selected, use “Add Component” and find the “Event Trigger” component. Then once it is in your lamp object, click “Add New Event Type” and choose “PointerEnter”.
* Drag your object from the scene view into the slot underneat "Runtime Online" inside the "PointerEnter" event trigger. Next, from the dropdownlist, choose `GazeBehaviour.startingAt` to link the `PointerEnter` event to the behaviour we've defined in our script.

For this all to work, our scene also needs an `EventSystem`. Add it to the scene via UI -> Event System

* In the Event System's inspector, disable the Standalone Input Module and the Touch Input Module. Those would be competing with the GazeInputModule we are adding next.
* Add new component -> `GazeInputModule`. Check 'VR Mode Only'

To detect that we are looking at our object, the object needs a 'collider'.

* Select your object, add component -> choose a collider shape that roughly matches the shape of your object.
* If necessary, adjust the scale and postition of your collider.

