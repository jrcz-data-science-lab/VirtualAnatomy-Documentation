---
weight: 1
---

# Localization

The localization feature of the application was done through Unreal Engine's integrated localization. Here we will walk you through the process and how to update localization if needed.

# Localization dashboard configuration

![Localization usage](https://jrcz-data-science-lab.github.io/VirtualAnatomy-Documentation/images/localization-dashboard.png)

First we gather text present in all game files. For this we tick the "Gather from Packages" checkbox and specify where the engine should look for text to gather. To specify, we make use of the path wildcards option and tell the engine to look through everything contained in the `Content` directory which contains all our files for the application.

After this we can add new cultures through the `Localization dashboard`. We then set the native culture for the application, which is English in this case. And finally we gather the text using the `Gather Text` button in the dashboard.

The next step is to actually translate the text we need. For this we open the `Translation editor` through the `Actions` segment of the cultures.

![Localization usage](https://jrcz-data-science-lab.github.io/VirtualAnatomy-Documentation/images/translation-actions.png)

In the translation editor we quite simply manually translate the words we need.

**Note**

It is possible to use third party services to translate, using `Portable Object` files. As these third party services are usually paid we refrained from using them, considering the scale of the application.

![Localization usage](https://jrcz-data-science-lab.github.io/VirtualAnatomy-Documentation/images/translation-editor.png)

Once we have the necessary words translated, the process is simple:

 `save > gather text > compile text`

 # Important

 For all text blocks we want to translate, we need to specify if they should be localized. For this purpose we simply open the UMG, select the text we want to localize and in the `Details` panel we check the `localize` option like so:

 ![Localization usage](https://jrcz-data-science-lab.github.io/VirtualAnatomy-Documentation/images/umg-localization.png)

 # Localization functionality

  ![Localization usage](https://jrcz-data-science-lab.github.io/VirtualAnatomy-Documentation/images/localization-functionality.png)

  We use dispatchers for the buttons since they are reusables, then we simply set the preffered culture using the `Set Current Culture` node where we specify the culture. (The `Hide All Dots` and `Set Dot Visible` nodes are for UI purposes only).

  To make sure we are getting the correct specification of the culture we can check it by hovering over the cultures in the `Localization Dashboard`:

![Localization usage](https://jrcz-data-science-lab.github.io/VirtualAnatomy-Documentation/images/culture-spec.png)




