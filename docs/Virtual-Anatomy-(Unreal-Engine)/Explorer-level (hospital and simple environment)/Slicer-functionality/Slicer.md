---
weight: -1
---
# Slicer

The slicer is a tool within the Virtual Anatomy that is designed to reveal cross-sections of 3D anatomical models by cutting through them visually. 
It helps explore internal structures by adjusting the slicing plane’s position and orientation. 
The slicing effect is achieved through a material setup that uses real-time parameters such as the plane’s location, direction, and activation state.

To support this functionality, the class [ACPP_Slicer](ACPP_Slicer.md) provides the logic that drives the slicer. 
It handles updates to the slicing plane’s transform, communicates with the material system through a Material Parameter Collection (MPC), 
and saves or restores slicer settings like position, rotation, and enabled state via [UCPP_GameInstance]().

This code logic is applied through the Blueprint `BP_Slicer`, which is placed in explorer levels where slicing functionality is required. 
`BP_Slicer` already includes the necessary Plane and SpringArm components preconfigured for use and those are not created inside the code.

!!! Warning

    If BP_Slicer is not present in one of the Explorer levels or `ACPP_Slicer` throws any error, the slicer functionality will be disabled and unavailable. 
    This means slicing-related controls in the UI will disappear, and materials relying on slicer parameters will not display any slicing behavior.