# Explorer sidebar

As you can see, most of the actions in the explorer level are performed by the user using the sidebar element.

This component is located at `Content/UI/SideBar`.

The sidebar consists of multiple components, including buttons that are always displayed on the left side. These buttons come in two types: a square button with rounded corners, which represents actions within the sidebar.

Lastly the sidebar you can hide and show at will is implement inside the `WB_SideBarContent`

### Sidebar Buttons

To simplify development, the `CPP_SideBarButton` class was created in the `Source/Widgets/Buttons` folder. The main purpose of this class is to implement reusable button logic, allowing each button to be customized with different icons and border colors.

A Blueprint based on the `UCPP_SideBarButton` class was created inside the `UI/ReusableButtons` folder. Named `WB_SideBarButton`, this Blueprint provides a reusable button design.

Here are the steps to use this reusable button:

1. In the User Widget Blueprint editor of your choice, place the `WB_SideBarButton` component. By default, it includes a placeholder icon with a black border.

2. In the Details tab, specify the icon and, optionally, the border color for this button.

3. Once placed, set the button as a variable, allowing you to implement its on-click event within the User Widget component's Blueprint editor.

<figure markdown="span">
  ![Customise button option](https://jrcz-data-science-lab.github.io/VirtualAnatomy-Documentation/images/side-bar-button-change-colour-and-texture.png) <figcaption>Fields where button can be customised</figcaption>
</figure>

If you're unsure about these steps, refer to the `WB_ExplorerSideBar` Blueprint for example implementation.
