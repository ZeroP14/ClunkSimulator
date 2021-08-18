There might be some problems which are caused by rigidbody. I'll give you the best example I can. Imagine, you want to create a game where 2 objects (each with a rigidbody) needs to move at the high speed.

First problem, at high speed, these objects can go through walls, we don't what that. 
It is easy to solve, you need to change `CollisionDetectionMode` in the inspector to `Continuous`.
Here is an image of where you need to make these changes:


![Снимок экрана (27)](https://user-images.githubusercontent.com/88005081/129928470-3ccae899-bc4a-4984-8821-2f81bca5c407.png)


Second problem, objects still can go through each other. The solution, as the documentation says, to change `CollisionDetectionMode` to `ContinuousDynamic` for one of the objects. 

Well, as you might guess it didn't work. One object could go through another one and walls as well, the one with `ContinuousDynamic` mode.
After some searching and thinking we might come up with the code for that.
Read the documentation for better understanding: 
>For best results, set this value to CollisionDetectionMode.ContinuousDynamic for fast moving objects, and for other objects which these need to collide with, set it to CollisionDetectionMode.Continuous.

So, the fastest object should have `Continuous`, and the slowest one should have `CollisionDetectionMode`. You know where I'm going, right?
We can create a simple code for that:
``` C#
using UnityEngine;

public class ObjectsScript : MonoBehaviour
{
    //Creating the variables and rigidbodies for objects
    
    [SerializeField] private GameObject heavyObject;
    [SerializeField] private GameObject lightObject;

    private Rigidbody heavyRb;
    private Rigidbody lightRb;

    void Start()
    {
        heavyRb = heavyObject.GetComponent<Rigidbody>();
        lightRb = lightObject.GetComponent<Rigidbody>();
    }
    private void FixedUpdate()
    {
        ChangeCollisionMode();
    }

    private void ChangeCollisionMode()
    {
        //Compare the velocity variables between two objects
        //Changing the CollisionDetectionMode
        if(lightRb.velocity.x > heavyRb.velocity.x)
        {
            lightRb.collisionDetectionMode = CollisionDetectionMode.Continuous;
            heavyRb.collisionDetectionMode = CollisionDetectionMode.ContinuousDynamic;
        }
        else if (lightRb.velocity.x < heavyRb.velocity.x)
        {
            lightRb.collisionDetectionMode = CollisionDetectionMode.ContinuousDynamic;
            heavyRb.collisionDetectionMode = CollisionDetectionMode.Continuous;
        }
        else 
        {
            //This happens when the velocities of the objects are the same, and at the very start of the game.
            //Sometimes, at the very start, the object with the velocity can go through another, so I made a default state
            //I do not know what causes this problem, but it might be that somehow Unity cannot see the velocity differents
            lightRb.collisionDetectionMode = CollisionDetectionMode.ContinuousDynamic;
            heavyRb.collisionDetectionMode = CollisionDetectionMode.Continuous;
        }
    }
}
```
