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
# Update Camera Controller
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

        playerTransform.rotation = Quaternion.Slerp(transform.rotation, targetRotation, smoothSpeed * Time.deltaTime);
        transform.rotation = Quaternion.Slerp(transform.rotation, targetRotation, smoothSpeed * Time.deltaTime);
        


    }
}
```
# Shooting Controller Script and Reloading Ammo
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class ShootingController : MonoBehaviour
{
    public Transform firePoint;
    public float fireRate = 0.1f;
    public float fireRange = 10f;
    private float nextFireTime = 0f;

    public bool isAuto = false;
    public int maxAmmo = 30;

    private int currentAmmo; //when the ammo is full or your current ammo 

    public float reloadTime = 1.5f; //the time when pressing the R button the reloading time is 1.5seconds

    private bool isReloading = false; //by default the reloading the gun is not active

    void Start()
    {
        currentAmmo = maxAmmo; //start mathods update the current ammo 
    }

    void Update()
    {
        if(isReloading) // when reloading occur its retrun to reloading function will ture
            return;
        if(isAuto == true) // Press left click to fire 
        {
            if(Input.GetButton("Fire1") && Time.time >= nextFireTime)
            {
                nextFireTime = Time.time + 1f / fireRate;
                Shoot();
            }
        }
        else
        { // to do fire same functonality by pressing the button fire active and shoot function call
            if(Input.GetButtonDown("Fire1") && Time.time >= nextFireTime)
            {
                nextFireTime = Time.time + 1f / fireRate;
                Shoot();
            }
        }
        //Mannual Reload by pressing R button
        if(Input.GetKeyDown(KeyCode.R) && currentAmmo < maxAmmo)
        {
            Reload();
        }
    }

    private void Shoot(){
        // its a range where the fire can exist , can be change later to increase and decrease
        if(currentAmmo > 0)
        {
            RaycastHit hit;
            if(Physics.Raycast(firePoint.position, firePoint.forward, out hit, fireRange))
            {
                Debug.Log(hit.transform.name);
                //apply damage to animal
            }
            currentAmmo--; //decrement of ammo
        }
        else
        {
            //reload function call
            Reload();
        }
    }
    private void Reload()
    {
        if(!isReloading && currentAmmo < maxAmmo)
        {
            //reload anim
            isReloading = true;
            //play reload sound
            Invoke("FinishReloading", reloadTime);

        }
    }
    private void FinishReloading()
    {
        currentAmmo = maxAmmo;
        isReloading = false;
        //reset reload animation
    }
}

```
# Animator set and Reset the animation
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class ShootingController : MonoBehaviour
{
    public Animator animator;
    public Transform firePoint;
    public float fireRate = 0.1f;
    public float fireRange = 10f;
    private float nextFireTime = 0f;

    public bool isAuto = false;
    public int maxAmmo = 30;

    private int currentAmmo; //when the ammo is full or your current ammo 

    public float reloadTime = 1.5f; //the time when pressing the R button the reloading time is 1.5seconds

    private bool isReloading = false; //by default the reloading the gun is not active

    void Start()
    {
        currentAmmo = maxAmmo; //start mathods update the current ammo 
    }

    void Update()
    {
        if(isReloading) // when reloading occur its retrun to reloading function will ture
            return;
        if(isAuto == true) // Press left click to fire 
        {
            if(Input.GetButton("Fire1") && Time.time >= nextFireTime)
            {
                nextFireTime = Time.time + 1f / fireRate;
                Shoot();
            }
            else
            {
                animator.SetBool("Shoot", false);
            }
        }
        else
        { // to do fire same functonality by pressing the button fire active and shoot function call
            if(Input.GetButtonDown("Fire1") && Time.time >= nextFireTime)
            {
                nextFireTime = Time.time + 1f / fireRate;
                Shoot();
            }
            else
            {
                animator.SetBool("Shoot", false);
            }
        }
        //Mannual Reload by pressing R button
        if(Input.GetKeyDown(KeyCode.R) && currentAmmo < maxAmmo)
        {
            Reload();
        }
    }

    private void Shoot(){
        // its a range where the fire can exist , can be change later to increase and decrease
        if(currentAmmo > 0)
        {
            RaycastHit hit;
            if(Physics.Raycast(firePoint.position, firePoint.forward, out hit, fireRange))
            {
                Debug.Log(hit.transform.name);
                //apply damage to animal
            }

            animator.SetBool("Shoot", true);
            currentAmmo--; //decrement of ammo
        }
        else
        {
            //reload function call
            Reload();
        }
    }
    private void Reload()
    {
        if(!isReloading && currentAmmo < maxAmmo)
        {
            //reload anim
            animator.SetTrigger("Reload");

            isReloading = true;
            //play reload sound
            Invoke("FinishReloading", reloadTime);

        }
    }
    private void FinishReloading()
    {
        currentAmmo = maxAmmo;
        isReloading = false;
        //reset reload animation
        animator.ResetTrigger("Reload");
    }
}

```

# For DemoAnimation for animals not used in the game just for saving this below script
```
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

namespace BLINK
{
    public class AnimationDemo : MonoBehaviour
    {

        public enum AnimationType
        {
            Trigger,
            Bool
        }

        [System.Serializable]
        public class AnimationEntry
        {
            public string animationName;
            public AnimationType type;
        }

        public List<AnimationEntry> entries = new List<AnimationEntry>();

        public List<Animator> animators = new List<Animator>();

        public int entryIndex;
        public Text animationNameText;

        private void Update()
        {
            if (Input.GetKeyDown(KeyCode.A))
            {
                NextAnimation();
            }

            if (Input.GetKeyDown(KeyCode.R))
            {
                ReplayAnimation();
            }
        }

        public void NextAnimation()
        {
            entryIndex++;
            if (entries.Count - 1 < entryIndex) entryIndex = 0;
            animationNameText.text = entries[entryIndex].animationName;
            PlayAnimation();
        }

        public void PreviousAnimation()
        {
            entryIndex--;
            if (entryIndex < 0) entryIndex = entries.Count - 1;
            animationNameText.text = entries[entryIndex].animationName;
            PlayAnimation();
        }

        public void ReplayAnimation()
        {
            PlayAnimation();
        }

        private void ResetAllBool()
        {
            foreach (var entry in entries)
            {
                if (entry.type != AnimationType.Bool) continue;
                foreach (var animator in animators)
                {
                    animator.SetBool(entry.animationName, false);
                }
            }
        }

        private void PlayAnimation()
        {
            ResetAllBool();

            if (entries[entryIndex].type == AnimationType.Bool)
            {
                foreach (var animator in animators)
                {
                    animator.SetBool(entries[entryIndex].animationName, true);
                }
            }
            else
            {
                foreach (var animator in animators)
                {
                    animator.SetTrigger(entries[entryIndex].animationName);
                }
            }
        }
    }
}

```

# NavMesh Ai Not exatcly this script added in project
```

using UnityEngine;
using UnityEngine.AI;

public class EnemyAiTutorial : MonoBehaviour
{
    public NavMeshAgent agent;

    public Transform player;

    public LayerMask whatIsGround, whatIsPlayer;

    public float health;

    //Patroling
    public Vector3 walkPoint;
    bool walkPointSet;
    public float walkPointRange;

    //Attacking
    public float timeBetweenAttacks;
    bool alreadyAttacked;
    public GameObject projectile;

    //States
    public float sightRange, attackRange;
    public bool playerInSightRange, playerInAttackRange;

    private void Awake()
    {
        player = GameObject.Find("PlayerObj").transform;
        agent = GetComponent<NavMeshAgent>();
    }

    private void Update()
    {
        //Check for sight and attack range
        playerInSightRange = Physics.CheckSphere(transform.position, sightRange, whatIsPlayer);
        playerInAttackRange = Physics.CheckSphere(transform.position, attackRange, whatIsPlayer);

        if (!playerInSightRange && !playerInAttackRange) Patroling();
        if (playerInSightRange && !playerInAttackRange) ChasePlayer();
        if (playerInAttackRange && playerInSightRange) AttackPlayer();
    }

    private void Patroling()
    {
        if (!walkPointSet) SearchWalkPoint();

        if (walkPointSet)
            agent.SetDestination(walkPoint);

        Vector3 distanceToWalkPoint = transform.position - walkPoint;

        //Walkpoint reached
        if (distanceToWalkPoint.magnitude < 1f)
            walkPointSet = false;
    }
    private void SearchWalkPoint()
    {
        //Calculate random point in range
        float randomZ = Random.Range(-walkPointRange, walkPointRange);
        float randomX = Random.Range(-walkPointRange, walkPointRange);

        walkPoint = new Vector3(transform.position.x + randomX, transform.position.y, transform.position.z + randomZ);

        if (Physics.Raycast(walkPoint, -transform.up, 2f, whatIsGround))
            walkPointSet = true;
    }

    private void ChasePlayer()
    {
        agent.SetDestination(player.position);
    }

    private void AttackPlayer()
    {
        //Make sure enemy doesn't move
        agent.SetDestination(transform.position);

        transform.LookAt(player);

        if (!alreadyAttacked)
        {
            ///Attack code here
            Rigidbody rb = Instantiate(projectile, transform.position, Quaternion.identity).GetComponent<Rigidbody>();
            rb.AddForce(transform.forward * 32f, ForceMode.Impulse);
            rb.AddForce(transform.up * 8f, ForceMode.Impulse);
            ///End of attack code

            alreadyAttacked = true;
            Invoke(nameof(ResetAttack), timeBetweenAttacks);
        }
    }
    private void ResetAttack()
    {
        alreadyAttacked = false;
    }

    public void TakeDamage(int damage)
    {
        health -= damage;

        if (health <= 0) Invoke(nameof(DestroyEnemy), 0.5f);
    }
    private void DestroyEnemy()
    {
        Destroy(gameObject);
    }

    private void OnDrawGizmosSelected()
    {
        Gizmos.color = Color.red;
        Gizmos.DrawWireSphere(transform.position, attackRange);
        Gizmos.color = Color.yellow;
        Gizmos.DrawWireSphere(transform.position, sightRange);
    }
}

```
Animation Demos, Mot this script added in the project
```
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

namespace BLINK
{
    public class AnimationDemo : MonoBehaviour
    {

        public enum AnimationType
        {
            Trigger,
            Bool
        }

        [System.Serializable]
        public class AnimationEntry
        {
            public string animationName;
            public AnimationType type;
        }

        public List<AnimationEntry> entries = new List<AnimationEntry>();

        public List<Animator> animators = new List<Animator>();

        public int entryIndex;
        public Text animationNameText;

        private void Update()
        {
            if (Input.GetKeyDown(KeyCode.A))
            {
                NextAnimation();
            }

            if (Input.GetKeyDown(KeyCode.R))
            {
                ReplayAnimation();
            }
        }

        public void NextAnimation()
        {
            entryIndex++;
            if (entries.Count - 1 < entryIndex) entryIndex = 0;
            animationNameText.text = entries[entryIndex].animationName;
            PlayAnimation();
        }

        public void PreviousAnimation()
        {
            entryIndex--;
            if (entryIndex < 0) entryIndex = entries.Count - 1;
            animationNameText.text = entries[entryIndex].animationName;
            PlayAnimation();
        }

        public void ReplayAnimation()
        {
            PlayAnimation();
        }

        private void ResetAllBool()
        {
            foreach (var entry in entries)
            {
                if (entry.type != AnimationType.Bool) continue;
                foreach (var animator in animators)
                {
                    animator.SetBool(entry.animationName, false);
                }
            }
        }

        private void PlayAnimation()
        {
            ResetAllBool();

            if (entries[entryIndex].type == AnimationType.Bool)
            {
                foreach (var animator in animators)
                {
                    animator.SetBool(entries[entryIndex].animationName, true);
                }
            }
            else
            {
                foreach (var animator in animators)
                {
                    animator.SetTrigger(entries[entryIndex].animationName);
                }
            }
        }
    }
}

```
# Animal01_Ai Script :Working Perfactly, In this Script the animal move from one place to another place by adding the animator and the NevMesh and NevAgent
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.AI;

public class AnimalsAi : MonoBehaviour
{
    [Header("Bear Attributes")]
    public float health;

    [Header("Patrolling")]
    public Transform[] waypoints;
    public float waitTime = 2f;
    private int currentWaypointIndex = 0;
    private bool isWaiting = false;
    private bool walkPointSet = false;

    [Header("Chasing and Attacking")]
    public Transform player; // Assign the player Transform in the Inspector
    public float sightRange, attackRange;
    public LayerMask whatIsGround, whatIsPlayer;
    public float timeBetweenAttacks;
    private bool alreadyAttacked = false;

    public NavMeshAgent agent;
    public Animator animator;
    private bool playerInSightRange, playerInAttackRange;

    private void Awake()
    {
        // Try to find the player object automatically if not assigned in the Inspector
        if (player == null)
        {
            GameObject playerObj = GameObject.Find("Player");
            if (playerObj != null)
            {
                player = playerObj.transform;
            }
            else
            {
                Debug.LogError("Player object not found. Please assign the player Transform in the Inspector.");
            }
        }

        agent = GetComponent<NavMeshAgent>();
        animator = GetComponent<Animator>();
    }

    private void Start()
    {
        MoveToNextWaypoint();
    }

    private void Update()
    {
        // Check for sight and attack range
        playerInSightRange = Physics.CheckSphere(transform.position, sightRange, whatIsPlayer);
        playerInAttackRange = Physics.CheckSphere(transform.position, attackRange, whatIsPlayer);

        if (!playerInSightRange && !playerInAttackRange)
        {
            Patroling();
        }
        if (playerInSightRange && !playerInAttackRange)
        {
            ChasePlayer();
        }
        if (playerInAttackRange && playerInSightRange)
        {
            AttackPlayer();
        }

        if (!isWaiting && agent.remainingDistance < agent.stoppingDistance)
        {
            StartCoroutine(WaitBeforeMoving());
        }
    }

    private void MoveToNextWaypoint()
    {
        if (waypoints.Length == 0)
            return;

        agent.SetDestination(waypoints[currentWaypointIndex].position);
        animator.SetBool("isMoving", true);
        animator.Play("Bear_WalkForward");

        currentWaypointIndex = (currentWaypointIndex + 1) % waypoints.Length;
    }

    private IEnumerator WaitBeforeMoving()
    {
        isWaiting = true;
        animator.SetBool("isMoving", false);
        animator.Play("Bear_Idle");

        yield return new WaitForSeconds(waitTime);

        MoveToNextWaypoint();
        isWaiting = false;
    }

    private void Patroling()
    {
        if (!walkPointSet) SearchWalkPoint();

        if (walkPointSet)
            agent.SetDestination(waypoints[currentWaypointIndex].position);

        Vector3 distanceToWalkPoint = transform.position - waypoints[currentWaypointIndex].position;

        // Walkpoint reached
        if (distanceToWalkPoint.magnitude < 1f)
        {
            walkPointSet = false;
            MoveToNextWaypoint();
        }
    }

    private void SearchWalkPoint()
    {
        walkPointSet = true;
    }

    private void ChasePlayer()
    {
        agent.SetDestination(player.position);
        animator.SetBool("isMoving", true);
        animator.Play("Bear_RunForward");
    }

    private void AttackPlayer()
    {
        // Make sure bear doesn't move
        agent.SetDestination(transform.position);
        animator.SetBool("isMoving", false);

        transform.LookAt(player);

        if (!alreadyAttacked)
        {
            // Play a random attack animation
            int attackIndex = Random.Range(1, 4); // Assuming you have Bear_Attack1, Bear_Attack2, Bear_Attack3
            animator.Play("Bear_Attack" + attackIndex);

            // Implement melee attack to player here (e.g., apply damage)

            alreadyAttacked = true;
            Invoke(nameof(ResetAttack), timeBetweenAttacks);
        }
    }

    private void ResetAttack()
    {
        alreadyAttacked = false;
    }

    public void TakeDamage(int damage)
    {
        health -= damage;
        animator.Play("Bear_GetHitFromFront");

        if (health <= 0) Invoke(nameof(DestroyBear), 0.5f);
    }

    private void DestroyBear()
    {
        animator.Play("Bear_Death");
        Destroy(gameObject, 2f); // Delay to allow death animation to play
    }
}

```

# Updated Script Animal Moving
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.AI;

public class AnimalsAi : MonoBehaviour
{
    [Header("Bear Attributes")]
    public float health;

    [Header("Patrolling")]
    public Transform[] waypoints;
    public float waitTime = 2f;
    private int currentWaypointIndex = 0;
    private bool isWaiting = false;
    private bool walkPointSet = false;

    [Header("Chasing and Attacking")]
    public Transform player; // Assign the player Transform in the Inspector
    public float sightRange, attackRange;
    public LayerMask whatIsGround, whatIsPlayer;
    public float timeBetweenAttacks;
    private bool alreadyAttacked = false;

    public NavMeshAgent agent;
    public Animator animator;
    private bool playerInSightRange, playerInAttackRange;

    private void Awake()
    {
        // Try to find the player object automatically if not assigned in the Inspector
        if (player == null)
        {
            GameObject playerObj = GameObject.Find("Player");
            if (playerObj != null)
            {
                player = playerObj.transform;
            }
            else
            {
                Debug.LogError("Player object not found. Please assign the player Transform in the Inspector.");
            }
        }

        agent = GetComponent<NavMeshAgent>();
        animator = GetComponent<Animator>();
    }

    private void Start()
    {
        MoveToNextWaypoint();
    }

    private void Update()
    {
        // Check for sight and attack range
        playerInSightRange = Physics.CheckSphere(transform.position, sightRange, whatIsPlayer);
        playerInAttackRange = Physics.CheckSphere(transform.position, attackRange, whatIsPlayer);

        if (!playerInSightRange && !playerInAttackRange)
        {
            Patroling();
        }
        if (playerInSightRange && !playerInAttackRange)
        {
            ChasePlayer();
        }
        if (playerInAttackRange && playerInSightRange)
        {
            AttackPlayer();
        }

        if (!isWaiting && agent.remainingDistance < agent.stoppingDistance)
        {
            StartCoroutine(WaitBeforeMoving());
        }
    }

    private void MoveToNextWaypoint()
    {
        if (waypoints.Length == 0)
            return;

        agent.SetDestination(waypoints[currentWaypointIndex].position);
        animator.SetBool("isMoving", true);
        animator.Play("Bear_WalkForward");

        currentWaypointIndex = (currentWaypointIndex + 1) % waypoints.Length;
    }

    private IEnumerator WaitBeforeMoving()
    {
        isWaiting = true;
        animator.SetBool("isMoving", false);
        animator.Play("Bear_Idle");

        yield return new WaitForSeconds(waitTime);

        MoveToNextWaypoint();
        isWaiting = false;
    }

    private void Patroling()
    {
        if (!walkPointSet) SearchWalkPoint();

        if (walkPointSet)
            agent.SetDestination(waypoints[currentWaypointIndex].position);

        Vector3 distanceToWalkPoint = transform.position - waypoints[currentWaypointIndex].position;

        // Walkpoint reached
        if (distanceToWalkPoint.magnitude < 1f)
        {
            walkPointSet = false;
            MoveToNextWaypoint();
        }
    }

    private void SearchWalkPoint()
    {
        walkPointSet = true;
    }

    private void ChasePlayer()
    {
        agent.SetDestination(player.position);
        animator.SetBool("isMoving", true);
        animator.Play("Bear_RunForward");
    }

    private void AttackPlayer()
    {
        // Make sure bear doesn't move
        agent.SetDestination(transform.position);
        animator.SetBool("isMoving", false);

        transform.LookAt(player);

        if (!alreadyAttacked)
        {
            // Play a random attack animation
            int attackIndex = Random.Range(1, 4); // Assuming you have Bear_Attack1, Bear_Attack2, Bear_Attack3
            animator.Play("Bear_Attack" + attackIndex);

            // Implement melee attack to player here (e.g., apply damage)

            alreadyAttacked = true;
            Invoke(nameof(ResetAttack), timeBetweenAttacks);
        }
    }

    private void ResetAttack()
    {
        alreadyAttacked = false;
    }

    public void TakeDamage(int damage)
    {
        health -= damage;
        animator.Play("Bear_GetHitFromFront");

        if (health <= 0) Invoke(nameof(DestroyBear), 0.5f);
    }

    private void DestroyBear()
    {
        animator.Play("Bear_Death");
        Destroy(gameObject, 2f); // Delay to allow death animation to play
    }

}

```
# Update Script
# Adding the Animator and Animation - Script is Working and Genune
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.AI;

public class AnimalsAi : MonoBehaviour
{
    [Header("Bear Attributes")]
    public float health;

    [Header("Patrolling")]

    public Transform[] waypoints; 
    
    public float waitTime = 2f;
    

    private int currentWaypointIndex = 0;

    private bool isWaiting = false;

    private bool walkPointSet = false;


    [Header("Chasing and Attacking")]

    public Transform player; // Assign the player Transform in the Inspector
    public float sightRange, attackRange;

    public LayerMask whatIsGround, whatIsPlayer;
    public float timeBetweenAttacks;


    private bool alreadyAttacked = false;



    public NavMeshAgent agent;
    public Animator animator;

    public float chaseSpeed = 8f; // add this in video i will tell you i will add this later on but just 
    // write above variable

    


    private bool playerInSightRange, playerInAttackRange;


    private void Awake()
    {
        // Try to find the player object automatically if not assigned in the Inspector
        if (player == null)
        {
            GameObject playerObj = GameObject.Find("Player");
            if (playerObj != null)
            {
                player = playerObj.transform;
            }
            else
            {
                Debug.LogError("Player object not found. Please assign the player Transform in the Inspector.");
            }
        }

        agent = GetComponent<NavMeshAgent>();
        animator = GetComponent<Animator>();
    }



    private void Start()
    {
        MoveToNextWaypoint();
    }


    private void Update()
    {
        // Check for sight and attack range
        playerInSightRange = Physics.CheckSphere(transform.position, sightRange, whatIsPlayer);
        playerInAttackRange = Physics.CheckSphere(transform.position, attackRange, whatIsPlayer);

        if (!playerInSightRange && !playerInAttackRange)
        {
            Patroling();
        }
        if (playerInSightRange && !playerInAttackRange)
        {
            ChasePlayer();
        }
        if (playerInAttackRange && playerInSightRange)
        {
            AttackPlayer();
        }

        if (!isWaiting && agent.remainingDistance < agent.stoppingDistance)
        {
            StartCoroutine(WaitBeforeMoving());
        }
    }

    private void MoveToNextWaypoint()
    {
        if (waypoints.Length == 0)
            return;

        agent.SetDestination(waypoints[currentWaypointIndex].position);
        animator.SetBool("isMoving", true);
        animator.Play("Walk Forward");

        currentWaypointIndex = (currentWaypointIndex + 1) % waypoints.Length;
    }


    private IEnumerator WaitBeforeMoving()
    {
        isWaiting = true;
        animator.SetBool("isMoving", false);
        animator.Play("Idle");

        yield return new WaitForSeconds(waitTime);

        MoveToNextWaypoint();
        isWaiting = false;
    }


    private void Patroling()
    {
        if (!walkPointSet) SearchWalkPoint();

        if (walkPointSet)
            agent.SetDestination(waypoints[currentWaypointIndex].position);

        Vector3 distanceToWalkPoint = transform.position - waypoints[currentWaypointIndex].position;

        // Walkpoint reached
        if (distanceToWalkPoint.magnitude < 1f)
        {
            walkPointSet = false;
            MoveToNextWaypoint();
        }
    }

    private void SearchWalkPoint()
    {
        walkPointSet = true;
    }



    private void ChasePlayer()
    {
        agent.speed = chaseSpeed; //add in video
        agent.SetDestination(player.position);
        animator.SetBool("isMoving", true);
        animator.Play("RunForward");
    }

    private void AttackPlayer()
    {
        // Make sure bear doesn't move
        agent.SetDestination(transform.position);
        animator.SetBool("isMoving", true);

        transform.LookAt(player);

        if (!alreadyAttacked)
        {
            // Play a random attack animation
            int attackIndex = Random.Range(1, 4); // Assuming you have Bear_Attack1, Bear_Attack2, Bear_Attack3
            animator.Play("Attack" + attackIndex);

            // Implement melee attack to player here (e.g., apply damage)

            alreadyAttacked = true;
            Invoke(nameof(ResetAttack), timeBetweenAttacks);
        }
    }
    private void ResetAttack()
    {
        alreadyAttacked = false;
    }


    public void TakeDamage(int damage)
    {
        health -= damage;
        animator.Play("Bear_GetHitFromFront");

        if (health <= 0) Invoke(nameof(DestroyBear), 0.5f);
    }

    private void DestroyBear()
    {
        animator.Play("Death");
        Destroy(gameObject, 2f); // Delay to allow death animation to play
    }

}
```
![image](https://github.com/user-attachments/assets/16366a49-386a-469c-b2fa-ff842ea3bfa3)

![image](https://github.com/user-attachments/assets/39c10ab0-b689-4b16-8857-99b19e3a4fa1)

![image](https://github.com/user-attachments/assets/d1b9b84b-9be9-4255-9237-e172a960a130)

![image](https://github.com/user-attachments/assets/39dda12a-eec3-4893-a00c-142c0ba472d7)

![image](https://github.com/user-attachments/assets/a4ce8325-47a1-44a5-a70d-398e064290eb)

# Update Scripts
Player Movements - Adding Player Animation while Moving, while Running ,Remove Idle and Learning About Scripts
```
using System;
using System.Collections;
using System.Collections.Generic;
using System.Numerics;
using Unity.Mathematics;
using Unity.VisualScripting;
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    [Header("Player Movement & Gravity")]
    public float movementSpeed = 5f;

    public float movementFastSpeed = 12f; //Add this line


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

    public float currentSpeed; // you can also create a varaible outside 

    [Header("Player Animation of Movement")]
    public Animator animator;

    private bool isWalking = false; // by defalut the walking of player is not active
    private bool isRunning = false; // to track if the player is running



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

    private void HandleMovement()
    {
        float horizontalInput = Input.GetAxis("Horizontal");
        float verticalInput = Input.GetAxis("Vertical");
        UnityEngine.Vector3 movement = transform.right * horizontalInput + transform.forward * verticalInput;

        // Determine current speed based on whether the player is running
        if (Input.GetKey(KeyCode.LeftShift))
        {
            currentSpeed = movementFastSpeed;
            isRunning = true;
            isWalking = false;
        }
        else
        {
            currentSpeed = movementSpeed;
            isRunning = false;
            isWalking = true;
        }

        // Set the appropriate animations
        if(movement.magnitude > 0.1f) // Player is moving
        {
            if(isRunning)
            {
                animator.SetTrigger("Run");
                animator.ResetTrigger("Walk");
            }
            else
            {
                animator.SetTrigger("Walk");
                animator.ResetTrigger("Run");
                isWalking = true;
            }
        }
        else // Player is idle
        {
            animator.ResetTrigger("Walk");
            animator.ResetTrigger("Run");
            isWalking = false;
            isRunning = false;
        }

        // Calculate movement direction and apply movement
        movement.y = 0;
        controller.Move(movement * currentSpeed * Time.deltaTime);
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
# Shooting Projectile in FPS - Shooting Bullets
### Following are some screen shots then after that their are source code
![Screenshot 2024-08-14 031501](https://github.com/user-attachments/assets/3d43590c-9200-4adc-9d73-97a1f51303b5)

![Screenshot 2024-08-14 034127](https://github.com/user-attachments/assets/2eabea0b-a63d-4668-8406-1d85d0c4c95a)
![Screenshot 2024-08-14 034257](https://github.com/user-attachments/assets/20670288-91f8-4f36-828f-035c4d53b367)

![Screenshot 2024-08-14 034331](https://github.com/user-attachments/assets/21dee69d-a190-49d4-94aa-61bfdca388d4)


![Screenshot 2024-08-14 034727](https://github.com/user-attachments/assets/7b65c2ef-5208-44b8-a4bb-91aee9f8940a)

# Source Code - Projectile - Update Function call the shootProjectile Function();
```
if(Input.GetButton("Fire1") && Time.time >= nextFireTime)
    {
        nextFireTime = Time.time + 1f / fireRate;
        Shoot();
        ShootProjectile();
    }
```
# Then Add this function in shooting projectile Script
```
 void ShootProjectile()
    {
        Ray ray = cam.ViewportPointToRay(new Vector3(0.5f,0.5f,0));
        RaycastHit hit;

        if(Physics.Raycast(ray, out hit))
        {
            destination = hit.point;
        }
        else
        {
            destination = ray.GetPoint(1000);
        }
        
        if(leftHand)
        {
            leftHand = false;
            InstantiateProjectile(LHFirePoint);
        }
        else
        {
            leftHand = true;
            InstantiateProjectile(RHFirePoint);
        }

    }
    void InstantiateProjectile(Transform firePoint)
    {
        var projectileObj = Instantiate (projectile, firePoint.position, Quaternion.identity) as GameObject;
        projectileObj.GetComponent<Rigidbody>().velocity = (destination - firePoint.position).normalized * projectileSpeed;
    }

```
# Adding Projectile Script in Uniy - ShootingController.cs - (Concept)
```
    [Header("Mouse Aim Shooting")]

    public Camera cam;
    public GameObject projectile;
    public Transform LHFirePoint, RHFirePoint;

    public float projectileSpeed = 30;
    private Vector3  destination;
    private bool leftHand;


void Update()
{
    if(isReloading) // when reloading occur its retrun to reloading function will ture
            return;
        if(isAuto == true) // Press left click to fire 
        {
            if(Input.GetButton("Fire1") && Time.time >= nextFireTime)
            {
                nextFireTime = Time.time + 1f / fireRate;
                Shoot();
----------------ShootProjectile();
            }
            else
            {
                animator.SetBool("Shoot", false);
            }

        }
        else
        { // to do fire same functonality by pressing the button fire active and shoot function call
            if(Input.GetButtonDown("Fire1") && Time.time >= nextFireTime)
            {
                nextFireTime = Time.time + 1f / fireRate;
                Shoot();
----------------ShootProjectile();
            }
            else
            {
                animator.SetBool("Shoot", false);
            }
        }
            //Mannual Reload by pressing R button
        if(Input.GetKeyDown(KeyCode.R) && currentAmmo < maxAmmo)
        {
            Reload();
        }
}
// Add below function
void ShootProjectile()
    {
        Ray ray = cam.ViewportPointToRay(new Vector3(0.5f,0.5f,0));
        RaycastHit hit;

        if(Physics.Raycast(ray, out hit))
        {
            destination = hit.point;
        }
        else
        {
            destination = ray.GetPoint(1000);
        }
        
        if(leftHand)
        {
            leftHand = false;
            InstantiateProjectile(LHFirePoint);
        }
        else
        {
            leftHand = true;
            InstantiateProjectile(RHFirePoint);
        }

    }
    void InstantiateProjectile(Transform firePoint)
    {
        var projectileObj = Instantiate (projectile, firePoint.position, Quaternion.identity) as GameObject;
        projectileObj.GetComponent<Rigidbody>().velocity = (destination - firePoint.position).normalized * projectileSpeed;
    }


```
# Add the Script in Unity - BulletProjectile.cs - This Script is Invalid but if you understand you can use it by your own risk :) hhahahaah jusk joking. (:
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class bulletProjectile : MonoBehaviour
{
    [SerializeField] private Transform vfxHitGreen;
    [SerializeField] private Transform vfxHitRed;

    private Rigidbody bulletRigidbody;

    private void Awake() {
        bulletRigidbody = GetComponent<Rigidbody>();
    }    

    private void Start() {
        float Speed = 10f;
        bulletRigidbody.velocity = transform.forward * Speed;
    }

    private void OnTriggerEnter(Collider other) {
        if(other.GetComponent<BulletTarget>() != null)
        {
            //Hit Target
            Instantiate(vfxHitGreen ,transform.position , Quaternion.identity);
        }
        else
        {
            //Hit something else
            Instantiate(vfxHitRed ,transform.position , Quaternion.identity);
        }
        Destroy(gameObject);
    }

}

```
# Updated Full Code - ShootingController.cs
```
using System;
using System.Collections;
using System.Collections.Generic;
using Unity.VisualScripting;
using UnityEngine;

public class ShootingController : MonoBehaviour
{
    public Animator animator;
    public Transform firePoint;
    public float fireRate = 0.1f;
    public float fireRange = 10f;
    private float nextFireTime = 0f;

    public bool isAuto = false;
    public int maxAmmo = 30;

    private int currentAmmo; //when the ammo is full or your current ammo 

    public float reloadTime = 1.5f; //the time when pressing the R button the reloading time is 1.5seconds

    private bool isReloading = false; //by default the reloading the gun is not active

    public ParticleSystem muzzleFlash;

    [Header("Sound Effects")]
    public AudioSource soundAudioSource;
    public AudioClip shootingSoundClip;
    public AudioClip reloadSoundClip;

    [Header("Mouse Aim Shooting")]

    public Camera cam;
    public GameObject projectile;
    public Transform LHFirePoint, RHFirePoint;

    public float projectileSpeed = 30;
    private Vector3  destination;
    private bool leftHand;


    // [SerializeField] private LayerMask aimColliderLayerMask = new LayerMask();
    // [SerializeField] private Transform debugTransform;



    void Start()
    {
        currentAmmo = maxAmmo; //start mathods update the current ammo 
    }

    void Update()
    {
        if(isReloading) // when reloading occur its retrun to reloading function will ture
            return;
        if(isAuto == true) // Press left click to fire 
        {
            if(Input.GetButton("Fire1") && Time.time >= nextFireTime)
            {
                nextFireTime = Time.time + 1f / fireRate;
                Shoot();
                ShootProjectile();
            }
            else
            {
                animator.SetBool("Shoot", false);
            }

        }
        else
        { // to do fire same functonality by pressing the button fire active and shoot function call
            if(Input.GetButtonDown("Fire1") && Time.time >= nextFireTime)
            {
                nextFireTime = Time.time + 1f / fireRate;
                Shoot();
                ShootProjectile();
            }
            else
            {
                animator.SetBool("Shoot", false);
            }
        }
        //Mannual Reload by pressing R button
        if(Input.GetKeyDown(KeyCode.R) && currentAmmo < maxAmmo)
        {
            Reload();
        }
        // // for mouse when the mouse moves our curser also emit the light
        // Vector2 sceenCenterPoint = new Vector2(Screen.width / 2f, Screen.height / 2f);
        // Ray ray = Camera.main.ScreenPointToRay(sceenCenterPoint);
        // if(Physics.Raycast(ray, out RaycastHit raycastHit, 999f , aimColliderLayerMask))
        // {
        //     debugTransform.position = raycastHit.point;
        // }
    }

    void ShootProjectile()
    {
        Ray ray = cam.ViewportPointToRay(new Vector3(0.5f,0.5f,0));
        RaycastHit hit;

        if(Physics.Raycast(ray, out hit))
        {
            destination = hit.point;
        }
        else
        {
            destination = ray.GetPoint(1000);
        }
        
        if(leftHand)
        {
            leftHand = false;
            InstantiateProjectile(LHFirePoint);
        }
        else
        {
            leftHand = true;
            InstantiateProjectile(RHFirePoint);
        }

    }

    void InstantiateProjectile(Transform firePoint)
    {
        var projectileObj = Instantiate (projectile, firePoint.position, Quaternion.identity) as GameObject;
        projectileObj.GetComponent<Rigidbody>().velocity = (destination - firePoint.position).normalized * projectileSpeed;
    }

    private void Shoot(){
        // its a range where the fire can exist , can be change later to increase and decrease
        if(currentAmmo > 0)
        {
            RaycastHit hit;
            if(Physics.Raycast(firePoint.position, firePoint.forward, out hit, fireRange))
            {
                Debug.Log(hit.transform.name);
                //apply damage to animal
            }
            //add this below line code
            muzzleFlash.Play();
            animator.SetBool("Shoot", true);
            currentAmmo--; //decrement of ammo

            soundAudioSource.PlayOneShot(shootingSoundClip);

        }
        else
        {
            //reload function call
            Reload();
        }
    }
    private void Reload()
    {
        if(!isReloading && currentAmmo < maxAmmo)
        {
            //reload anim
            animator.SetTrigger("Reload");

            isReloading = true;
            //play reload sound
            soundAudioSource.PlayOneShot(reloadSoundClip);
            Invoke("FinishReloading", reloadTime);

        }
    }
    private void FinishReloading()
    {
        currentAmmo = maxAmmo;
        isReloading = false;
        //reset reload animation
        animator.ResetTrigger("Reload");
    }
}

```
# Updated Script - PlayerMovement.cs
```
using System;
using System.Collections;
using System.Collections.Generic;
using System.Numerics;
using Unity.Mathematics;
using Unity.VisualScripting;
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    [Header("Player Movement & Gravity")]
    public float movementSpeed = 5f;

    public float movementFastSpeed = 12f; //Add this line


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

    public float currentSpeed; // you can also create a varaible outside 

    [Header("Player Animation of Movement")]
    public Animator animator;

    private bool isWalking = false; // by defalut the walking of player is not active
    private bool isRunning = false; // to track if the player is running

    [Header("Mouse Aim Shooting")]
    [SerializeField] private LayerMask aimColliderLayerMask;
    [SerializeField] private Transform debugTransform;

    // [Header("Shooting bullet")]
    // [SerializeField] private Transform pfBullet;
    // [SerializeField] private Transform spawnBulletPosition;

    private Rigidbody bulletRigidbody;




    void Start()
    {
        controller = GetComponent<CharacterController>();

    }

    private void Awake() {
        bulletRigidbody = GetComponent<Rigidbody>();
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

        // for mouse when the mouse moves our curser also emit the light
        // Mouse Aim Handling
        UnityEngine.Vector2 screenCenterPoint = new UnityEngine.Vector2(Screen.width / 2f, Screen.height / 2f);
        Ray ray = Camera.main.ScreenPointToRay(screenCenterPoint);
        if (Physics.Raycast(ray, out RaycastHit raycastHit, 999f, aimColliderLayerMask))
        {
            debugTransform.position = raycastHit.point;
        }

        controller.Move(velocity * Time.deltaTime);

        
    }

    private void HandleMovement()
    {
        float horizontalInput = Input.GetAxis("Horizontal");
        float verticalInput = Input.GetAxis("Vertical");
        UnityEngine.Vector3 movement = transform.right * horizontalInput + transform.forward * verticalInput;

        // Determine current speed based on whether the player is running
        if (Input.GetKey(KeyCode.LeftShift))
        {
            currentSpeed = movementFastSpeed;
            isRunning = true;
            isWalking = false;
        }
        else
        {
            currentSpeed = movementSpeed;
            isRunning = false;
            isWalking = true;
        }

        // Set the appropriate animations
        if(movement.magnitude > 0.1f) // Player is moving
        {
            if(isRunning)
            {
                animator.SetTrigger("Run");
                animator.ResetTrigger("Walk");
            }
            else
            {
                animator.SetTrigger("Walk");
                animator.ResetTrigger("Run");
                isWalking = true;
            }
        }
        else // Player is idle
        {
            animator.ResetTrigger("Walk");
            animator.ResetTrigger("Run");
            isWalking = false;
            isRunning = false;
        }

        // Calculate movement direction and apply movement
        movement.y = 0;
        controller.Move(movement * currentSpeed * Time.deltaTime);
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

    // private void ShootBullet()
    // {
    //     // Instantiate the bullet at the spawn position
    //     Transform bulletInstance = Instantiate(pfBullet, spawnBulletPosition.position, spawnBulletPosition.rotation);

    //     // Get the Rigidbody component of the bullet
    //     Rigidbody bulletRigidbody = bulletInstance.GetComponent<Rigidbody>();

    //     // Apply force to the bullet
    //     float bulletSpeed = 20f; // You can set this speed to any value you want
    //     bulletRigidbody.velocity = spawnBulletPosition.forward * bulletSpeed;
    // }

    // private void OnTriggerEnter(Collider other)
    // {
    //     Destroy(gameObject);
    // }
    

    
}

```

hello my name is abdul rehman and i am the developer of this unity game project so i wanted to say i have to complete this project as soon as possible that is my mission so if you want to do any commnet related to me so you can do by direct message me or by via email. So what you are waiting for go and just do it, duw na pia tu phir kia jiya , life is too short so dont waste on irrelent thing! got my point! .Manta hu phir! :) Manna pahra ga. just joking
