---
weight: -1
---

# Raycasting

## How is raycasting done?

!!! Note

    This class cannot be seen inside the Unreal Editor as it does not inherit any of the classes provided by Unreal Engine. Its main purpose is to abstract away the ray casting logic.

As mentioned, we have chosen ray casting as the primary method for selecting which objects the user clicks on.

The way it works is that when the user clicks on the screen, we receive coordinates about where the mouse was clicked (in screen space). We then transform this mouse location to world space and calculate the direction from our character to the point where the mouse clicked. This direction is normalized, meaning it has a length of 1.
After that, we specify a ray length parameter to determine where the ray should end and then cast the ray from the character’s position towards the calculated endpoint.

Previously, the system used basic ray-object collision detection techniques, but we have now implemented the Möller-Trumbore algorithm for more precise intersection testing with triangles in the scene. This algorithm allows efficient ray-triangle intersection detection, making it particularly useful for accurate selection of 3D models and surfaces.

All of these calculations are performed once the user presses the left mouse button.

As you might imagine, all of this is happening in the `Click` event inside the `CPP_User` class.

The main responsibility of the `FRayCaster` class is to trace the actual rays through the scene.
