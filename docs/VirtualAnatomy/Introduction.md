# Basic Information for Project Development

This part of the documentation will contain basic information one needs to know in order to carry on with the project.

## Purpose 

The purpose of the Unreal Engine application is to do what it does best: displaying 3D models with interactive frame rates.

It consists of two main levels: **Explorer Level** and **Quiz Level**. In the Explorer Level, teachers and students can freely explore a fully loaded 3D model of the human body.

In the Quiz Level, teachers and students—primarily students—can complete quizzes that are created by the teachers.

## Setting Up 

To begin development of this application, you need [Unreal Engine](https://www.unrealengine.com/en-US) version 5.4, which can be found in the [Epic Games Launcher](https://store.epicgames.com/en-US/download).

### Windows

For Windows, you also need [Visual Studio](https://visualstudio.microsoft.com) with the following workload items added:

- Game Development with C++
- .NET SDK (can be installed by enabling .NET desktop development)

Once you have everything set up, you can launch the project by opening the `ThreeDAnatomy.uproject` file with Unreal Engine. 

To add the necessary files to open the project with Visual Studio, run `Tools > Generate Visual Studio Project`. Then, open the project in Visual Studio by opening the generated `ThreeDAnatomy.sln` file.

### Mac OS

----------------

For Mac OS install [Rider](https://www.jetbrains.com/rider/), and launch the project by opening the `ThreeDAnatomy.uproject` with Rider.

### Linux

-----------------

For how to set up project up and running on Linux please reffer to [this file](/VirtualAnatomy/Linux-set-up) 


### C++ code guide lines 

For class members that have macro `UPROPERTY` or `UFUNCTION` defined above them we have used the `PascalCase`

For functions that are used inside classes we have also used `PascalCase`

For private member functions that are only accesible within the classes we have used `m_` prefix and `smallPacalCase` example


```c++
class FooBar{

public:
    Foo(std::string name);

public:
    UPROPERTY(BlueprintReadOnly)
    AActor* actor

    UFUNCTION(BlueprintCallable)
    void DisplayFoo(bool& success);

private:
    int m_countOfFoos;
}

```

### Class names

Class names are specified with `PascalCase`


### Forward declaration of headers 

In `C++` you can use `#include` prepocessor to include external header files. This however is going to take long time to compile, therefore we have used forward declaration wherever possible inside the header files and only use `#include` preprocessor inside the `cpp` files. 


### Memory safety

We have designed the class strucutre around `Ownership` priciple. To do that we have use `smart pointers` that are defined by unreal engine. This assures that only given class can "own" the resource and others can only borrow it without additionaly coppies. 

