# Simulation of Blood Flow 

This section explains the architecture of the simulation and the rationale behind creating different C++ classes for inheritance in Blueprints.

## Why Simulation 

Nursing students often struggle to visualize how the human body operates. This simulation aims to bring the 3D anatomy model to life, enhancing understanding through interactive learning. By incorporating animations, we will simulate blood behavior and the oxygen transition within the human body, providing a dynamic educational tool. 

## Event Driven Design

Given that many different components need to interact and transform data, the simplest approach might seem to be directly calling functions across various parts of the simulation. However, this is not feasible in Unreal Engine without obtaining the correct actors and subsequently calling their functions, which can be inefficient.

To overcome this issue, we decided to use `delegates`, which are C++ mechanisms for dispatching events. This will enable us to signal different states of the simulation to various actors, allowing them to respond appropriately.
