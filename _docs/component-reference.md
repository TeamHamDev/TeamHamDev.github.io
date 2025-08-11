---
title: "Class Reference"
layout: single
permalink: /docs/component-reference/
toc: true
toc_sticky: true
toc_label: "Contents"
page_nav: false
---

This document details the core classes of the Smart Flying Navigation plugin.

## Actors

### OctreeVoxelVolume
Description...

## Components

### PathfindingComponent
<p>The core component responsible for calculating and following paths for a single agent.</p>

<h4>Core Configuration</h4>
<table>
  <thead>
    <tr>
      <th>Property</th>
      <th>Type</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>TargetActor</code></td>
      <td><code>AActor*</code></td>
      <td>The actor that this agent will attempt to move towards. Its location is used as the pathfinding destination.</td>
    </tr>
    <tr>
      <td><code>AcceptanceRadius</code></td>
      <td><code>float</code></td>
      <td>The distance from a waypoint or the final destination at which it is considered 'reached'. Prevents the agent from circling its target.</td>
    </tr>
  </tbody>
</table>

<h4>Agent Properties</h4>
<table>
  <thead>
    <tr>
      <th>Property</th>
      <th>Type</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>BaseAgentRadius</code></td>
      <td><code>float</code></td>
      <td>The agent's fundamental radius, assuming its scale is 1.0. The actual pathfinding radius is this value multiplied by the actor's scale.</td>
    </tr>
    <tr>
      <td><code>AgentRadius</code></td>
      <td><code>float</code></td>
      <td>(Read-Only) The final, calculated radius used for pathfinding (BaseAgentRadius * Actor Scale).</td>
    </tr>
    <tr>
      <td><code>AvoidanceRadius</code></td>
      <td><code>float</code></td>
      <td>The 'personal space' radius for real-time dynamic avoidance. For predictable results, it is recommended to set this value equal to the 'Pathfinding Radius (Scaled)'.</td>
    </tr>
  </tbody>
</table>

<h4>Movement</h4>
<table>
  <thead>
    <tr>
      <th>Property</th>
      <th>Type</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>MovementSpeed</code></td>
      <td><code>float</code></td>
      <td>The target speed (in UU/s) at which the agent moves along its calculated path.</td>
    </tr>
    <tr>
      <td><code>RotationInterpSpeed</code></td>
      <td><code>float</code></td>
      <td>Controls how quickly the agent rotates to face its direction of travel. Higher values result in sharper turns.</td>
    </tr>
    <tr>
      <td><code>MovementMode</code></td>
      <td><code>enum</code></td>
      <td>(Read-Only) Shows the current movement strategy (Spline or Linear). This is determined automatically by the component for optimal, safe movement.</td>
    </tr>
    <tr>
      <td><code>PartialPointSplineNum</code></td>
      <td><code>int</code></td>
      <td>The number of future path points to use when generating the spline for smooth following.</td>
    </tr>
  </tbody>
</table>

<h4>Dynamic Pathfinding</h4>
<table>
  <thead>
    <tr>
      <th>Property</th>
      <th>Type</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>bPeriodicRepath</code></td>
      <td><code>bool</code></td>
      <td>If true, the agent will automatically request a new path on a timer, defined by Repath Cooldown. Essential for tracking moving targets.</td>
    </tr>
    <tr>
      <td><code>RepathCooldown</code></td>
      <td><code>float</code></td>
      <td>The time in seconds to wait between automatic path requests when bPeriodicRepath is enabled.</td>
    </tr>
    <tr>
      <td><code>GoalUpdateThreshold</code></td>
      <td><code>float</code></td>
      <td>Triggers a new path request only if the target has moved more than this distance (in cm) since the last path was calculated. A performance-saving alternative to periodic repathing.</td>
    </tr>
  </tbody>
</table>

<h4>Debugging & Tools</h4>
<table>
  <thead>
    <tr>
      <th>Property</th>
      <th>Type</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>bDrawDebugPath</code></td>
      <td><code>bool</code></td>
      <td>If true, the agent's calculated path will be visualized in the world.</td>
    </tr>
    <tr>
      <td><code>Duration</code></td>
      <td><code>float</code></td>
      <td>How long (in seconds) the debug path visualization persists in the viewport.</td>
    </tr>
    <tr>
      <td><code>LineThickness</code></td>
      <td><code>float</code></td>
      <td>The thickness of the debug path lines.</td>
    </tr>
    <tr>
      <td><code>BoxThickness</code></td>
      <td><code>float</code></td>
      <td>The thickness of the outline for the debug boxes at each waypoint.</td>
    </tr>
    <tr>
      <td><code>BoxHalfSize</code></td>
      <td><code>float</code></td>
      <td>The size of the debug boxes drawn at each waypoint.</td>
    </tr>
    <tr>
      <td><code>PathColor</code></td>
      <td><code>FColor</code></td>
      <td>The color of the debug path lines connecting waypoints.</td>
    </tr>
    <tr>
      <td><code>PathPointColor</code></td>
      <td><code>FColor</code></td>
      <td>The color of the debug boxes at each waypoint.</td>
    </tr>
    <tr>
      <td><code>CurrentState</code></td>
      <td><code>enum</code></td>
      <td>(Read-Only) Shows the agent's current high-level behavior (e.g., Idle, FollowPath).</td>
    </tr>
    <tr>
      <td><code>CurrentTargetLocation</code></td>
      <td><code>FVector</code></td>
      <td>(Read-Only) The world location the agent is currently trying to reach.</td>
    </tr>
    <tr>
      <td><code>FoundPath</code></td>
      <td><code>TArray&lt;FVector&gt;</code></td>
      <td>(Read-Only) The array of FVectors representing the waypoints of the current path.</td>
    </tr>
  </tbody>
</table>

## Subsystems

### AvoidanceSubsystem
Description...
