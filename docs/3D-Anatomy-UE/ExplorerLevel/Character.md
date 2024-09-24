# Character


Here we will cover basics like how come that i can walk around the scene without actualy seing cammera. Or how is it possible that I can rotate without finding `OnUserClick` event. 

## Character

Since this is not a video game we have named or character User. 

Character is for what would you expect it to be, to walk around and interact with the level. 

Character is spawned at the player start location which can be seen in the `World explorer ` menu on the right 

![player-start-in-world-view](images/player-start.png)

To specify which Character is going to be spawned inside the level, we can open `CPP_GameMode.cpp` file and inside the contrustre we see this code 

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

This code graphs the Blueprint of our user and assignes it to the Game mode so that the Unreal Enigne knows what character to spawn and which character will be "possesed" by the player. In other words with this we tell unreal engine that our character is going to be controlled by the user behind the computer. 

But wait, why do we have BP_User if we are using only C++ ?

Well this is for the ease of development if you visit the path specified inside `FClassFinder` your can see that the parent class of this `BP_User` is the `CPP_User`.

With this approach we can simply modify various parameters as we see fit without additional hustle of C++.

Since `BP_User` is inhereted from `CPP_User` it contains all of the infomation and actions as what is written in `CPP_User`. 


Now you might be wanderin how come that I see without any camera.

This is covered in the following section. 


## Character class (CPP_User.cpp)

This class inherits `ACharacter` which means that our the player (user) is able to controll the character and move it in the scene. 

Now we will walk your throught the `CPP_User.h` file which contains the definition of the `CPP_User` class 

### `ACPP_User();`
Constructor of the class. Here we set up camera togher with the spring arm component to aid us with collisions with meshes and floor. 

The camera and spring arm are created and added to the root component of the character using following code 


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

If you visit the `BP_User` and open the view port you can see both spring arm and camera there 

>NOTE: to see the viwe port of the blue print you have to enable the `full blue print edditor`. It should promt you to do so once you double click on the BP_User

![camera and user in view port](./docs/images/camera-and-spring-arm-in-view-port.png)


## Public fiels 

----

### `AActor* TargetActor`

This is the invisible actor in the scene which is specifing the target around which our camera orbits.

This actor is moved around the model of the human body so based on which part of the body was selected. See [ActorSelector]()

----

### `USpringArmComponent* CameraBoom;`

This pointer holds the spring arm component thorught this pointer you can access it and manipulate it **whithin the class**

----

### `UCameraComponent* MainCamera;`

This is the pointer to the actual camera that is going to be used for the player (user in our case). 

Once your press the play button in Unreal Editor you will see what this camera seas. It is really taht simple 

----

###	`void BeginPlay()`

This is function that is inhereted from the `ACharacter`. It is overwritten so that we can put anything we want to happen once you press the play button in the controller.

For example if you want user to start spinning once game starts you put it there. 


Here we setup our defautl `TargetActor` and `RayCaster` since both of them require access to the `World` and `World` is not accessible during the construction time e.g. in constructor.

-----

## Protected fields 

-----

###	`void Zoom(const FInputActionValue& Value)`

This action gets exetued once the user zooms in. The `FInputActionValue` is reference to the ammount of which user used inside. This is handled through the Enhanced Input system of the Unreal Engine. 

In other words this function zooms the player either in or out in the scene based on how much he spinned the mouse whele or moved his fingers on touch pad. 

----

###	`void Look(const FInputActionValue& Value)`

Function that gets exectued once user turns around using his mouse, touchpad or touch screen. 

To calculate the rotation of the acctor the function takes position of the actor at which we want to look at.

It substracts the player (user) position from the position of the `Target Actor` to get the direction pointing towards that target actor.

We that normalize this direction to be of the length of 1

And calculate how much should the character (user) rotate around this actor using `RotationDelta` once this is calculated we construct rotation matrix to preform the acctual rotation. 

We apply this matrix on the calculated direction to get the new direction at which is our player looking at.

And lastly we calculate the new position of the player (user) using this newly calculated direction. 

----

### `void Click(const FInputActionValue& Value);`

This function is exectued once per click

The value here should represent the position of the mouse in the `world space` howver for some reson it did not work as intedet therefore we used `Unproject ` to calculate this value ourseles

This functio also preforms ray cast to determine on which actor user cliked on (if any) sets new position of the target actor to update where the camera should look at and highlight the selected mesh. 






