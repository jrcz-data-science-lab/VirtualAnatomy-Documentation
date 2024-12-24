# Blood simulation

**NOTE**: this section will assume that you are at least a bit familiar with inner workings of the application and niagara system

## General idea

This section will discuss how the blood is created and how it was designed to account for various parameters and user selection.

To begin, idea of blood in this project is simple. The blood is defined as a particle system called Niagara. there are countless reasources online that can show you how to work with it. In essence you can define various parameters of the particle system, for example, particles should have speeed, velocity and gravity you put it together by simply cliking + button in visual niagar edditor. UE will do all of the magic for you and you will end up with working particle simulation, that can collide with enviroment.

The core functionality is that we have a circulatory system (Defined in __BP_CirculatorySystem__) that contains:
- picker mesh for veins 
- picker mesh for arteries
- set of splines that represetn arteries and veins
- rendering mesh for arteries
- rendering mesh for veins 

For us only set of splines are interesting. To make the particle follow the spline we have to somehow retrieve it and than pass it to the Niagara system (particles) so that they know what path to take. We pass the spline by simply assigning niagara particle as a child of that spline. See depiction below: 

```
RootComponent
 -> SplineComponent
    -> NiagaraSystemComponent
```

For niagara system to recognize this, we have to set up **User defined parameter** with type Spline. 

<figure markdown="span">
  ![user parameters ](https://jrcz-data-science-lab.github.io/VirtualAnatomy-Documentation/images/user-params-niagara.png) <figcaption>User defined parameters in niagara system edditor</figcaption>
</figure>

## User parameters 

With the help of this parameters we are not only able to pass a splines that the blood should follow but also change the behaviour of the Niagara system at runtime. This allowed us to for example change the speed of the blood particles. 

## Making particles follow spline 

This is rather simple, we defined that simulation should be altered using the custom blueprint. This blueprint is taking the age of the particle and moving it along the spline based on the age. The age was choosen since it increases which can be used to move the particle along the curve. 

This can be seen in the figure below 


<figure markdown="span">
  ![user parameters ](https://jrcz-data-science-lab.github.io/VirtualAnatomy-Documentation/images/blood-particle-system.png) <figcaption>User defined parameters in niagara system edditor</figcaption>
</figure>

# Spawning particles 

This is not as straight forward as one might expect. To spawn the particles we have to define a spline that can be retrieved from `BP_CirculatorySystem`. Due to constrains of unreal engine it is impossible to share components between different actors. That is suppose we have actor that is responsible for managing the particle system, and another actor that contains the set of splines. In order to not duplicate the splines we one might thing that we can simple pass it as a argument to some `Init` function (since constructors can not accept parameters in UE). 

Well, the problem is when we pass the spline component as an argument for some currently unknown resons it will get nulled and new spline will be created from scratch, which will result in a straing line.

To account for this we had to duplicate the spline using C++ and copy the point data from the provided spline and apply it to the new one. With this set up the splines that were defined in `BP_CirculatroySystem` can be passed to the niagara system. 


This implementation show above discribed problem in action 
```c++
void ACPP_BloodPathSystem::Init(UCPP_SimulationManager* SimManager, USplineComponent* Spline,
	UNiagaraSystem* Blood)
{
	// call init of the base class
	ACPP_BloodParticleSystemBase::Init(SimManager,Blood);

	if (Spline)
	{

		// Since we can not pass diferent components between actors we have to recreate the componenent manualy
		TArray<FVector> SplinePoints;
		for (int i = 0; i<Spline->GetNumberOfSplinePoints(); i++)
		{
			SplinePoints.Push(Spline->GetSplinePointAt(i, ESplineCoordinateSpace::World).Position);
		}

		SplineToFollow->SetSplinePoints(SplinePoints, ESplineCoordinateSpace::World);
		SetActorScale3D(FVector(2.0f, 2.0f, 2.0f));

		BloodNiagaraSystem = Blood;

		BloodParticleComponent = UNiagaraFunctionLibrary::SpawnSystemAttached(BloodNiagaraSystem,SplineToFollow, NAME_None, FVector(0.f), FRotator(0.f),
															  EAttachLocation::Type::KeepRelativeOffset, true, false);

```