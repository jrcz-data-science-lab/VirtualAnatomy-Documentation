# Blood Simulation

**NOTE**: This section assumes you are at least somewhat familiar with the inner workings of the application and the Niagara system.

## General Idea

This section discusses how the blood simulation is created and designed to account for various parameters and user selections.

The idea of blood in this project is straightforward. Blood is defined as a particle system using Niagara. There are countless resources online that demonstrate how to work with it. Essentially, you can define various parameters of the particle system (e.g., speed, velocity, and gravity). By clicking the **+** button in the visual Niagara editor, you can set these parameters, and Unreal Engine (UE) handles the rest. This results in a working particle simulation that can collide with the environment.

The core functionality relies on a circulatory system (defined in `BP_CirculatorySystem`) that includes:
- Picker mesh for veins 
- Picker mesh for arteries
- A set of splines representing arteries and veins
- Rendering mesh for arteries
- Rendering mesh for veins 

For our purposes, the set of splines is of primary interest. To make the particles follow the spline, we retrieve the spline and pass it to the Niagara system so the particles know their path. This is achieved by assigning the Niagara particle system as a child of the spline, as shown below:

```
RootComponent
 -> SplineComponent
    -> NiagaraSystemComponent
```

To make the Niagara system recognize this structure, we set up a **User-Defined Parameter** of type `Spline`.

<figure markdown="span">
  ![User Parameters](https://jrcz-data-science-lab.github.io/VirtualAnatomy-Documentation/images/user-params-niagara.png)
  <figcaption>User-Defined Parameters in the Niagara System Editor</figcaption>
</figure>

## User Parameters 

With the help of these parameters, we can not only pass splines that the blood should follow but also change the behavior of the Niagara system at runtime. For example, this allows us to modify the speed of the blood particles dynamically.

## Making Particles Follow the Spline 

This process is simple. We define that the simulation should be altered using a custom blueprint. This blueprint takes the age of the particle and moves it along the spline based on its age. Age was chosen as it increases consistently, which makes it suitable for driving the particle along the curve.

This is illustrated in the figure below:

<figure markdown="span">
  ![Blood Particle System](https://jrcz-data-science-lab.github.io/VirtualAnatomy-Documentation/images/blood-particle-system.png)
  <figcaption>User-Defined Parameters in the Niagara System Editor</figcaption>
</figure>

# Spawning Particles 

## Retrieving Splines 

Retrieving splines is not as straightforward as one might expect. To spawn particles, we need to define a spline that can be retrieved from `BP_CirculatorySystem`. Due to the constraints of Unreal Engine, it is impossible to share components between different actors. For instance, if one actor manages the particle system and another contains the splines, passing the spline directly is problematic.

When we pass the spline component as an argument, it becomes `null` for reasons currently unknown. A new spline is then created from scratch, resulting in a straight line.

To address this, we duplicate the spline using C++ and copy the point data from the original spline to the new one. This ensures the splines defined in `BP_CirculatorySystem` can be used with the Niagara system.

The implementation of this solution is shown below:

```c++
void ACPP_BloodPathSystem::Init(UCPP_SimulationManager* SimManager, USplineComponent* Spline,
	UNiagaraSystem* Blood)
{
	// Call Init of the base class
	ACPP_BloodParticleSystemBase::Init(SimManager, Blood);

	if (Spline)
	{
		// Since components cannot be shared between actors, recreate the component manually
		TArray<FVector> SplinePoints;
		for (int i = 0; i < Spline->GetNumberOfSplinePoints(); i++)
		{
			SplinePoints.Push(Spline->GetSplinePointAt(i, ESplineCoordinateSpace::World).Position);
		}

		SplineToFollow->SetSplinePoints(SplinePoints, ESplineCoordinateSpace::World);
		SetActorScale3D(FVector(2.0f, 2.0f, 2.0f));

		BloodNiagaraSystem = Blood;

		BloodParticleComponent = UNiagaraFunctionLibrary::SpawnSystemAttached(
			BloodNiagaraSystem, SplineToFollow, NAME_None, FVector(0.f), FRotator(0.f),
			EAttachLocation::KeepRelativeOffset, true, false
		);
	}
}
```

## Spawning Actors 

The particle system is represented by an actor structured as follows:

```c++
=====================================================================
 *          ACPP_BloodPathSystem
 *
 * Root # default
 * |_Spline (spline to follow, supplied by BP_CirculatorySystem)
 *   |_ NiagaraSystem (blood visualization)
 *
=====================================================================
```

This actor is not manually placed in the world. Instead, `CPP_BloodFlowSimulation` iterates over the splines it contains and creates an actor for each spline. All logic for `ACPP_BloodPathSystem` is encapsulated within the actor itself.

```c++
// BeginPlay of ACPP_ArteriesBloodFlowSimulation class

// Get all splines from skeletal mesh children
TArray<USceneComponent*> TempSplines;
ArteriesSkeletalMesh->GetChildrenComponents(false, TempSplines);
for (auto& spline : TempSplines)
{
    // Cast the spline from USceneComponent to USplineComponent
    if (auto s = Cast<USplineComponent>(spline))
    {
        // Create an actor representing blood and store it for future reference
        SpawnedBloodParticles.Add(SpawnBloodParticle(s));
        UE_LOG(LogTemp, Display, TEXT("Spline in the arteries component was found"));
    }
}
```
