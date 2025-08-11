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
<p>The <code>OctreeVoxelVolume</code> actor is responsible for managing the static collision data used for pathfinding. It bakes the environment into an octree structure.</p>

<h4>Properties</h4>
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
      <td><code>VoxelExtent</code></td>
      <td><code>float</code></td>
      <td>
        <details>
          <summary>The side length of the smallest individual voxel.</summary>
          It directly controls the granularity of the navigation grid. Smaller voxel extents allow for more precise pathfinding and better obstacle avoidance, especially for agents with small radii. Conversely, a larger voxel extent results in a coarser grid, which is more memory-efficient but may lead to less optimal paths or collisions with smaller details.
        </details>
      </td>
    </tr>
    <tr>
      <td><code>MapSize</code></td>
      <td><code>FIntVector</code></td>
      <td>
        <details>
          <summary>The overall dimensions (X, Y, Z) of the navigation volume.</summary>
          It defines the total area where pathfinding can occur. This setting, combined with Voxel Extent, determines the physical size of the navigable space. A larger map size covers a wider area but can also contribute to increased memory usage and baking time.
        </details>
      </td>
    </tr>
    <tr>
      <td><code>BakedOctreeData</code></td>
      <td><code>TSoftObjectPtr&lt;UOctreeDataAsset&gt;</code></td>
      <td>
        <details>
          <summary>Holds the pre-baked octree data for static geometry.</summary>
          This data is loaded at runtime to initialize the static collision information for pathfinding.
        </details>
      </td>
    </tr>
    <tr>
      <td><code>StaticObjectProfile</code></td>
      <td><code>UVoxelObstacleProfile*</code></td>
      <td>
        <details>
          <summary>Defines which actors are treated as static obstacles.</summary>
          This profile allows you to filter objects by their Collision Channel or Object Tags, giving you precise control over what is included in the baked navigation data. The profile can be configured to 'Match Any' (baking objects that meet any of the specified criteria) or 'Match And' (baking only objects that meet all specified criteria). If no profile is assigned, the system defaults to treating all objects with the <code>WorldStatic</code> collision object type as obstacles.
        </details>
      </td>
    </tr>
    <tr>
      <td><code>BakeExcludeActors</code></td>
      <td><code>TArray&lt;AActor*&gt;</code></td>
      <td>
        <details>
          <summary>(Editor-only) Actors to be ignored during baking.</summary>
          Any actors added to this list will be explicitly ignored during the octree baking process, preventing them from being considered static obstacles.
        </details>
      </td>
    </tr>
  </tbody>
</table>

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
<p>These are the publicly exposed variables in <code>UAvoidanceSubsystem</code>, which are accessible in the Unreal Editor's Details panel and can be read/written in Blueprints.</p>

<h4>Properties</h4>
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
      <td><code>TimeHorizon</code></td>
      <td><code>float</code></td>
      <td>Defines how far into the future the avoidance system considers potential collisions.</td>
    </tr>
    <tr>
      <td><code>MaxNeighbors</code></td>
      <td><code>int32</code></td>
      <td>Specifies the maximum number of neighboring agents or obstacles considered for each agent during calculations.</td>
    </tr>
    <tr>
      <td><code>GridCellSize</code></td>
      <td><code>float</code></td>
      <td>Defines the size of grid cells used for spatial partitioning, influencing obstacle detection granularity.</td>
    </tr>
    <tr>
      <td><code>SymmetryBreakingRangeMultiplier</code></td>
      <td><code>float</code></td>
      <td>Introduces asymmetry to prevent agents from getting stuck in reciprocal oscillations.</td>
    </tr>
    <tr>
      <td><code>SolverIterations</code></td>
      <td><code>int32</code></td>
      <td>Determines the number of iterations the avoidance solver performs for optimal velocity calculation.</td>
    </tr>
    <tr>
      <td><code>SideBiasMagnitude</code></td>
      <td><code>float</code></td>
      <td>Controls the strength of a bias encouraging agents to prefer passing on one side of an obstacle.</td>
    </tr>
    <tr>
      <td><code>InterpSpeed</code></td>
      <td><code>float</code></td>
      <td>Controls how quickly an agent adjusts its current velocity towards the calculated optimal avoidance velocity.</td>
    </tr>
    <tr>
      <td><code>bShowVelocityDebugLine</code></td>
      <td><code>bool</code></td>
      <td>When true, enables debug lines to visualize agent velocities.</td>
    </tr>
    <tr>
      <td><code>bUseAvoidance</code></td>
      <td><code>bool</code></td>
      <td>When true, enables the avoidance system for agents.</td>
    </tr>
  </tbody>
</table>
