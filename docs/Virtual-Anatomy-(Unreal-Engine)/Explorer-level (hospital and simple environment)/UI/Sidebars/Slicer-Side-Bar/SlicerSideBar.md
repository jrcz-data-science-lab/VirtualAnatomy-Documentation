# Slicer Side Bar

The slicer and slicer sidebar in the Virtual Anatomy project work together to provide users with an intuitive way to interact with and manipulate a slicing plane within the 3D anatomy environment. The slicer itself is an in-world actor responsible for rendering and managing the slicing of anatomical models along a customizable plane. This plane can be moved and rotated in real time, enabling users to explore cross-sections of the body from different angles and depths.

The slicer sidebar is a user interface widget built with UMG that gives users control over the slicer’s behavior. When the sidebar becomes visible, it triggers the slicer to display its slicing plane in the 3D space. Conversely, when the sidebar is hidden, the slicer plane is also hidden to keep the interface clean and focused.

The sidebar provides a checkbox to enable or disable the slicer entirely, sliders to fine-tune the position and rotation of the slicer plane, and several buttons that allow for quick application of predefined distance and rotation values. These controls send commands to the slicer actor in the scene, updating its transform parameters accordingly.

During initialization, the sidebar checks for the presence of the slicer actor and associated widgets. If any required component is missing, it sets an internal error state, which can be queried to diagnose configuration issues. Additionally, the sidebar loads any previously saved slicer parameters from the game instance so that the interface reflects the last-used values, offering a consistent user experience across sessions.

Overall, the slicer and its sidebar work in tandem to give users precise and flexible control over anatomical visualization, blending interactive 3D manipulation with a clean and accessible interface.