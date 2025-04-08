
# Welcome to DracoArts

![Logo](https://dracoarts-logo.s3.eu-north-1.amazonaws.com/DracoArts.png)




#  Mirror Unity Multiplayer Networking Example 
Mirror is a high-performance, easy-to-use networking solution built specifically for Unity, designed to streamline and simplify the development of multiplayer games. As a community-driven fork of Unity’s now-deprecated UNET (Unity Networking) system, Mirror retains the familiar API structure that made UNET accessible while addressing its limitations—delivering better performance, enhanced stability, and modern features that meet today’s multiplayer game development needs. Unlike UNET, which was officially discontinued by Unity, Mirror is actively maintained and improved by a dedicated community of developers, ensuring ongoing support, bug fixes, and optimizations.

# 🔹 What is Mirror Networking?
## Mirror is a Unity networking library that provides:

- Simple API for beginners

- Powerful features for advanced developers

- Server-authoritative architecture (prevents cheating)

- Flexible transport layers (TCP, UDP, WebSockets)

-  Automatic object synchronization (SyncVars, Commands, RPCs)

## Why Use Mirror Instead of UNET or Netcode?

- ✅ Open-source & free (MIT License)

- ✅ Better performance than UNET

- ✅ Active community & updates

- ✅ Works with modern Unity versions

 - ✅ Supports dedicated servers, P2P, and relay servers

 # 🔹 Core Concepts in Mirror

## 1. NetworkManager

- The central controller for multiplayer games.

### Handles:
- ### Player spawning

- ### Scene management

- ### Connection callbacks (when players join/leave)
## 2. NetworkIdentity

- Marks a GameObject as a networked object.

- Required for any object that needs to be synchronized across clients.

## 3. NetworkBehaviour

- A base class for networked scripts (like MonoBehaviour but for multiplayer).

- Contains networking features:

 ## [SyncVar] 
  - Synchronized variables

## [Command]

- Client → Server function calls

## [ClientRpc] 
-  Server → Client function calls

## [TargetRpc] 
- Server → Specific client function calls


## 4. SyncVars
- Automatically syncs variables from server → clients.

- Example: Player health, score, position.

- Supports hooks (functions that trigger when the value changes).

## 5. Commands ([Command])
- Client → Server function calls.

- Used for player input (e.g., movement, shooting).

- Only the local player can call commands on their own objects.

## 6. Remote Procedure Calls (RPCs)
 ####  Server → Clients ([ClientRpc])

- Broadcasts to all clients (e.g., spawning effects).

#### Server → Specific Client ([TargetRpc])

- Sends to one client (e.g., private messages).

## 7. NetworkTransform
- Automatically syncs position, rotation, and scale.

- Can be customized for smoother movement (interpolation).

## 🔹 How Mirror Networking Works
 ### 1. Server-Client Architecture
- Server (Host) – The authority that controls game logic.

- Clients – Players connected to the server.

-  Host Mode – A player acts as both server & client (like in P2P games).

### 2. Player Movement Example
- Client sends input via [Command].

- Server validates and applies movement.

- Server updates [SyncVar] position.

- Clients receive updates and render smoothly.

## 3. Anti-Cheat & Lag Compensation
- Server-authoritative movement prevents speed hacks.

- Client-side prediction makes controls feel responsive.

- Lag compensation adjusts hit detection for delayed players.


# 🔹 Mirror Networking Setup (Step-by-Step)
### 1. Installation

- Option 1: Unity Package Manager → Add from Git URL:

https://github.com/vis2k/Mirror.git

- Option 2: Download from Asset Store
  [Mirror](https://assetstore.unity.com/packages/tools/network/mirror-129321?srsltid=AfmBOooLuaHckeUba47IC9HGyue7dupkXuMtO40ZCY2CE5n-0qLB5C95)

### 2. Basic Setup

  #### 1 .Add NetworkManager to an empty GameObject.

 #### 2 .Create a Player Prefab and add:

- NetworkIdentity

- NetworkTransform (if movement needs syncing)

- A script with NetworkBehaviour

#### 3. Assign the Player Prefab in the NetworkManager.
# ✅ Advantages

✔ Easy to learn (similar to UNET)

✔ High performance (optimized for games)\

✔ Works with modern Unity (2020 LTS+)

✔ Supports WebGL & dedicated servers

✔ Active Discord community


## Usage/Examples
## 1. Setup NetworkManager
    using Mirror;
    using UnityEngine;

    public class MyNetworkManager : NetworkManager
    {
    public override void OnServerAddPlayer(NetworkConnection conn)
    {
        base.OnServerAddPlayer(conn);
        
        GameObject player = conn.identity.gameObject;
        
        player.GetComponent<PlayerController>().SetRandomColor();
       }
    }

## 2. Player Controller

    using Mirror;
    using UnityEngine;

    public class PlayerController : NetworkBehaviour
    {
    [SyncVar]
    private Color playerColor;

    private Material playerMaterial;
    
    void Start()
    {
        playerMaterial = GetComponent<Renderer>().material;
        
        if (isLocalPlayer)
        {
            // Only local player can move
            GetComponent<PlayerMovement>().enabled = true;
        }
        
        UpdateColor();
    }
    
    public void SetRandomColor()
    {
        if (isServer)
        {
            playerColor = new Color(
                Random.Range(0f, 1f),
                Random.Range(0f, 1f),
                Random.Range(0f, 1f)
            );
        }
    }
    
    private void UpdateColor()
    {
        playerMaterial.color = playerColor;
    }
    

    void OnPlayerColorChanged(Color oldColor, Color newColor)
    {
        UpdateColor();
     }
    }

## 3. Player Movement



    using Mirror;
    using UnityEngine;

    public class PlayerMovement : NetworkBehaviour
    {
    public float speed = 5f;
    
    void Update()
    {
        if (!isLocalPlayer) return;
        
        float moveHorizontal = Input.GetAxis("Horizontal");
        float moveVertical = Input.GetAxis("Vertical");
        
        Vector3 movement = new Vector3(moveHorizontal, 0f, moveVertical);
        transform.Translate(movement * speed * Time.deltaTime);
        
       
    }
    
  
    }

## Setup Scene:

 - Create a new empty GameObject and add the MyNetworkManager component

- Add a NetworkManagerHUD component for testing UI

- Create a player prefab with:

- NetworkIdentity component

- PlayerController component

- PlayerMovement component

- Renderer and Collider components

- Assign the player prefab in the NetworkManager
## Image


![](https://github.com/AzharKhemta/DemoClient/blob/main/Unity%20Mirror%20Multiplayer.gif?raw=true)


## Authors

- [@MirHamzaHasan](https://github.com/MirHamzaHasan)
- [@WebSite](https://mirhamzahasan.com)


## 🔗 Links

[![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/company/mir-hamza-hasan/posts/?feedView=all/)
## Documentation

[Mirror](https://mirror-networking.gitbook.io/docs)


## Tech Stack
**Client:** Unity,C#

**Plugin:** Mirror Networking



