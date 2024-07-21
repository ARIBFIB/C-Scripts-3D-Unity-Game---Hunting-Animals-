# Unity C-Sharp Scripts
![image](https://github.com/user-attachments/assets/9b4ee461-e6b8-4458-98ff-7a1dfbfeb775)# C-Scripts-3D-Unity-Game---Hunting-Animals-
Here I will provide you the all scripts which is used in unity game, developers can copy and paste in your tutrotials

# Player Movement 3D
```
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
```

# Update The Script Player Movement
## Add Gravity to the player Script and Add to the Ground Check
![image](https://github.com/user-attachments/assets/28fec217-e0df-4674-bf8c-026fe5a630ff)

![image](https://github.com/user-attachments/assets/55e295bb-083f-4d31-9e80-a18f42565683)

```
using System.Collections;
using System.Collections.Generic;
using System.Numerics;
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    [Header("Player Movement & Gravity")]
    public float movementSpeed = 5f;
    private CharacterController controller;

    public float gravity = -9.81f;
    
    public Transform groundCheck;

    public LayerMask groundMask;

    public float groundDistance = 0.4f;

    private bool isGrounded;
    
    private UnityEngine.Vector3 velocity;



    void Start()
    {
        controller = GetComponent<CharacterController>();
    }

    void Update()
    {
        isGrounded = Physics.CheckSphere(groundCheck.position, groundDistance, groundMask);

        if(isGrounded && velocity.y < 0)
        {
            velocity.y = -2f;
        }
        HandleMovement();
        HandleGravity();

        controller.Move( velocity * Time.deltaTime);
    }

    void HandleMovement(){
        float horizontalInput = Input.GetAxis("Horizontal");
        float verticalInput = Input.GetAxis("Vertical");

        UnityEngine.Vector3 movement = transform.right * horizontalInput + transform.forward * verticalInput;
        movement.y = 0;
        controller.Move(movement * movementSpeed * Time.deltaTime);

    }
    void HandleGravity(){
        velocity.y += gravity * Time.deltaTime;
    }
}

```
# Adding Jump in the game C# Script
```
// handle jump
        if(Input.GetButtonDown("Jump") && isGrounded)
        {
            velocity.y = Mathf.Sqrt(jumpForce * -2 * gravity);
        }
```
## Update full Script Jump
### Script Name: PlayerMovement
```
using System.Collections;
using System.Collections.Generic;
using System.Numerics;
using Unity.Mathematics;
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    [Header("Player Movement & Gravity")]
    public float movementSpeed = 5f;

    public float jumpForce = 2f;
    private CharacterController controller;

    public float gravity = -9.81f;
    
    public Transform groundCheck;

    public LayerMask groundMask;

    public float groundDistance = 0.4f;

    private bool isGrounded;
    
    private UnityEngine.Vector3 velocity;



    void Start()
    {
        controller = GetComponent<CharacterController>();
    }

    void Update()
    {
        isGrounded = Physics.CheckSphere(groundCheck.position, groundDistance, groundMask);

        if(isGrounded && velocity.y < 0)
        {
            velocity.y = -2f;
        }
        HandleMovement();
        HandleGravity();

        // handle jump
        if(Input.GetButtonDown("Jump") && isGrounded)
        {
            velocity.y = Mathf.Sqrt(jumpForce * -2 * gravity);
        }

        controller.Move( velocity * Time.deltaTime);
    }

    void HandleMovement(){
        float horizontalInput = Input.GetAxis("Horizontal");
        float verticalInput = Input.GetAxis("Vertical");

        UnityEngine.Vector3 movement = transform.right * horizontalInput + transform.forward * verticalInput;
        movement.y = 0;
        controller.Move(movement * movementSpeed * Time.deltaTime);

    }
    void HandleGravity(){
        velocity.y += gravity * Time.deltaTime;
    }
}

```


# Add Foot Step Audio
## Adding the foot step audio when the player move
## Updated Code
```
using System;
using System.Collections;
using System.Collections.Generic;
using System.Numerics;
using Unity.Mathematics;
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    [Header("Player Movement & Gravity")]
    public float movementSpeed = 5f;

    public float jumpForce = 1f;
    private CharacterController controller;

    public float gravity = -9.81f;
    
    public Transform groundCheck;

    public LayerMask groundMask;

    public float groundDistance = 0.4f;

    private bool isGrounded;
    
    private UnityEngine.Vector3 velocity;

    [Header("Audio Foot Steps")]
    public AudioSource leftFootAudioScource;
    public AudioSource rightFootAudioScource;
    public AudioClip[] footstepSounds;
    public float footstepInterval = 0.5f;

    public float nextFootstepTime;

    private bool isLeftFootstep = true;



    void Start()
    {
        controller = GetComponent<CharacterController>();
    }

    void Update()
    {
        isGrounded = Physics.CheckSphere(groundCheck.position, groundDistance, groundMask);

        if(isGrounded && velocity.y < 0)
        {
            velocity.y = -2f;
        }
        HandleMovement();
        HandleGravity();

        //handle Footsteps
        if(isGrounded && controller.velocity.magnitude > 0.1f && Time.time >= nextFootstepTime)
        {
            PlayerFootstepSound();
            nextFootstepTime = Time.time + footstepInterval;
        }

        // handle jump
        if(Input.GetButtonDown("Jump") && isGrounded)
        {
            velocity.y = Mathf.Sqrt(jumpForce * -2 * gravity);
        }

        controller.Move( velocity * Time.deltaTime);
    }

    void HandleMovement(){
        float horizontalInput = Input.GetAxis("Horizontal");
        float verticalInput = Input.GetAxis("Vertical");

        UnityEngine.Vector3 movement = transform.right * horizontalInput + transform.forward * verticalInput;
        movement.y = 0;
        controller.Move(movement * movementSpeed * Time.deltaTime);

    }
    void HandleGravity(){
        velocity.y += gravity * Time.deltaTime;
    }

    void PlayerFootstepSound(){
        AudioClip footstepClip = footstepSounds[UnityEngine.Random.Range(0,footstepSounds.Length)];

        if(isLeftFootstep)
        {
            leftFootAudioScource.PlayOneShot(footstepClip);
        }
        else
        {
            rightFootAudioScource.PlayOneShot(footstepClip);
        }

        isLeftFootstep = !isLeftFootstep;
    }

    
}
```
# Add Camera Controller
## Adding the Camera Controller when the player move the mouse the camera also move
## Updated Code
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CameraController : MonoBehaviour
{
    public Transform playerTransform;
    public float sensitivity = 2f;
    public float minXAngle = -30f;
    public float maxXAngle = 30f;
    public float minYAngle = -360f;
    public float maxYAngle = 360f;
    public float rotationX = 0f;
    public float rotationY = 0f;
    public float smoothSpeed = 10f;

    void Start()
    {
        Cursor.lockState = CursorLockMode.Locked;
        Cursor.visible = false;
    }
    void Update()
    {
        float mouseX = Input.GetAxis("Mouse Y") * sensitivity;
        float mouseY = Input.GetAxis("Mouse X") * sensitivity;

        rotationX -= mouseX;
        rotationY += mouseY;

        rotationX = Mathf.Clamp(rotationX, minXAngle, maxXAngle);
        rotationY = Mathf.Clamp(rotationY, minYAngle, maxYAngle);

        Quaternion targetRotation = Quaternion.Euler(rotationX, rotationY, 0);

        transform.rotation = Quaternion.Slerp(transform.rotation, targetRotation, smoothSpeed * Time.deltaTime);
        


    }
}
```
