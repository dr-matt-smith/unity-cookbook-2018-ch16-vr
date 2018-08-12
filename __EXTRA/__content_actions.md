
 public class GUITextField : IGUI {
     public string text = "";
     public GUIContent label = new GUIContent();

     // Unused in my example, but you may want to check if
     // a textbox becomes empty for example.
     public event System.Action<string> TextChanged;

     public void OnGUI() {
         // Also I wanted to show you BeginChangeCheck and EndChangeCheck
         // which is the Unity GUI way of checking if a GUI control changed...
         EditorGUI.BeginChangeCheck ();
         text = EditorGUILayout.TextField (label, text);
         if (EditorGUI.EndChangeCheck () && TextChanged != null)
             TextChanged (text);
     }
 }

public class GUIButton : IGUI {
     public GUIContent label = new GUIContent();
     public event System.Action Clicked;

     public void OnGUI() {
         if (GUILayout.Button (label) && Clicked != null)
             Clicked ();
     }
}



