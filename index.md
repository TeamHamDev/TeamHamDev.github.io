---
# 프로젝트 루트 폴더에 새로 만드세요.
layout: splash
permalink: /
header:
  overlay_color: "#000"
  overlay_filter: "0.7"
  overlay_image: /assets/images/main-background.jpg # /assets/images 폴더에 배경 이미지를 넣어주세요.
  actions:
    - label: "<i class='fas fa-video'></i> DEMO VIDEO"
      url: "https://www.youtube.com/watch?v=EJKfgji1gbg" # 실제 데모 영상 링크로 수정
    - label: "<i class='fas fa-download'></i> DOWNLOAD ON FAB"
      url: "https://www.fab.com/ko/listings/256ea996-5989-4c8c-b9f4-ce2182fe0999"
title: "Smart Flying Navigation"
excerpt: >
  Octree-based 3D pathfinding with Lazy Theta* smoothing and asynchronous 3D ORCA avoidance, enabling natural and efficient multi-agent flight in large, complex worlds.

---

## Change Log

<div class="changelog-entry" markdown="1">
### v1.0.3 — 2025-08-12

**Improved**

- **UI/UX:** Exposed all UAvoidanceSubsystem parameters in a new "Avoidance Tuning" section within the Pathfinding Tuner, allowing for detailed real-time configuration.
- **Component Properties:** Exposed the AcceptanceRadius parameter on the UPathfindingComponent for more flexible goal completion criteria.
- **Component Properties:** Exposed the RotationInterpSpeed parameter on the UPathfindingComponent, allowing for adjustable agent turning speeds.

**Fixed**

- **Core Logic:** Fixed a critical bug where the OnDestinationReached event would fail to trigger, especially when moving sequentially between multiple targets.
</div>

<div class="changelog-entry" markdown="1">
### v1.0.2 — 2025-08-10

**Fixed**

- packaging-related bug fix
</div>

<div class="changelog-entry" markdown="1">
### v1.0.1 — 2025-08-09

**Added**

- Lazy Theta* pathfinding for visibility-based shortcuts and smoother paths

**Improved**

- Slate UI/UX: improved visual clarity and component recognition, enabling faster and more accurate setup

**Fixed**

- A* path discontinuity occurring under specific obstacle arrangements
</div>