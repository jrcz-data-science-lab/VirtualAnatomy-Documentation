# Character

Here we will cover the basics, like how I can walk around the scene without actually seeing the camera, or how it is possible that I can rotate without finding the `OnUserClick` event.


## Character

Since this is not a video game, we have named our character "User."

The character is what you would expect it to be: to walk around and interact with the level.

The character is spawned at the player start location, which can be seen in the `World Explorer` menu on the right.


![player-start-in-world-view](https://jrcz-data-science-lab.github.io/VirtualAnatomy-Documentation/images/player-start.png)

To specify which character is going to be spawned inside the level, we can open the `CPP_GameMode.cpp` file, and inside the constructor, we see this code:


```c++
static ConstructorHelpers::FClassFinder<APawn> PlayerPawnBP(TEXT("/Game/Blueprints/UserControls/BP_User"));

	if(PlayerPawnBP.Class != NULL)
	{
		DefaultPawnClass = 	PlayerPawnBP.Class;
		UE_LOG(LogTemp, Warning, TEXT("UserBP was set as default pawn class"));
	}else{
		UE_LOG(LogTemp, Error, TEXT("UserBP was not found"));
	}
```

This code grabs the Blueprint of our user and assigns it to the Game Mode so that the Unreal Engine knows what character to spawn and which character will be "possessed" by the player. In other words, with this, we tell Unreal Engine that our character is going to be controlled by the user behind the computer.

But wait, why do we have `BP_User` if we are using only C++?

Well, this is for the ease of development. If you visit the path specified inside `FClassFinder`, you can see that the parent class of this `BP_User` is the `CPP_User`.

With this approach, we can simply modify various parameters as we see fit without the additional hassle of C++.

Since `BP_User` is inherited from `CPP_User`, it contains all of the information and actions as what is written in `CPP_User`.

Now you might be wondering how I can see without any camera.

This is covered in the following section.



## Character class (CPP_User.cpp)
This class inherits from `ACharacter`, which means that our player (user) is able to control the character and move it in the scene.

Now we will walk you through the `CPP_User.h` file, which contains the definition of the `CPP_User` class.

### `ACPP_User();`

Constructor of the class. Here we set up the camera together with the spring arm component to aid us with collisions with meshes and the floor.

The camera and spring arm are created and added to the root component of the character using the following code:



```c++
    //for spring arm it is named camera boom as most of the Unreal Engine comunty gives this name to it 
    CameraBoom = CreateDefaultSubobject<USpringArmComponent>(TEXT("Camera Boom"));
    
    //add to the root of the player 
    CameraBoom->SetupAttachment(RootComponent);
```

The camera is created in similuar manner and is added as a **child** of `CameraBoom`

```c++
    MainCamera =  CreateDefaultSubobject<UCameraComponent>(TEXT("Main camera"));
    MainCamera->SetupAttachment(CameraBoom,USpringArmComponent::SocketName);
```

If you visit the `BP_User` and open the viewport, you can see both the spring arm and camera there.

> **NOTE:** To see the viewport of the blueprint, you have to enable the **full blueprint editor**. It should prompt you to do so once you double-click on the `BP_User`.

![camera and user in view port](https://jrcz-data-science-lab.github.io/VirtualAnatomy-Documentation/images/camera-and-spring-arm-in-view-port.png)


## Public fiels 

----

### `AActor* TargetActor`

> `	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Target Actor",meta = (AllowPrivateAccess = "true"))`

This is the invisible actor in the scene which specifies the target around which our camera orbits.

This actor is moved around the model of the human body based on which part of the body was selected. See [MeshSelector](../ExplorerLevel/MeshSelection/MeshSelector.md) for more details.

----

### `USpringArmComponent* CameraBoom;`
>`	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = Camera)`


This pointer holds the spring arm component. Through this pointer, you can access it and manipulate it **within the class**.

----

### `UCameraComponent* MainCamera;`
>`	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = Camera)`

This is the pointer to the actual camera that is going to be used for the player (user, in our case).

Once you press the play button in the Unreal Editor, you will see what this camera sees. It is really that simple.

----

###	`void BeginPlay()`

This is a function that is inherited from the `ACharacter`. It is overridden so that we can put anything we want to happen once you press the play button in the controller.

For example, if you want the user to start spinning once the game starts, you would put it there.

Here we set up our default `TargetActor` and `RayCaster`, since both of them require access to the `World`, and the `World` is not accessible during construction time (e.g., in the constructor).

-----

### `void Tick(float DeltaTime);`

**Parameters:**

- `float DeltaTime:` - time between ticks, this parameter is provided by the Unreal Engine 


This is a function that is executed every `tick`. The tick frequency is usually every frame.

The parameter `float DeltaTime` is the time in milliseconds between two tick functions.

We use this function to store the delta time in order to make user movement frame rate independent.

----

## Protected fields 

-----

###	`void Zoom(const FInputActionValue& Value)`

**Parameters:**

`float FInputActionValue& Value:` - reference to the how much user has value passed by the enhanced input system 

This action gets executed once the user zooms in. The `FInputActionValue` is a reference to the amount that the user adjusted. This is handled through the Enhanced Input system of Unreal Engine.

In other words, this function zooms the player either in or out in the scene based on how much they spun the mouse wheel or moved their fingers on the touchpad.

----

###	`void Look(const FInputActionValue& Value)`

**Parameters:**

`float FInputActionValue& Value:` - reference to the how much user has value passed by the enhanced input system 


This function gets executed once the user turns around using their mouse, touchpad, or touch screen.

To calculate the rotation of the actor, the function takes the position of the actor that we want to look at.

It subtracts the player (user) position from the position of the `Target Actor` to get the direction pointing towards that target actor.

We then normalize this direction to be of length 1.

Next, we calculate how much the character (user) should rotate around this actor using `RotationDelta`. Once this is calculated, we construct a rotation matrix to perform the actual rotation.

We apply this matrix to the calculated direction to get the new direction that the player is looking at.

Lastly, we calculate the new position of the player (user) using this newly calculated direction.


----

### `void Click(const FInputActionValue& Value);`

**Parameters:**

`float FInputActionValue& Value:` - reference to the how much user has value passed by the enhanced input system 

This function is executed once per click.

The value here should represent the position of the mouse in the `world space`; however, for some reason, it did not work as intended, so we used `Unproject` to calculate this value ourselves.

This function also performs a ray cast to determine which actor the user clicked on (if any), sets the new position of the target actor to update where the camera should look, and highlights the selected mesh.

----

### `void SetupPlayerInputComponent(class UInputComponent* PlayerInputComponent)`

**Parameters:**

`UInputComponent* PlayerInputComponent` - The input component to configure. This component holds the mappings of input actions and axes to function calls.

This member function is inherited from the `ACharacter` class, and its main purpose is to tell Unreal Engine what functions should be executed once the user executes inputs specified by the Enhanced Input system.

In other words, this function specifies that once the user moves the mouse in an arbitrary direction, the function `Look` will be executed. Similarly, when the user presses the right mouse button, the function `Click` will be executed.

----

## Protected member fields

----


### `UInputMappingContext* InputMapping;`
>`UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "EnhancedInput", meta = (AllowPrivateAccess = "true"))`

This is a variable that is configured in `BP_User` (inherited from `CPP_User`) and specifies what input mappings are going to be used for our user.

This means it maps the keys that the user presses to the actions that are going to be executed. Refer to [this video](https://www.youtube.com/watch?v=O-3PNiTHlE0) for more details.


----


### `UInputAction* IA_Rotate, IA_Zoom, IA_Click`
>`	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category= "EnhancedInput")`

These are the input actions that allow us to specify what actions our character can execute. Those fields are also configured inside the `BP_User` blueprint.

The video linked above explains everything about the topic of the Unreal Engine enhanced input system.

----


### `float CameraSpeed`
>`	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = Camera `

This is a simple float variable that defines the speed of the camera. This can also be changed inside the Unreal Editor.

----


### `float ZoomSpeed`
>`	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = Camera `

This is a simple float variable that defines the speed at which the user zooms to the currently set target.

----

## Private members

----


### `float m_deltaTime`

This variable represents the time before different ticks, used to achieve frame rate independent movement.

----


### `TUniquePtr<RayCaster> m_rayCaster`
This is a unique pointer for the ray caster class. The ray caster is used to determine which objects the user has clicked on.

We have chosen to go with ray casting instead of pixel picking, as pixel picking requires another scene pass, which can be quite expensive given the complexity of the model.

We use a unique pointer to enforce ownership and prevent memory leaks. See [this](https://baemincheon.github.io/2020/03/14/unreal-unique-pointer/) blog post.

----


### `TUniquePtr<MeshSelector> m_meshSelector`

Holds unique pointer to the `Mesh Selector` class

The main purpose of this class is to highlihgt the objects that user has clicked on. See [MeshSelectoror](../ExplorerLevel/MeshSelection/MeshSelector.md)

----


### `void RotateAroundActor(const FVector2D& LookInput)`

**Parameters:**

- `FVector2D& LookInput` - how much should camera rotate

Rotates the character around the `TargetActor` this method is being called inside the `Look` function to rotate the player

----


### `void ZoomToTheActor(const float& LookInput)`

**Parameters:**

- `const float& LookInput` - how much should user move towards the target actor

Moves user towards or backwards from the target actor based on the provided parameter.

Parameter is extracted from the Enhanced input system.

This function is called inside the `Zoom` method



