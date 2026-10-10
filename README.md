<br>
<div align="center">
  <h2>
    <a href="../../releases/latest">📥 DOWNLOAD LATEST VERSION</a>
  </h2>
</div>
<br>

<div align="right">
  <h3>🌍 <strong>English</strong> | <a href="README.ru.md">🇷🇺 Русский</a></h3>
</div>
<div align="center">
  <h1>🌌 Proximity Engine for After Effects</h1>
  <p><b>Distance-based layer proximity animation extension for Adobe After Effects.</b></p>

  [![Version](https://img.shields.io/badge/version-v0.7-00F0FF?style=for-the-badge)](CHANGELOG.md)
  [![Platform](https://img.shields.io/badge/platform-After_Effects-FF2A6D?style=for-the-badge)](#)
</div>

---
<p align="center">
  <img width="32%" alt="изображение" src="https://github.com/user-attachments/assets/0608d077-588d-4aa9-b508-e8c59bba0c20" />
  <img width="32%" alt="изображение" src="https://github.com/user-attachments/assets/ee781221-c63d-4933-908d-90f447d1150f" />
  <img width="32%" alt="изображение" src="https://github.com/user-attachments/assets/cd368e44-2f17-47fd-b617-a84d3e7561c1" />
</p>

## 📖 About the Project
Proximity Engine is a CEP extension for Adobe After Effects. Primarily, it is a tool for effortlessly animating a massive number of layers on the timeline using a single controller layer (Proximity animation). Its main feature is the ability to easily create large-scale scenes with numerous animated objects. It allows you to animate layer properties (position, scale, rotation, opacity, and custom effects) based on their physical distance to a selected controller layer on the timeline.

## ✨ Features and Mechanics

### 🚀 100% Portability (No Plugin Required for Playback)
It is important to note that the plugin is simply a UI wrapper for the engine itself. Projects created using this extension can be safely shared with others. Even if they don't have Proximity Engine installed, everything will work perfectly. Moreover, they will still be able to edit the project and use most of its features, as almost all calculations are offloaded to native expressions on the layers.

### 📐 1. Distance Physics and Transformations
*   **Property Control:** Link Position (pushing objects apart), Scale, Rotation, Opacity, and Custom Effect parameters (e.g., sliders) to the controller.
*   **Separate 3D Rotation:** Individual toggles for X, Y, and Z axes. The script checks if the layer is 3D; if it's 2D, standard flat rotation is applied.
*   **One-Sided 3D Opacity:** Normal vector calculation. The opacity of a 3D layer drops to 0% if it turns its "back" to the camera.
*   **Boundary Sharpness (Sigmoid Sharpness):** Each property has a sharpness parameter (from 1 to 10) that defines exactly how the animation propagates through a massive array of objects. At the minimum value (1), the controller approaching a cluster of layers creates a soft, smooth wave of transformations. As the value increases, the interpolation curve compresses into a hard boundary. This turns a smooth wave into a sharp, stepped, or "pixelated" cascading effect, where the animation difference between adjacent objects becomes maximally contrasting, snapping to new values instantly rather than blending gradually.
*   **Deterministic Randomization:** Global start offset and random value scatter. This is based on a custom pseudo-randomization algorithm tied to layer names. This means **layers with identical names will generate absolutely identical random values**. This is indispensable when you need to stack several layers that must move synchronously, without disabling the global random for the rest of the scene.

### 🧠 2. Centralized "Mega-Expression" Architecture
Instead of applying heavy processing code to every single layer in the scene, the plugin uses a "mega-expression" system. All core computational code is located on one universal layer (`PROXIMITY_ENGINE`), while all other layers simply reference it using lightweight link expressions.
*   **Cleanliness & Optimization:** The project stays clean, avoids bloat from duplicated code, and management is centralized in a single point—the engine layer.
*   **Headless Operation:** Thanks to this architecture, when working without the extension panel open (or if you handed the project to a colleague without the plugin), **absolutely all parameters can be freely edited manually right inside the expression of the `PROXIMITY_ENGINE` text layer**.

### 🧩 3. Expressions Management
*   **Expression Blending (Blend Mode):** When activated, the plugin doesn't overwrite your old expressions (e.g., `wiggle()`). The code is wrapped in a `try...catch` block, a mathematical delta is calculated, and the Proximity effect is added on top of existing animations.
*   **Smart Disconnect:** The plugin locates its expressions on selected layers, removes them, and restores previous values or previous custom expressions.
*   **Shape Groups Support:** Ability to apply expressions to internal shape layer groups (`Contents > Group 1`). The script calculates local position deltas, so internal groups won't fly outside the composition bounds.
*   **Cross-Comp Binding:** Link layers to a controller physically located in a different composition.

### 🎛️ 4. Multi-Instance
*   **Multiple Engines:** Create several independent systems within a single composition (`PROXIMITY_ENGINE`, `PROXIMITY_ENGINE_2`, etc.).
*   **Engine Selector:** A dropdown menu to switch context. All parameters and bindings are automatically routed to the selected text layer engine.

### 🗂️ 5. Group Editor (Mass Operations)
*   **Group Detection:** The plugin groups layers by their base names (stripping number suffixes, e.g., `Base 1` and `Base 2` become the `Base` group).
*   **Group Sync:** Link properties of one layer group to another based on suffix matching (e.g., automatically transferring properties from `Overlay 1` to `Base 1`, and so on for all layers in the groups).
*   **🎭 Mass Track Matte & Parent:** The Group Editor includes the ability to assign Track Mattes (Alpha/Luma, including Inverted) and Parents between layer groups based on their numbering (e.g., layer `Mask 1` automatically becomes the mask for `Panel 1`, etc., for all layers in the two groups). Supports both the new Track Matte API (AE 23.0+) and the classic method for older AE versions (Note: the classic method is untested, stability unknown).
*   **Group Effect Remover:** Select an effect on one layer, and the plugin will remove that exact same effect from all other layers in that group with a single click.
*   **Target Selection:** Quickly select all layers of a chosen group on the timeline. Useful if group layers are scattered throughout the comp with other layers in between them.
*   **Active Connections Manager:** A unified list to view and manage all active bindings (Links, Mattes, Parents). The delete button recognizes and breaks the selected connection type.

### 💾 6. Preset System
*   **JSON Format:** Save current parameters as lightweight `.json` files.
*   **Custom Folder:** Choose any local folder for storage. The path is saved globally in After Effects settings (`app.settings`) and works across all projects.
*   **Management:** Load, Overwrite, and Delete presets directly from the UI.

### 🖥️ 7. Interface & QoL
*   **Auto-Apply:** Option to send values to After Effects in real-time as you type in text fields.
*   **Sync Button:** Force request current values from the text layer engine back into the UI.
*   **Mass Duplicate:** Feature to generate a specified number of layer copies. Includes a direction toggle (Upwards / Downwards) to place duplicates above or below the original on the timeline. Perfect for quickly generating layers before linking them.

---

## 📦 Installation Guide

1. Download the `ProximityEngine vX.X.zip` archive from the [Releases](../../releases) section.

2. Extract the archive contents into the Adobe CEP extensions system folder:
   - **Windows:** `C:\Program Files (x86)\Common Files\Adobe\CEP\extensions\`
   - **macOS:** `/Library/Application Support/Adobe/CEP/extensions/`

3. **Enable Developer Mode (PlayerDebugMode):**
   Since the extension does not have an official Adobe digital signature, it must be allowed in the OS; otherwise, After Effects will block it.

   * **For Windows (Automatic):**
     1. Download the `PlayerDebugMode.reg` file from the repository.
     2. Double-click the file.
     3. Read the warning, click "Yes", or proceed to the manual method.

   * **For Windows (Manual):**
     1. Press `Win + R`, type `regedit`, and press `Enter`.
     2. Navigate to: `HKEY_CURRENT_USER\Software\Adobe\`
     3. Find the key for your CSXS version (e.g., `CSXS.11`, `CSXS.12`, etc., depending on your After Effects version). If it doesn't exist, create it manually.
     4. Inside this key, create a String Value named **`PlayerDebugMode`**.
     5. Set its value to **`1`**.

   * **For macOS:**
     Open Terminal and execute the following command (replace `11` with your CSXS version if using a different AE release year):
     ```bash
     defaults write com.adobe.CSXS.11 PlayerDebugMode 1
     ```

4. Restart After Effects and navigate to the menu: **Window > Extensions > Proximity Engine**.
<br>
<div align="center">
  <h2>
    <a href="../../releases/latest">📥 DOWNLOAD LATEST VERSION</a>
  </h2>
</div>
