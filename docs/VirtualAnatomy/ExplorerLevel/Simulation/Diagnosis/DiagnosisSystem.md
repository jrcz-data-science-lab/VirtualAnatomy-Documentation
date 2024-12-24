# Diagnosis System

## Overview

**NOTE**: This system uses advanced C++ principles. It assumes familiarity with:

- **Smart Pointers** (specifically unique pointers and their behavior under single ownership).

- **Const References** (and their purpose in ensuring immutability and efficient access).

This documentation does not cover these concepts, so ensure you understand them before modifying the system.

---

The disease system is designed to simulate shock and other diseases. Currently, it supports **hypervolemic shock** but is flexible enough to include more diseases in the future. It acts as a **data interface** that defines parameters affecting the model's behavior, animation, and simulation.

---

## Design Principles

The system follows the **single ownership principle**, where:

- Only one object owns a resource.

- Access to the resource is provided through **const references**, preventing unintended modifications.

**Unique pointers** (`TUniquePtr`) are used to enforce this principle. These pointers prevent copying and ensure that the only way to access the resource is via a const reference. (**There are ways to get the raw pointer but avoid those at all cost**)

---

## Components

### `CPP_DiagnosisRegistry`
This class is the sole owner of `CPP_Diagnosis` instances. It manages their lifecycle and provides access to them as const references. No direct manipulation of `CPP_Diagnosis` objects is allowed outside the registry.

### `CPP_Diagnosis`
Represents the data for a specific disease, such as hypervolemic shock. All data is encapsulated, and instances are accessible only through const references provided by `CPP_DiagnosisRegistry`.

The access to various diagnosis is done by the following enum. Only this enum, which is selected in combo box is needed to retrieve right diagnosis parameters 


```c++
UENUM(BlueprintType)
enum class EDiagnosisType: uint8
{
	Healthy = 0,
	HyperVolumetricShock,
    //others...
};
```
---

# Class diagram 

<figure markdown="span">
  ![Blood Particle System](https://jrcz-data-science-lab.github.io/VirtualAnatomy-Documentation/images/simulationManagerClassDiagram.png)
  <figcaption>User-Defined Parameters in the Niagara System Editor</figcaption>
</figure>

## Adding new diagnosis

I will guide you how to add new diagnosis. For this example I will create diagnosis which is basicaly Death. Meanign patient died which in sense is a diagnosis. 

### Add new enum

You will go to the `Types/DiagnosisTypes.h` and add new entry to the enum

```c++
UENUM(BlueprintType)
enum class EDiagnosisType: uint8
{
	Healthy = 0,
	HyperVolumetricShock,
  
  // new
  Death
};
```

### Add entry to the drop down menu 

In `CPP_DropDownMenu` you will go to the `NativeConstruct` function and add your new Diagnosis 

This ensures that user can select the Diagnosis

```c++
void UCPP_DiagnosisDropDownMenu::NativePreConstruct()
{
	Super::NativePreConstruct();

	DiseaseOptions.Empty();
	DiseaseOptions.Add("Healthy",EDiagnosisType::Healthy);
	DiseaseOptions.Add("Hypo volumetric shock",EDiagnosisType::HyperVolumetricShock);

  // new
  DiseaseOptions.Add("Death",EDiagnosisType::Death);

  //...
}
```

### Specify parameters

>We used builder pattern to create the unique pointer. Note that the builder patter returens the uniqe pointer which might look misleading but in fact it is using move semantics to transfer the owner ship.

This part will specify various parameters of of the dissease. Go to the `DiagnosisRegistry` and to the `BuildDiagnosisList()` method 

Now you can use builder to specify the parameters

```c++
void DiagnosisRegistery::BuildDiagnosisList()
{

	//================================
	// HYPER VOLUMETRIC SHOCK
	//================================
	FSimulationSlideBarsParameters hyperVolumetricShockParameters;
	hyperVolumetricShockParameters.Speed = 1;
	hyperVolumetricShockParameters.BloodThickness = 300;
	hyperVolumetricShockParameters.BeatsPerMinute = 200;
	auto newDiagnosis =
		DiagnosisBuilder.SetName("Hypervolumetric shock")
		.SetDescription("Giant loos of blood")
		.SetDiagnosisType(EDiagnosisType::HyperVolumetricShock)
		.SetSimulationParameters(hyperVolumetricShockParameters)
		.Build();
	DiagnosisList.Add(MoveTemp(newDiagnosis));


  // new
	
  //================================
	// DEATH
	//================================
  FSimulationSlideBarsParameters deathParameters;
	hyperVolumetricShockParameters.Speed = 0;
	hyperVolumetricShockParameters.BloodThickness = 0;
	hyperVolumetricShockParameters.BeatsPerMinute = 0;
	// we overwirte new diagnosis 
  newDiagnosis =
		DiagnosisBuilder.SetName("Death")
		.SetDescription("You died !")
		.SetDiagnosisType(EDiagnosisType::HyperVolumetricShock)
		.SetSimulationParameters(deathParameters)
		.Build();
    // note the MoveSemantics
	DiagnosisList.Add(MoveTemp(newDiagnosis));
}
```

And thats it the new disseases was added and simulation parameters will be changed once the user selects this dissease