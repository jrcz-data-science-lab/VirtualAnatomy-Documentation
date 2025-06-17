---
weight: -1
---

# Character

Here, we will cover the basics, like how you can walk around the scene without actually seeing the camera, or how it is possible that you can rotate without finding the `OnUserClick` event.

Since this is not a video game, we have named our character `User`.

The character is what you would expect it to be: to walk around and interact with Explorer level.

The character is spawned at the player start location, which can be seen in the `World Explorer` menu on the right side of the Unreal Engine.

![player-start-in-world-view](https://jrcz-data-science-lab.github.io/VirtualAnatomy-Documentation/images/player-start.png)

To specify which character is going to be spawned inside the level, we can open the `CPP_GameMode.cpp` file, and inside the constructor, you can find this piece of code:


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

However, you might ask why we have `BP_User` blueprint if we are using only C++?

Well, this is for the ease of development. If you visit the path specified inside `FClassFinder`, you can see that the parent class of this `BP_User` is the `CPP_User`.

With this approach, we can simply modify various parameters as we see fit without the additional hassle of C++.

Since `BP_User` is inherited from `CPP_User`, it contains all the information and actions as what is written in `CPP_User`.

Now you might be wondering how you can see without any camera.

This is covered inside the [ACPP_User](CPP_User.md) page, in `ACPP_User()` explanation.