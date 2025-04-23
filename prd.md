# Project Requirements Document (PRD)

## Project Title: Path of the Apprentice

### Codename: “POTA”

---

### 1. Executive Summary

Path of the Apprentice is a lightweight, single‑player action‑adventure RPG built with Unity 2022 LTS for Windows and macOS.  Designed explicitly as a *learning vehicle* for a first‑time game developer, it delivers a 5–10‑minute vertical slice showcasing exploration, combat, looting, and dialogue.  The project emphasizes clarity of code structure, repeatable production pipelines, and disciplined use of version control so that each feature teaches a core Unity concept.

---

### 2. Goals & Success Criteria

| Goal                                  | Success Proof                                                                                 |
| ------------------------------------- | --------------------------------------------------------------------------------------------- |
| Understand Unity’s core editor tools  | You can confidently navigate Hierarchy, Inspector, Scene/Game views, and Profiler             |
| Master essential C# gameplay patterns | Implement inventory, melee & ranged combat, and dialogue without external tutorials           |
| Ship a playable vertical slice        | Build runs on Win / Mac, delivers 5–10 min of polished gameplay, and is shareable via itch.io |
| Adopt professional workflows          | Git branches for every feature; PR‑style reviews; automated headless build passes             |

---

### 3. Target Audience & Platforms

- **Primary learner**: you (solo developer)
- **Secondary audience**: friends/mentors providing feedback
- **Platforms**: Desktop PC & Mac (stand‑alone builds, keyboard & mouse, 16:9 / 16:10)

---

### 4. Gameplay Scope

- **Core Loop**: *Explore → Fight → Loot → Upgrade*
- **Vertical Slice Content**
  - **Scenes**: Title → Village Hub → Forest → Boss Arena → Credits
  - **Characters**: 1 player prefab, 2 enemy types (Goblin Melee, Goblin Archer), 1 NPC quest‑giver
  - **Items**: 5 weapons (3 melee, 2 ranged), 3 consumables (potion, food, scroll)
  - **Systems**: Combat, Inventory, Equipment, Dialogue, Quests, Save/Load
  - **Play Time Goal**: 5–10 minutes for average tester

*Anything not listed lives in the “Nice‑to‑Have Backlog.”*

---

### 5. Technical Requirements

- **Engine**: Unity 2022 LTS or later (URP template)
- **Language / Runtime**: C# 10, .NET Standard 2.1
- **Key Packages**: TextMeshPro, Cinemachine, Input System, Addressables
- **Version Control**: Git (GitHub), branch naming `feature/<slug>` → Pull Request → merge to *main*
- **Build Target**: Win x64&#x20;

---

### 6. Team & Responsibilities

| Role                | Name                       | Weekly Time | Key Deliverables                       |
| ------------------- | -------------------------- | ----------- | -------------------------------------- |
| Solo Dev / Producer | *You*                      | \~5 hrs     | All code, design, integration          |
| Art Sourcing        | Asset Store / marketplaces | –           | Low‑poly character & environment packs |
| QA Play‑Testing     | Friends & forum volunteers | ad‑hoc      | Written feedback, bug tickets          |

---

### 7. Tools & Pipelines

- **IDE**: Rider or VS Code with Unity extensions
- **3D Art**: Blender → Substance Painter → export FBX/PNG
- **Audio**: Audacity (editing), royalty‑free libraries
- **Continuous Integration**: GitHub Action runs `-batchmode` Unity build, publishes zip artifact
- **Pre‑Commit Hook**: `dotnet format`, Unity asset refresh

---

### 8. Milestones & Schedule (12‑Week Roadmap)

| Week | Focus             | Key Deliverables                                        |
| ---- | ----------------- | ------------------------------------------------------- |
| 1    | Project bootstrap | Repo cleaned, folder structure, placeholder cube player |
| 2    | Movement & Camera | Cinemachine follow cam, WASD/Mouse look                 |
| 3    | Stats & UI        | XP system, HUD bars                                     |
| 4    | Melee Combat & AI | NavMesh enemies, hit detection                          |
| 5    | Inventory         | UI window, item pickup                                  |
| 6    | Equipment         | Weapon switching, basic VFX                             |
| 7    | Ranged Combat     | Projectile base class                                   |
| 8    | Dialogue & Quests | Quest giver NPC, dialogue UI                            |
| 9    | Loot Tables       | Weighted drops                                          |
| 10   | Save / Load       | JSON persistence, Addressables intro                    |
| 11   | Polish            | Audio, animations, pause/settings                       |
| 12   | Build & Ship      | CI build passes, itch.io upload                         |

Weekly retrospective & demo video every Sunday evening.

---

### 9. Risks & Mitigations

| Risk                | Likelihood | Impact | Mitigation                                                       |
| ------------------- | ---------- | ------ | ---------------------------------------------------------------- |
| Feature creep       | Med        | High   | Maintain a strict *Nice‑to‑Have* list; freeze scope after Week 6 |
| Learning curve      | High       | Med    | Complete Unity Learn tutorial before each milestone              |
| Asset compatibility | Low        | Med    | Conform to URP, test import pipeline early                       |
| Burn‑out            | Med        | High   | Limit weekly hours, schedule one week buffer after Week 10       |

---

### 10. Frontend (Player‑Facing) Requirements

- **UI/UX**: Unity UI Toolkit preferred; responsive (anchor‑based) layouts
- **Prefabs**: Main Menu, HUD, Inventory, Dialogue Box, Pause Menu
- **Art Style**: Choose *Low‑Poly Fantasy* by Week 2; document color palette & naming conventions (`Art/<Category>/<Asset>_v01.fbx`)
- **Audio**: SFX categories (UI, Combat, Ambient, Footsteps); 2 music loops (overworld, battle)
- **Input Map** (`Input Actions` asset):
  - `Move` (WASD)
  - `Look` (Mouse)
  - `Interact` (E)
  - `Attack` (LMB)
  - `Pause` (Esc)

---

### 11. Backend (Game‑Logic) Requirements

- **Architecture**
  - ScriptableObjects for static data (Items, Quests, LootTables)
  - Interfaces (`IDamageable`, `IInteractable`) for polymorphism
  - Namespace layers: `RPG.Core`, `RPG.Gameplay`, `RPG.UI`
- **Systems**
  - **Stats & Leveling**: `StatSO`, additive & percentage modifiers, event‑driven XP
  - **Combat**: `MeleeWeapon`, `RangedWeapon`; `(Attack – Defense) × Crit` formula
  - **Inventory & Equipment**: `ItemInstance` list; drag‑drop UI; equipment adds stat mods
  - **Dialogue & Quests**: sequential nodes, Quest SO with objectives
  - **Save/Load**: JSON at `Application.persistentDataPath`; serializes player state & current scene
  - **Game State Manager**: `DontDestroyOnLoad` bootstrap, handles scene transitions
- **Data Streaming**: Addressables for large scenes & audio

---

### 12. DevOps & Quality

- **Branch Protection**: Require PR review + CI pass to merge
- **CI Pipeline**: Unity ‑batchmode ‑nographics build → zip artifact → GitHub Releases (draft)
- **Code Quality**: `dotnet format`, maximum 200 LOC per script, profiler run every milestone
- **Testing**: Play‑mode tests for critical systems (stat math, save/load)

---

### 13. Asset‑Creation Pipeline

1. **Concept** in Procreate/Photoshop → approve silhouette & palette
2. **Model** in Blender → apply proper naming (`SM_Player_v01`)
3. **UV Unwrap** → texture in Substance Painter (Albedo / Normal / RM)
4. **Export** FBX + textures → Unity `Assets/Art/<Category>/`
5. **In‑Engine**: configure import settings, create Prefab, add colliders & animator

---

### 14. Best‑Practice Coding Guidelines

- Separate *data* (SO) from *behaviour* (MonoBehaviour)
- Minimize `Update()` usage; prefer events & coroutines
- Use `UnityEvent` or C# events for decoupled messaging (`OnDamageTaken`)
- Custom editor inspectors once a workflow becomes repetitive
- Profile early & often (Window → Analysis → Profiler)

---

### 15. Acceptance Criteria for Vertical Slice

- New player can download ZIP, run executable, finish demo in ≤10 min
- All mapped inputs work; no blocking bugs or crashes
- Save/Load retains player position, inventory, and quest progress
- GitHub Actions build succeeds; artifact size ≤300 MB
- Each prefab, script, and asset folder follows naming conventions

---

### 16. Next Steps

1. **Confirm** Unity version (2022 LTS URP) & package list
2. **Push** clean `main` branch with starter folder structure:\
   `Assets/{Scripts,Art,Scenes,Prefabs,SO,UI}`
3. Begin *Milestone 1* tasks (see schedule)
4. Schedule first check‑in call end of Week 1 for review

