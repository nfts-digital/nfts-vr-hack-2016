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
