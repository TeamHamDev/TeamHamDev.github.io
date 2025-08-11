---
title: "Quick-Start Guide"
permalink: /docs/quick-start-guide/
toc: true
page_nav: false
---

This guide provides a basic, step-by-step tutorial to get your first flying agent moving with the Smart Flying Navigation plugin.

## 1. Creating the Agent

1.  Create a new `Blueprint Actor` in your Content Browser. Let's name it `BP_FlyingAgent`.
2.  Open the Blueprint editor for `BP_FlyingAgent`.
3.  Add a `Static Mesh` component so you can see the agent in the world. Assign any mesh you like.
4.  Add the `PathfindingComponent` to the actor.

## 2. Setting Up the Scene

1.  Drag an instance of `BP_FlyingAgent` into your level.
2.  Create a target for the agent to move to. For example, you can place an empty Actor in the level and assign it to the `TargetActor` property of your `BP_FlyingAgent` in the Details panel.
3.  Ensure you have a `Voxel Octree Volume` in your scene that covers the area where the agent will move.

## 3. Making it Move

With the `TargetActor` set, the `PathfindingComponent` will automatically begin to find a path and move the agent towards it when you start the game.

Press **Play** in the editor. You should see your `BP_FlyingAgent` navigate towards the target actor.

This is a very basic setup. For more advanced options, please refer to the **[Class Reference](/docs/component-reference/)**.