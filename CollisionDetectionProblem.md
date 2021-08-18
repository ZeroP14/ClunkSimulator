When I gave Rigidbody to my objects (light and heavy object) I had a problem.
First, at high speed these objects could go through walls, I didn't what that. 
It was easy to solve, I just changed `CollisionDetectionMode` in the inspector to `Continuous`.
Second, objects still could go through each other. The solution, as the Internet told me, to change `CollisionDetectionMode` to `ContinuousDynamic` for one of my objects. 
Well, as you might guess it didn't work. One object could go through another one and walls as well, the one with `ContinuousDynamic` mode.
After some searching and thinking I created the code for that.
Look, if you'll read the documentation it says: 
>For best results, set this value to CollisionDetectionMode.ContinuousDynamic for fast moving objects, and for other objects which these need to collide with, set it to CollisionDetectionMode.Continuous.
So, the fast moving object should have `Continuous`, and the slow one should have `CollisionDetectionMode`. You know where I'm going, right?
Here we can create a simple code for that:
``` C#
//Creating the variables for objects and rigidbodies for them

//Compare the velocity variables between two objects

//Changing the CollisionDetectionMode

//PS sometimes at the start the object with the velocity could go through another, so I made a default state when the velocities of the objects are the same.
```
