## Adding a New Model with the Picker

To add a new part of the model, follow these steps. This guide assumes you have already imported your picker mesh and skeletal mesh.

1. **Make the Merged Model a Skeletal Mesh:**  
   Refer to the [Animation]( /VirtualAnatomy-Documentation/VirtualAnatomy/ExplorerLevel/Animations/Animations/) chapter to learn how to create a Skeletal Mesh.

2. **Create a New Blueprint:**  
   Name it `BP_YourPartOfTheBody`.

3. **Place the Blueprint in the World:**  
   Drag and drop the blueprint into the world and organize it within the World Explorer window.

4. **Open the Blueprint and Navigate to the `Viewport`:**  
   Inside the blueprint, go to the `Viewport` tab.

5. **Add the Skeletal Mesh to the Scene:**  
   In the `Viewport`, you should see the `SceneRoot`. Open the Asset Explorer (`Ctrl + Space`), find your Skeletal Mesh, and drag and drop it into the `SceneRoot` component.

6. **You Should See Something Like This:**

    <figure markdown="span">
    ![bones and picker mesh](https://jrcz-data-science-lab.github.io/VirtualAnatomy-Documentation/images/adding-new-meshes-part-1.png) <figcaption>Viewport of the new body part</figcaption>
   </figure>

7. **Rename the Skeletal Mesh in the Viewport:**  
   Rename the Skeletal Mesh to match the name that will be displayed in the tree view.

8. **Assign a Descriptive Tag:**  
   Add a short, descriptive tag (e.g., bones, veins, arteries, heart, liver, stomach, etc.) in the Details panel.

9. **Add the Picker Mesh:**  
   Open the Asset Explorer again (`Ctrl + Space`) and navigate to the folder where your picker mesh is located.

10. **Drag and Drop the Picker Mesh into the Skeletal Mesh:**  
    Select all the meshes of the picker model and drag them into the Skeletal Mesh component. This ensures that the picker meshes become children of the Skeletal Mesh.

    You should now see something like this:

    <figure markdown="span">
      ![bones and picker mesh](https://jrcz-data-science-lab.github.io/VirtualAnatomy-Documentation/images/adding-new-meshes-part-2.png) <figcaption>Viewport of the new body part and its picker mesh</figcaption>
    </figure>

11. **Configure the Picker Mesh:**  
    Select all the picker meshes and perform the following actions:
    - Change the material to `M_MaterialPicker`.
    - Set the visibility to **Hidden**.
    - Set the collision preset to **Custom**.

    Your configuration should look like this:

    <figure markdown="span">
      ![bones and picker mesh](https://jrcz-data-science-lab.github.io/VirtualAnatomy-Documentation/images/adding-new-meshes-part-3.png) <figcaption>Collision presets</figcaption>
    </figure>

Once you've completed these steps, you should be able to select individual parts of the model as well as the entire skeletal mesh from the sidebar of the application.
