# Asset Reference Restrictions Sample

This sample project demonstrates how to configure and enforce strict **Asset Referencing Policies** in Unreal Engine without writing C++ code, leveraging the built-in **Asset Referencing Policy** plugin.

It showcases developer sandbox isolation (`/Game/Developers/`) and gameplay variant boundaries (`Variant_Horror` vs. `Variant_Shooter`).

---

## 🎯 Key Objectives

1. **Developer Sandbox Isolation**:
   - Prevent assets located in `/Game/Developers/` from leaking into cooked builds or being referenced by core project content.
   - Utilize the `DirectoriesToNeverCook` array in Packaging Settings to automatically trigger the plugin's built-in `Never Cooked Content` domain.

2. **Gameplay Variant Isolation**:
   - Isolate gameplay variants (`Variant_Horror` and `Variant_Shooter`) so they can both reference shared project frameworks (`/Game/Characters/`, core systems) while preventing cross-variant dependencies.

---

## 🛠️ How It Works

### 1. Developer Sandbox Domain
The `/Game/Developers/` folder is added to **Project Settings > Packaging > Directories to never cook**.
- **Result**: The plugin treats this directory as a *Never Cooked Content* domain.  
  Developers can reference any core game asset inside their sandbox, but core assets cannot reference sandbox items.

### 2. Custom Domains for Variants
Custom Asset Reference Domains are set up in **Project Settings > Asset Referencing Policy**:
- **Domain `Variant_Horror`**: Covers `/Game/Variant_Horror/`.
- **Domain `Variant_Shooter`**: Covers `/Game/Variant_Shooter/`.

Both domains allow access to standard Engine and Project content but explicitly forbid referencing each other.

---

## 📊 Summary Matrix

| Scenario | Action | Result in Editor |
| :--- | :--- | :--- |
| Developer opens a test Blueprint in `/Game/Developers/` and references `/Game/Weapons/` | **Allowed** | Full read access to shared project content. |
| Developer tries to place a test actor from `/Game/Developers/` into `/Game/FirstPerson/Maps/` | **Blocked** | Asset Picker hides the test actor; Data Validation throws an error on save. |
| `Variant_Horror` Blueprint references a character mesh from `/Game/Characters/` | **Allowed** | Both variants access shared core content. |
| `Variant_Shooter` Blueprint attempts to reference an asset inside `/Game/Variant_Horror/` | **Blocked** | Cross-variant dependencies are prevented. |
