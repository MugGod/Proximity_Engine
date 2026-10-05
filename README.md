<br>
<div align="center">
  <h2>
    <a href="../../releases/latest">📥 DOWNLOAD LATEST RELEASE</a>
  </h2>
</div>
<br>

<div align="right">
  <h3>🌍 <strong>English</strong> | <a href="README.ru.md">🇷🇺 Русский</a></h3>
</div>

<div align="center">
  <h1>🌌 Proximity Engine for After Effects</h1>
  <p><b>A distance-based animation extension for Adobe After Effects.</b></p>

  [![Version](https://img.shields.io/badge/version-v0.6-00F0FF?style=for-the-badge)](CHANGELOG.md)
  [![Platform](https://img.shields.io/badge/platform-After_Effects-FF2A6D?style=for-the-badge)](#)
</div>

---

## 📖 About
Proximity Engine is a CEP extension for Adobe After Effects, designed primarily as a powerful tool for animating a massive number of layers on the timeline using a single Controller layer. Its main feature is the ability to create complex scenes with hundreds of animated objects as easily as possible. It allows you to animate layer properties (such as Position, Scale, Rotation, Opacity, and Custom Effects) based on their physical distance to a designated "Controller" layer on the timeline.

## ✨ Features & Mechanics

### 🚀 100% Portable (No Plugin Required for Playback)
An essential feature of this project is that the plugin acts purely as a UI wrapper for the engine. Projects created using Proximity Engine can be safely shared with other people. Even if they don't have this extension installed, everything will work perfectly. They will even retain the ability to edit the project and use most of its features because almost all calculations are natively baked into expressions on the layers.

### 📐 1. Distance Physics & Transformations
*   **Property Control:** Link Position (push objects away), Scale, Rotation, Opacity, and Custom Effect properties (like sliders or specific plugin parameters) to the controller.
*   **Granular 3D Support:** Separate toggles for X, Y, and Z rotation. The script automatically detects 3D layers. If applied to a 2D layer, it falls back to standard 2D rotation.
*   **One-Sided 3D Opacity:** Calculates the layer's normal vector. Opacity can be set to drop to 0% when the back of a 3D layer faces the camera.
*   **Sigmoid Sharpness:** Each property has a "Sharpness" parameter (1-10) that defines how the animation propagates across a massive group of objects. At the minimum value (1), moving the controller through the layers creates a smooth, continuous wave of transformations. At higher values, the interpolation curve compresses into a hard boundary. This turns the smooth wave into a sharp, stepped, or "pixelated" cascading effect, where neighboring objects pop into their new states instantly rather than blending gradually.
*   **Deterministic Randomization:** Set global wake-up offsets or property-specific random spreads. The engine uses a custom pseudo-random algorithm tied to layer names. This means **layers with identical names will generate the exact same random values**. This is incredibly useful when you need to stack or "sandwich" multiple layers that must move perfectly in sync, without disabling the random effect for the rest of the scene.

### 🧠 2. Centralized "Mega-Expression" Architecture
Instead of duplicating heavy calculation code onto every single animated layer, Proximity Engine utilizes a centralized "mega-expression" system. The core mathematical engine resides solely on one universal layer (`PROXIMITY_ENGINE`), while all other layers simply use lightweight reference expressions that pull data from this central hub. 
*   **Clean & Performant:** Keeps your project timeline completely uncluttered and centralizes all control to a single engine layer.
*   **Headless Editing (No UI required):** Because of this architecture, if you are working without the extension panel open (or if you send the project to someone who doesn't own the plugin), **absolutely all engine parameters can be manually tweaked directly inside the text expression of the `PROXIMITY_ENGINE` layer**.

### 🧩 3. Expression Management
*   **Blend Expressions (Safe Mode):** When enabled, the plugin does not overwrite existing expressions on your layers (e.g., `wiggle()`). It wraps your original code in a `try...catch` block and calculates the mathematical delta, adding the proximity effect on top.
*   **Smart Disconnect:** The plugin can locate its own generated code on selected layers, remove it, and restore the original values or previous user expressions.
*   **Shape Group Targeting:** Expressions can be applied directly to internal Shape Layer groups (e.g., `Contents > Group 1`). The script calculates local position deltas so internal paths don't offset outside the composition.
*   **Cross-Comp Binding:** Allows linking layers to an engine and controller that are physically located in a different composition.

### 🎛️️ 4. Multi-Instance Support
*   **Multiple Engines:** Create several independent engines in a single composition (named `PROXIMITY_ENGINE`, `PROXIMITY_ENGINE_2`, etc.).
*   **Engine Dropdown:** A built-in selector in the UI allows you to switch the active context. Edits and connections are routed to the selected engine.

### 🗂️ 5. Group Editor (Bulk Operations)
*   **Group Detection:** The plugin groups layers by their base names (ignoring trailing numbers, e.g., `Base 1` and `Base 2` form the `Base` group).
*   **Group Sync:** Link properties from one layer group to another based on matching suffixes. (e.g., Apply properties from `Overlay 1` to `Base 1`, `Overlay 2` to `Base 2`, automatically).
*   **Group Effect Remover:** Select a specific effect on one layer and remove that exact effect from all other layers in the same group with one click.
*   **Target Selection:** Instantly highlight all layers belonging to a specific group on the timeline.

### 💾 6. Preset System
*   **JSON Storage:** Save current engine parameters as lightweight `.json` files.
*   **Custom Directory:** Choose any local folder to store presets. The path is saved globally in After Effects preferences (`app.settings`) and persists across projects.
*   **Management:** Load, overwrite, and delete presets directly from the plugin interface.

### 🖥️ 7. Interface & QoL Features
*   **Auto-Apply:** A toggle that updates engine parameters in After Effects in real-time as you type or adjust values in the UI.
*   **Sync Button:** Fetches current values from the active Engine text layer back into the UI inputs.

---

## 📦 Installation Instructions

1. Download the `ProximityEngine vX.X.zip` archive from the [Releases](../../releases) section.

2. Extract the contents of the archive into the Adobe CEP extensions system folder:
   - **Windows:** `C:\Program Files (x86)\Common Files\Adobe\CEP\extensions\`
   - **macOS:** `/Library/Application Support/Adobe/CEP/extensions/`

3. **Enable Developer Mode (PlayerDebugMode):**
   Since the extension does not have an official Adobe digital signature, it must be allowed in the system, otherwise After Effects will block it.

   * **For Windows (Automatic):**
     1. Download the `PlayerDebugMode.reg` file from the repository.
     2. Double-click the file.
     3. Read the warning, click Yes, or proceed to the manual option.

   * **For Windows (Manual):**
     1. Press `Win + R`, type `regedit`, and press `Enter`.
     2. Navigate to: `HKEY_CURRENT_USER\Software\Adobe\`
     3. Find the key for the required version of your CSXS package (e.g., `CSXS.11`, `CSXS.12`, etc., depending on the After Effects version). If there is no such key, create it manually.
     4. Inside this key, create a String Value named **`PlayerDebugMode`**.
     5. Change its value to **`1`**.

   * **For macOS:**
     Open the terminal and run the command (replace `11` with the version of your CSXS package if using a different year of After Effects):
     ```bash
     defaults write com.adobe.CSXS.11 PlayerDebugMode 1
     ```

4. Restart After Effects and go to the menu: **Window > Extensions > Proximity Engine**.

<br>
<div align="center">
  <h2>
    <a href="../../releases/latest">📥 DOWNLOAD LATEST RELEASE</a>
  </h2>
</div>
<br>
