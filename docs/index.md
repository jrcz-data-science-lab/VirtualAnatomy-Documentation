# Documentation intro

!!! Warning
    
    The documentation is undergoing significant development and modifications, and this will continue until at most July 6, 2025. As a result, it may be difficult to follow its content at the moment. Apologies for any inconvenience.

Welcome to the documentation for the 3D-Anatomy-Project built by [JRCZ Data Science Lab](https://jrcz.nl/en/data-science-lab.html)

## General overview

The project itself is divided into several repositories:

- 3D Unreal Engine application [GitHub](https://github.com/jrcz-data-science-lab/Unreal-Engine-3D-Anatomy)
    - This project is the meat of the whole thing, it contains the Unreal Engine project that can be loaded into UE5.

- Quiz, Django web application [GitHub](https://github.com/jrcz-data-science-lab/digital_anatomy_quiz)
    - Quiz creation web application, teachers are creating quizzes here that are retrieved in the UE application.

- Frontend for model tagging, Django web application [GitHub](https://github.com/jrcz-data-science-lab/anatomy_web)
    - Since different years of the nursing study need to see different parts of the anatomy, teachers can configure this through this website.

- Python script for importing the Blender models to the Unreal Engine [GitHub](https://github.com/jrcz-data-science-lab/blender-scripts)
- Python script for generating JSON files that can be loaded in the Model tagging application [GitHub](https://github.com/jrcz-data-science-lab/Anatomy-blender-auto-prepare)     
- This documentation [GitHub](https://github.com/jrcz-data-science-lab/3D-Anatomy-Documentation)

## How to Approach This Giant Project

This project is large and can feel overwhelming to grasp all at once. We’ve tried to make as many components as stable and unchanging as possible. This means that most of what you see on the screen doesn’t require a deep understanding right away—working code is best left untouched, right? 😄

For the first 2–3 weeks of your internship, we strongly encourage you to focus on learning C++, especially general OOP concepts and how pointers work. Try building a simple calculator or a rock-paper-scissors app using OOP and pointers to get comfortable.

Next, dive into general Unreal Engine tutorials. Remember, everything in Unreal Engine is highly decoupled—it’s not like a website or other applications where you can easily call everything through dependency injections or setters. We also recommend following some C++ tutorials in Unreal Engine and creating a small project to test things out. Once you get comfortable with the C++ side, using Blueprints will feel like a helpful tool for tying your logic together.

Keep in mind that C++, OOP, and other programming patterns aren’t quite the same when developing in Unreal Engine. Also, documentation can be extremely frustrating to navigate. Because of this, we strongly advise using JetBrains Rider IDE if your RAM allows it, as it provides the best autocompletion and function documentation support. If it doesn’t, try increasing your swap file to use your SSD as temporary RAM.

While this documentation isn’t perfect (we only had six months to develop, test, design, and write it!), we’ve tried to document as much as possible to help guide you through.

## Why We Used mostly C++ Instead of Blueprints

In short, for maintainability. Yes, C++ can be hard, complicated, and unforgiving, but once you become familiar with its inner workings, it will become both your best friend and, sometimes, your worst enemy. If you’re new to C++, take it slowly. If you’re not sure how to implement something, try building it in Blueprints first, even if it turns into a litteral of spaghetti code. When you reach that point, it’s usually a good time to start rewriting it in C++.

## Structure of this documentation

This documentation is mainly focused on the Unreal Engine application but in the future, we plan to expand it to other projects as well.

Keep in mind that we tried to make this cover as much as possible but as you will soon understand, it is quite difficult to handle all the HZ shenanigans, develop new features, and document it. :)
