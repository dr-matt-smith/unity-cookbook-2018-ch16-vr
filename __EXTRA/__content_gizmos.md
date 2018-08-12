
# Editor Extensions and Gizmos

In this chapter, we will cover the following topics:

1. Gizmo to show selected object in Scene panel
1. Gizmo to display icon for object type
1. Gizmo to draw a grid in the Scene panel


<!-- *********** intro *********** -->
# Introduction

xxx

# The big picture

XXX editor / onsepct / script obejts - CUSTOMIZING the Editro ....*[]:



## Gizmos

Gizmos are another kinds of Unity editor customization. Gizmos are provide visual aids to game designers in the Scene panel. They can be useful as setup aids (to help us know what we are doing), or for debugging (understanding why objects aren't behaving as expected).

Gizmos are not drawn through Editor scripts, but as part of Monobehavious - i.e. they only work for GameObjects in the current scene. Gizmo drawing is usually performed in 2 methods:

- OnDrawGizmos(): This is executed every frame, for every GameObject in the hierarchy

- OnDrawGizmosSelect(): This is executed every frame, for just the/those GameObject(s) that are currently selected in the hierarchy

Gizmo graphical drawing makes it simple to draw lines, cubes, spheres. Also more complex shapes with meshes, and also displaying 2D image icons (located in the Assets/Gizmos folder).

Several recipes in this chapter illustrate how Gizmos can be useful. Often new GameObjects created from Editor Extensions will have helpful Gizmos assocaite with them.





Gizmos Unity manual entry:

    - https://docs.unity3d.com/Manual/GizmosMenu.html


<!-- ******************************* -->
<!-- ******************************* -->
<!-- ******** new recipe ********** -->
<!-- ******************************* -->
<!-- ******************************* -->

# xxx

xxx



<!-- *********************** xx ************************* -->

## How to do it...

To create a menu with a menu item to log messages to console follow these steps:

1. In the Project panel create a new folder Editor.

1. In your new Editor folder create a new C# script-class named ConsoleUtilities.cs, containing the following:

```chsarp
using UnityEditor;
using UnityEngine;
using System.Reflection;

public class ConsoleUtilities : EditorWindow
{
    [MenuItem("My Utilities/Clear Console")]
    public static void ClearLogConsole()
    {
        var assembly = Assembly.GetAssembly(typeof(SceneView));
        var type = assembly.GetType("UnityEditor.LogEntries");
        var method = type.GetMethod("Clear");
        method.Invoke(new object(), null);
    }

    [MenuItem("My Utilities/Log a message")]
    public static void LogHello()
    {
        Debug.Log("Hello from my console utilties");
    }
}
```

1. After a few seconds you should now see a menu named My Utilities appear, with 2 items Clear Console and Log a message.

1. You should now be able to clear the console and generate Log messages with these menu items.


<!-- *********************** HOW it works ************************* -->

## How it works...

x

<!-- *********************** there's more ************************* -->

## There's more

There are some details that you don't want to miss.

### Keyboard shortcuts

xxx



using UnityEngine;

public class gizmoTest : MonoBehaviour
{
    public float explosionRadius = 5.0f;

    void OnDrawGizmosSelected()
    {
        // Display the explosion radius when selected
        Gizmos.color = new Color(1, 1, 0, 0.75F);
        Gizmos.DrawSphere(transform.position, explosionRadius);
    }
}




DrawGIzmoGrid - from Unity Wiki
http://wiki.unity3d.com/index.php/DrawGizmoGrid

using UnityEngine;
using System.Collections;
 
// DrawGizmoGrid.cs
// draws a useful reference grid in the editor in Unity. 
// 09/01/15 - Hayden Scott-Baron
// twitter.com/docky 
// no attribution needed, but please tell me if you like it ^_^
 
public class DrawGizmoGrid : MonoBehaviour
{
	// universal grid scale
	public float gridScale = 1f; 
 
	// extents of the grid
	public int minX = -15; 
	public int minY = -15; 
	public int maxX = 15; 
	public int maxY = 15; 
 
	// nudges the whole grid rel
	public Vector3 gridOffset = Vector3.zero; 
 
	// is this an XY or an XZ grid?
	public bool topDownGrid = true; 
 
	// choose a colour for the gizmos
	public int gizmoMajorLines = 5; 
	public Color gizmoLineColor = new Color (0.4f, 0.4f, 0.3f, 1f);  
 
	// rename + centre the gameobject upon first time dragging the script into the editor. 
	void Reset ()
	{
		if (name == "GameObject")
			name = "~~ GIZMO GRID ~~"; 
 
		transform.position = Vector3.zero; 
	}
 
	// draw the grid :) 
	void OnDrawGizmos ()
	{
		// orient to the gameobject, so you can rotate the grid independently if desired
		Gizmos.matrix = transform.localToWorldMatrix;
 
		// set colours
		Color dimColor = new Color(gizmoLineColor.r, gizmoLineColor.g, gizmoLineColor.b, 0.25f* gizmoLineColor.a); 
		Color brightColor = Color.Lerp (Color.white, gizmoLineColor, 0.75f); 
 
		// draw the horizontal lines
		for (int x = minX; x < maxX+1; x++)
		{
			// find major lines
			Gizmos.color = (x % gizmoMajorLines == 0 ? gizmoLineColor : dimColor); 
			if (x == 0)
				Gizmos.color = brightColor;
 
			Vector3 pos1 = new Vector3(x, minY, 0) * gridScale;  
			Vector3 pos2 = new Vector3(x, maxY, 0) * gridScale;  
 
			// convert to topdown/overhead units if necessary
			if (topDownGrid)
			{
				pos1 = new Vector3(pos1.x, 0, pos1.y); 
				pos2 = new Vector3(pos2.x, 0, pos2.y); 
			}
 
			Gizmos.DrawLine ((gridOffset + pos1), (gridOffset + pos2)); 
		}
 
		// draw the vertical lines
		for (int y = minY; y < maxY+1; y++)
		{
			// find major lines
			Gizmos.color = (y % gizmoMajorLines == 0 ? gizmoLineColor : dimColor); 
			if (y == 0)
				Gizmos.color = brightColor;
 
			Vector3 pos1 = new Vector3(minX, y, 0) * gridScale;  
			Vector3 pos2 = new Vector3(maxX, y, 0) * gridScale;  
 
			// convert to topdown/overhead units if necessary
			if (topDownGrid)
			{
				pos1 = new Vector3(pos1.x, 0, pos1.y); 
				pos2 = new Vector3(pos2.x, 0, pos2.y); 
			}
 
			Gizmos.DrawLine ((gridOffset + pos1), (gridOffset + pos2)); 
		}
	}
}

