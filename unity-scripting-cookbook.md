Unity 3D Scripting Cookbook
---------------------------

# Rotating Objects

  #pragma strict
  
  public var speed : float = 10f;
  
  function Update () {
  	transform.Rotate(Vector3.up, speed * Time.deltaTime);
  }

