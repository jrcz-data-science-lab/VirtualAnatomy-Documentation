---
weight: -1
---

# Codebase overview & guidelines

This section outlines fundamental guidelines and practices essential for continuing development on this project.

## C++ code guide lines 

For class members that have macro `UPROPERTY` or `UFUNCTION` defined above them, we have used the `PascalCase`

For functions that are used inside classes we have also used `PascalCase`

For private member functions that are only accessible within the classes we have used `m_` prefix and `smallPacalCase` example

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

In C++ you can use `#include` preprocessor to include external header files. This, however, is going to take long time to compile, therefore we have used forward declaration wherever possible inside the header files and only use `#include` preprocessor inside the `.cpp` files. 

### Memory safety

The class architecture is designed around the principle of ownership. We utilize Unreal Engine’s smart pointers (e.g., TSharedPtr, TWeakObjectPtr, etc.) to ensure that each resource has a single owner, while other classes may only reference it without unnecessary copying. This approach promotes memory safety and performance

## Why using C++ over Blueprints?

The decision to use C++ as the primary development language stems from a focus on long-term maintainability. While C++ can be complex and less forgiving than Blueprints, its performance advantages and precise control over application behavior make it a preferred choice for this project.

For those unfamiliar with C++, it’s perfectly acceptable to begin prototyping in Blueprints. Often, when a Blueprint implementation becomes difficult to manage or overly complex, it’s a good indicator that it should be refactored into C++.

Take your time learning C++; it can be both powerful and challenging—but ultimately rewarding.

## Development Approaches in Unreal Engine

### Blueprints

**Advantages**
- Easy to understand
- Integrated in the editor
- Easier debugging
- No C++ knowledge required

**Disadvantages**
- Limited scalability
- Difficult to implement complex systems
- Slower performance

**Notes**
> Best for prototyping and ideal for small projects.

---

### C++

**Advantages**
- High performance
- Full control over memory
- Higher maintainability
- Future-proof

**Disadvantages**
- Steeper learning curve
- Requires IDE setup and recompilation

**Notes**
> Excellent for large systems and requires experienced developers.

---

### C++ & Blueprints (Hybrid)

**Advantages**
- Best of both worlds
- Flexibility
- High efficiency
- Logical flow

**Disadvantages**
- Complexity of combining workflows
- Requires expertise in both systems

**Notes**
> Optimal for mid- to large-scale projects.
