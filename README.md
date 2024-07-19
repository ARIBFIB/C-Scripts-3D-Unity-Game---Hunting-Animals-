![image](https://github.com/user-attachments/assets/9b4ee461-e6b8-4458-98ff-7a1dfbfeb775)# C-Scripts-3D-Unity-Game---Hunting-Animals-
Here I will provide you the all scripts which is used in unity game, developers can copy and paste in your tutrotials

![image](https://github.com/user-attachments/assets/5433e343-650c-410a-961c-ed14c1bf2f7b)
````
C-Sharp Scripts
using System.Collections;
using System.Collections.Generic;
using System.Numerics;
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    [Header("Player Movement & Gravity")]
    public float movementSpeed = 5f;
    private CharacterController controller;
    


    void Start()
    {
        controller = GetComponent<CharacterController>();
    }

    void Update()
    {
        HandleMovement();
    }

    void HandleMovement(){
        float horizontalInput = Input.GetAxis("Horizontal");
        float verticalInput = Input.GetAxis("Vertical");

        UnityEngine.Vector3 movement = transform.right * horizontalInput + transform.forward * verticalInput;
        movement.y = 0;
        controller.Move(movement * movementSpeed * Time.deltaTime);

    }
}
````
