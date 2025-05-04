# ChappalGame
Chappal: Mobile Game Development Project Walkthrough
Overview: A cross-platform (Android & iOS) Unity game where two roles exist: 
- Parents throw sandals (chappals) and Kids dodge them. 
- Game modes include local NPC play, LAN mode, and online 5v5 PvP. 
- Monetization via ad‑unlocked sandal upgrades. 
- Low storage footprint, simple controls, 3D cartoon style.

1. Pre‑Production & Planning
Define Core Mechanics


Parent Role: Aim & throw sandals; limited stamina; special throws.
Kid Role: Dodge via movement & dodge‑roll; collect power‑ups.
Win Conditions: All kids eliminated or time runs out.


Art Style & Scope


Simple low‑poly 3D characters and environment (like Among Us vibe).
One small arena map (~50 MB total textures & assets).


Technical Stack


Unity LTS (latest stable), C# scripting.
Networking: Photon PUN2 for online, Unity Transport + Mirror for LAN fallback.
Monetization: Unity Ads + in‑game item system.


Team Roles


Dev A: Core gameplay, physics, animations.
Dev B: Networking, UI, monetization, build & store pipeline.


Milestones & Timeline


Week 1–2: Setup & prototype single‑player.
Week 3–4: LAN multiplayer prototype.
Week 5–6: Online multiplayer integration.
Week 7–8: UI/UX polish, ads & store builds.
Week 9–10: Testing, bugfixes, soft launch.

2. Project Setup
Install Unity


Download Unity Hub & Unity LTS (e.g., 2024.x) with Android & iOS modules.


Version Control


Create Git repo (e.g., on GitHub: bhusalgitthub/ChappalGame).
Add .gitignore for Unity; branch per feature.


Project Settings


Configure Player Settings: Company Name, Product Name = Chappal.
Set Bundle IDs for Android (com.yourcompany.chappal) & iOS.
Quality Settings: Mobile profile, disable real‐time shadows.
Optimize Build Size: Texture compression (ASTC/ETC2), strip unused code.


Folder Structure
/Assets
  /Art
    /Characters
    /Props
    /Environments
  /Scripts
    /Core
    /Networking
    /UI
    /Managers
  /Scenes
    /Prototype
    /MainMenu
    /Game
  /Prefabs
  /Plugins

3. Core Gameplay Prototype


Scene Setup
Create Prototype scene with simple plane & capsules.
Player Controller
C# PlayerController: WASD/touch joystick movement + dodge roll.
Camera follow script (3rd‐person, adjustable offset).
Throwing Mechanic
C# Chappal prefab: Rigidbody + Collider + script with Launch(Vector3 dir, float force).
Parent script: Aim via screen‐touch direction, then Instantiate(chappal).Launch(...).
Collision Logic
On Chappal collision with Kid: call Kid.TakeDamage(damage).
Kid health & respawn logic (HealthManager).
NPC Behavior (for early test)
Simple state machine: Idle → Patrol → Throw at nearest kid every 3 sec.

4. Networking: LAN & Online


Common Network Manager
Singleton GameNetworkManager to choose LAN or Photon at runtime.
LAN Mode (Mirror)
Import Mirror, configure Transport for Local LAN.
Host & Client UI: Discover LAN hosts via broadcast.
Online Mode (Photon PUN2)
Setup Photon App in Unity: App ID, Regions.
Room UI: Create/Join random room of 10 players.
Sync player spawns & chappal instantiation via PUN RPCs.
Matchmaking & Teams
On room join, assign role based on current count:Indices 0–4 = Parents, 5–9 = Kids.
Use Photon custom properties to track role & ready state.
State Synchronization
Network‐sync PlayerController position/rotation.
Sync Chappal launches (PhotonNetwork.Instantiate).

5. UI & UX
Main Menu: Buttons → Single Player, LAN, Online, Settings, Shop.
In‐Game HUD
Health bars, timer, score display (kills/survivors).
Role icon (parent/kid).
Shop/Ads
Unity Ads setup: rewarded placement triggers upgrade unlock.
In‐shop view: chappal skins & damage multipliers.
Settings
Audio volume sliders, graphics quality toggle.
Network selection (force LAN/online).

6. Art & Asset Pipeline
Low‑Poly Modeling
Use Blender: basic characters with 500–1,000 tris.
Simple animations: Idle, Run, Throw, Dodge.
Texture & Materials
Single atlas for characters where possible.
Mobile‐friendly shaders (URP/Lit).
Asset Import Settings
Compress textures, mesh compression on models.
Optimize import of audio (Mono, 22 kHz).

7. Monetization & Analytics
Unity Ads
Rewarded ads: after X views unlock next sandal tier.
In‑App Purchases (Optional)
Use Unity IAP for direct sandal pack purchase.
Analytics
Unity Analytics or Firebase: track session length, ad views.

8. Testing & Optimization
Single‑Player QA
Playtest core mechanics & controls.
Multiplayer Tests
LAN: multiple devices on same network.
Online: simulate high latency & packet loss.
Performance Profiling
Unity Profiler: reduce draw calls, optimize scripts.
Beta Release
TestFlight (iOS) & Internal testing in Google Play.

9. Build & Deployment
Android
Configure keystore, signing.
Generate APK/AAB & submit to Google Play console.
iOS
Setup Apple Developer account, provisioning profile.
Build Xcode project & submit via App Store Connect.
Store Metadata
Descriptions, screenshots, feature graphics.
Age rating & category settings.

10. Post‑Release & Live Ops
Monitoring
Crashlytics, user reviews, analytics dashboards.
Updates & Events
Seasonal maps, sandal skins, limited‑time modes.
Community Engagement
Social media, Discord server for feedback & events.





Week 1: Core Setup & Single‑Player Prototype
Day 1:
Both devs clone bhusalgitthub/ChappalGame and configure Unity Project Settings (bundle IDs, graphics profile).
Dev B sets up Git branches (main, feature/setup).
Day 2–3:
Dev A implements PlayerController movement (touch & keyboard). Create CameraFollow script.
Dev B configures project folder structure and .gitignore; commits initial project template.
Day 4–5:
Dev A builds jump/dodge-roll animations in Blender, exports as FBX; integrate into Unity.
Dev B sets up Unity Ads SDK and creates a test rewarded-ad button in the UI.
Day 6–7:
Combined: Create Prototype scene; instantiate player prefab, test movement & dodge. Commit working build.

Week 2: Throwing & Collision Mechanics
Day 8–9:
Dev A scripts Chappal prefab with Launch(dir,force) and collision detection.
Dev B designs simple UI overlay showing parent/kid role icons.
Day 10–11:
Dev A integrates throw aiming via touch swipe or mouse drag; test in editor.
Dev B creates HealthManager script and HUD health bars.
Day 12–14:
Combined: Test throw → hit → damage flow. Polish collision feedback (particles, sound).
Merge feature/throwing into main with code review.

Week 3: NPC & LAN Networking Prototype
Day 15–16:
Dev A builds NPC NPCParentController: patrol + throw at nearest kid every 3 secs.
Dev B imports Mirror, sets up GameNetworkManager for LAN discovery UI.
Day 17–18:
Both test LAN host/client on two devices: spawn synced players; validate movement.
Day 19–21:
Implement network‐sync for PlayerController and Chappal launches; fix latency jitter.

Week 4: Online Multiplayer with Photon PUN2
Day 22–23:
Dev B integrates Photon PUN2 SDK, configures App ID & Regions.
Create lobby UI: random match join.
Day 24–25:
Dev A implements room join callbacks, spawn players via PhotonNetwork.Instantiate.
Day 26–28:
Both set up team assignment logic (Parents vs Kids), test in 2–4 client sessions.

Week 5: UI/Shop/Ads Integration
Day 29–30:
Dev B builds Shop scene: catalog of sandal skins, watch ads to unlock tiers.
Day 31–32:
Dev A connects rewarded-ad callback to player damage multiplier upgrade.
Day 33–35:
Polish main menu, settings toggles, audio controls. Test ad flow end‑to‑end.

Week 6: Performance, Polish & Internal QA (Days 36–42)
Day 36–37:
Performance Profiling: Use Unity Profiler to identify CPU/GPU hotspots; optimize shaders, reduce draw calls, and bake lighting where possible.
Asset Optimization: Compress textures (ASTC/ETC2), enable mesh compression, strip unused assets.
Day 38–39:
Graphics & Effects Polish: Refine particle effects (dust, impact splashes), adjust post‑processing (bloom, ambient occlusion) with mobile performance in mind.
Audio Tuning: Balance SFX and music volumes; add 3D spatial audio parameters to throws and impacts.
Day 40–42:
Internal QA Playtests: Distributed to the team via internal test builds (Android APK sideload, iOS TestFlight). Collect feedback on controls, graphics, and stability.
Bug Triage: Log issues in GitHub with labels (performance, polish, bug); assigned to devs.

Week 7: Closed Beta & Feedback Loop (Days 43–49)
Day 43–44:
Beta Build Preparation: Create versioned beta builds (v0.9) for Google Play Internal Testing and TestFlight group. Ensure ad‑unlock flows are working.
Release Notes & Survey: Draft clear release notes and a short in‑game feedback survey (via Google Forms link).
Day 45–47:
Beta Deployment: Roll out builds to selected testers (friends, community). Monitor crash logs (Crashlytics/Firebase).
Feedback Collection: Gather quantitative data (session length, ad watches) and qualitative feedback (controls feel, enjoyment).
Day 48–49:
Analyze Feedback: Summarize top 10 bug reports and feature suggestions. Prioritize fixes vs. enhancements.
Sprint Planning: Create GitHub Issues for Week 8 fixes; assign tasks for bug severity. 

Week 8: Critical Fixes & Store Prep (Days 50–56)
Day 50–52:
Critical Bug Fixes: Address top 5 showstopper issues (crashes, major performance drops, ad‑SDK errors).
UI/UX Refinements: Polish menus, button layouts, tutorial prompts based on beta feedback.
Day 53–54:
Marketing Asset Creation: Prepare store screenshots, gameplay trailers (30‑second clip), icon variants for both platforms.
Store Listing Draft: Write compelling app descriptions, feature bullets, and localization notes if needed.
Day 55–56:
Final Internal Validation: Smoke test store-ready build (v1.0), verify versioning and signings (keystore & provisioning profiles).
Submission Ready: Upload to Google Play Console (internal track) and App Store Connect (TestFlight for review).

Week 9: Submission & Marketing Launch (Days 57–63)
Day 57–58:
Store Submission: Submit builds for review. Double‑check compliance (age rating, privacy policy link).
App Store Optimization (ASO): Optimize keywords, prepare A/B testing for icons/titles.
Day 59–60:
Marketing Kickoff: Announce soft‑launch on social channels (Twitter, Instagram, Discord). Share teaser videos.
Press Outreach: Send press kits to mobile game blogs, request early impressions.
Day 61–63:
Review Tracking: Monitor App Store and Play Store review statuses; respond to any review feedback from Apple.
Community Engagement: Host a live demo session on Discord or Twitch to showcase gameplay and gather live feedback.

Week 10: Live Monitoring & Live Ops Planning (Days 64–70)
Day 64–66:
Post‑Launch Analytics: Review first‑week KPIs—DAU, retention (Day 1/7), ad revenue metrics, crash rate.
Hotfix Deploy: Address any critical issues emerging in review or post‑launch analytics (patch v1.0.1).
Day 67–68:
Live Ops Calendar: Plan upcoming events—seasonal themes (e.g., summer festival), new sandal tiers, limited‑time maps.
Monetization Review: Analyze ad‑unlock drop‑off rates; adjust ad‑frequency tuning if needed.
Day 69–70:
Roadmap Update: Create GitHub Project board for Q2 content updates (maps, skins, modes).
Stakeholder Review: Hold retrospective with team; document lessons learned and celebrate milestone.

