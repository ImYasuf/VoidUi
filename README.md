<div align="center">

<br/>

```
██╗   ██╗ ██████╗ ██╗██████╗ ██╗   ██╗██╗
██║   ██║██╔═══██╗██║██╔══██╗██║   ██║██║
██║   ██║██║   ██║██║██║  ██║██║   ██║██║
╚██╗ ██╔╝██║   ██║██║██║  ██║██║   ██║██║
 ╚████╔╝ ╚██████╔╝██║██████╔╝╚██████╔╝██║
  ╚═══╝   ╚═════╝ ╚═╝╚═════╝  ╚═════╝ ╚═╝
```

**A modern, sleek Roblox UI Library**

![License](https://img.shields.io/badge/license-MIT-white?style=flat-square)
![Lua](https://img.shields.io/badge/language-Lua-white?style=flat-square)
![Roblox](https://img.shields.io/badge/platform-Roblox-white?style=flat-square)
![Version](https://img.shields.io/badge/version-1.0.0-white?style=flat-square)

</div>

---

## Table of Contents

- [Features](#features)
- [Hosting on GitHub](#hosting-on-github)
- [Loading VoidUI](#loading-voidui)
- [Creating a Window](#creating-a-window)
- [Tabs](#tabs)
- [Sections](#sections)
- [Elements](#elements)
  - [Button](#button)
  - [Toggle](#toggle)
  - [Slider](#slider)
  - [Dropdown](#dropdown)
  - [TextInput](#textinput)
  - [Keybind](#keybind)
  - [ColorPicker](#colorpicker)
  - [Label](#label)
  - [Separator](#separator)
- [Notifications](#notifications)
- [Element Methods](#element-methods)
- [Full Example Script](#full-example-script)
- [Theming](#theming)

---

## Features

| Feature | Details |
|---|---|
| Window | Draggable · Resizable · Minimizable · Close button |
| Tabs | Sidebar navigation with active indicator |
| Elements | Button · Toggle · Slider · Dropdown · TextInput · Keybind · ColorPicker |
| Notifications | Info · Success · Warning · Error with progress bar |
| Theme | Monochrome · Dark · Modern |
| API | Every element has `:Set()` / `:Get()` for full programmatic control |

---

## Loading VoidUI

Once hosted, load VoidUI at the top of any script using `loadstring` and `game:HttpGet`:

```lua
local VoidUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/ImYasuf/VoidUi/refs/heads/main/loader"))()
```

---

## Creating a Window

```lua
local Window = VoidUI:CreateWindow({
    Title    = "My Script",       -- Window title (string)
    Size     = UDim2.new(0, 560, 0, 420),  -- Optional, this is the default
    Position = UDim2.new(0.5, -280, 0.5, -210), -- Optional, centers on screen
})
```

| Property | Type | Default | Description |
|---|---|---|---|
| `Title` | string | `"VoidUI"` | Text shown in the title bar |
| `Size` | UDim2 | `560 × 420` | Initial window size |
| `Position` | UDim2 | Centered | Initial window position |

The window supports:
- **Drag** — click and drag the title bar to move
- **Resize** — drag the handle in the bottom-right corner
- **Minimize** — click `─` to collapse to the title bar only
- **Close** — click `✕` to destroy the window with a fade animation

---

## Tabs

Tabs appear in the left sidebar. You can have as many as you want.

```lua
local MainTab = Window:CreateTab({
    Name = "Main",   -- Tab label (string)
    Icon = "⚡",     -- Optional emoji or symbol
})

-- Shorthand (name only, no icon):
local SettingsTab = Window:CreateTab("Settings")
```

| Property | Type | Required | Description |
|---|---|---|---|
| `Name` | string | ✅ | Label shown in the sidebar |
| `Icon` | string | ❌ | Emoji/symbol shown before the name |

The first tab created is automatically selected.

---

## Sections

Sections group elements under a labeled divider inside a tab.

```lua
local CombatSection = MainTab:CreateSection("Combat")
local MovementSection = MainTab:CreateSection("Movement")

-- No label (just a visual grouping):
local UnnamedSection = MainTab:CreateSection()
```

All element methods are called on the section, not the tab directly.

---

## Elements

### Button

A clickable row that fires a callback.

```lua
CombatSection:CreateButton({
    Name     = "Kill All",
    Callback = function()
        print("Button clicked!")
    end,
})

-- Shorthand:
CombatSection:CreateButton("Kill All")
```

| Property | Type | Default | Description |
|---|---|---|---|
| `Name` | string | `"Button"` | Button label |
| `Callback` | function | `function() end` | Fires when clicked |

---

### Toggle

An on/off switch that fires a callback with the new state.

```lua
local MyToggle = CombatSection:CreateToggle({
    Name     = "Aimbot",
    Default  = false,      -- Initial state
    Callback = function(state)
        print("Aimbot is now:", state) -- true or false
    end,
})
```

| Property | Type | Default | Description |
|---|---|---|---|
| `Name` | string | `"Toggle"` | Toggle label |
| `Default` | boolean | `false` | Initial on/off state |
| `Callback` | function | `function(state) end` | Fires on change; receives `true`/`false` |

**Methods:**
```lua
MyToggle:Set(true)   -- Programmatically set state
MyToggle:Get()       -- Returns current state (boolean)
```

---

### Slider

A draggable slider for numeric values.

```lua
local MySlider = CombatSection:CreateSlider({
    Name     = "FOV",
    Min      = 10,
    Max      = 360,
    Default  = 90,
    Decimals = 0,      -- Decimal places (0 = integer)
    Suffix   = "°",    -- Unit shown after the value
    Callback = function(value)
        print("FOV:", value)
    end,
})
```

| Property | Type | Default | Description |
|---|---|---|---|
| `Name` | string | `"Slider"` | Slider label |
| `Min` | number | `0` | Minimum value |
| `Max` | number | `100` | Maximum value |
| `Default` | number | `Min` | Initial value |
| `Decimals` | number | `0` | Decimal places in displayed value |
| `Suffix` | string | `""` | Unit appended to the value (e.g. `"°"`, `" ms"`) |
| `Callback` | function | `function(value) end` | Fires on drag; receives current number |

**Methods:**
```lua
MySlider:Set(120)  -- Set value programmatically
MySlider:Get()     -- Returns current value (number)
```

---

### Dropdown

A single-select dropdown list.

```lua
local MyDropdown = CombatSection:CreateDropdown({
    Name     = "Target Part",
    Items    = { "Head", "Torso", "HumanoidRootPart" },
    Default  = "Head",
    Callback = function(selected)
        print("Selected:", selected)
    end,
})
```

| Property | Type | Default | Description |
|---|---|---|---|
| `Name` | string | `"Dropdown"` | Dropdown label |
| `Items` | table | `{}` | List of string options |
| `Default` | string | First item | Initially selected option |
| `Callback` | function | `function(selected) end` | Fires on selection; receives chosen string |

**Methods:**
```lua
MyDropdown:Set("Torso")                         -- Change selected item
MyDropdown:Get()                                -- Returns selected string
MyDropdown:SetItems({ "A", "B", "C" })          -- Replace the item list
```

---

### TextInput

A single-line text box.

```lua
local MyInput = SettingsSection:CreateTextInput({
    Name        = "Player Name",
    Placeholder = "Enter name...",
    Default     = "",
    Callback    = function(text, pressedEnter)
        if pressedEnter then
            print("Submitted:", text)
        end
    end,
})
```

| Property | Type | Default | Description |
|---|---|---|---|
| `Name` | string | `"Input"` | Field label |
| `Placeholder` | string | `"Type here..."` | Ghost text when empty |
| `Default` | string | `""` | Pre-filled value |
| `Callback` | function | `function(text, enter) end` | Fires on focus lost; `enter` is `true` if Enter was pressed |

**Methods:**
```lua
MyInput:Set("Hello")  -- Set value programmatically
MyInput:Get()         -- Returns current text (string)
```

---

### Keybind

Lets the player assign a keyboard shortcut. Click the button then press any key to bind it.

```lua
local MyKeybind = SettingsSection:CreateKeybind({
    Name     = "Toggle Menu",
    Default  = Enum.KeyCode.RightShift,
    Callback = function(key)
        -- Fires every time the bound key is pressed (not just on rebind)
        print("Hotkey pressed:", key.Name)
    end,
})
```

| Property | Type | Default | Description |
|---|---|---|---|
| `Name` | string | `"Keybind"` | Label |
| `Default` | KeyCode | `Enum.KeyCode.Unknown` | Initial keybind |
| `Callback` | function | `function(key) end` | Fires on keypress; receives `Enum.KeyCode` |

**Methods:**
```lua
MyKeybind:Set(Enum.KeyCode.F)  -- Set keybind programmatically
MyKeybind:Get()                -- Returns current KeyCode
```

---

### ColorPicker

An HSV color picker with a saturation/value square and hue bar. Click the color swatch to open/close.

```lua
local MyColor = VisualSection:CreateColorPicker({
    Name     = "ESP Color",
    Default  = Color3.new(1, 1, 1),
    Callback = function(color)
        print("R:", math.round(color.R * 255))
        print("G:", math.round(color.G * 255))
        print("B:", math.round(color.B * 255))
    end,
})
```

| Property | Type | Default | Description |
|---|---|---|---|
| `Name` | string | `"Color"` | Label |
| `Default` | Color3 | `Color3.new(1,1,1)` | Initial color |
| `Callback` | function | `function(color) end` | Fires on change; receives `Color3` |

**Methods:**
```lua
MyColor:Set(Color3.fromRGB(255, 0, 0))  -- Set color programmatically
MyColor:Get()                            -- Returns current Color3
```

---

### Label

A plain text label, useful for descriptions or status readouts.

```lua
MySection:CreateLabel("This feature requires admin permissions.")
```

---

### Separator

A thin horizontal divider line to visually separate groups of elements.

```lua
MySection:CreateSeparator()
```

---

## Notifications

Notifications appear as cards in the bottom-right corner of the screen with a color-coded accent and an animated progress bar.

```lua
-- Via the window object:
Window:Notify({
    Title    = "Action Complete",
    Desc     = "Your config has been saved.",
    Type     = "Success",    -- "Info" | "Success" | "Warning" | "Error"
    Duration = 4,            -- Seconds before auto-dismiss
})

-- Globally (no window needed):
VoidUI:Notify({
    Title    = "VoidUI Loaded",
    Desc     = "Script initialized.",
    Type     = "Info",
    Duration = 3,
})
```

| Property | Type | Default | Description |
|---|---|---|---|
| `Title` | string | `"VoidUI"` | Bold heading text |
| `Desc` | string | `""` | Smaller body text |
| `Type` | string | `"Info"` | Color theme: `Info` (blue) · `Success` (green) · `Warning` (yellow) · `Error` (red) |
| `Duration` | number | `4` | Auto-dismiss time in seconds |

---

## Element Methods

Every interactive element returns an object with consistent methods:

| Method | Returns | Description |
|---|---|---|
| `:Set(value)` | nothing | Programmatically update the element |
| `:Get()` | current value | Read the current value |

This lets you connect elements to each other or to game state without needing to track values yourself:

```lua
-- Example: reset slider when toggle turns off
local speedSlider = MovementSection:CreateSlider({
    Name = "Speed", Min = 16, Max = 500, Default = 16,
    Callback = function(val)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = val
    end,
})

MovementSection:CreateToggle({
    Name = "Speed Hack",
    Callback = function(state)
        if not state then
            speedSlider:Set(16)  -- Reset to default when toggled off
        end
    end,
})
```

---

## Full Example Script

```lua
-- Load VoidUI
local VoidUI = loadstring(game:HttpGet(
    "https://raw.githubusercontent.com/YOUR_USERNAME/VoidUI/main/VoidUI.lua"
))()

-- Create window
local Window = VoidUI:CreateWindow({ Title = "My Script" })

-- ── Tab: Main ────────────────────────────────
local MainTab = Window:CreateTab({ Name = "Main", Icon = "⚡" })
local CombatSection = MainTab:CreateSection("Combat")

CombatSection:CreateToggle({
    Name     = "Aimbot",
    Default  = false,
    Callback = function(state)
        -- your aimbot logic here
    end,
})

CombatSection:CreateSlider({
    Name     = "FOV",
    Min      = 10,
    Max      = 360,
    Default  = 90,
    Suffix   = "°",
    Callback = function(val)
        -- update FOV
    end,
})

CombatSection:CreateDropdown({
    Name     = "Target Part",
    Items    = { "Head", "Torso", "HumanoidRootPart" },
    Default  = "Head",
    Callback = function(part)
        -- update target
    end,
})

local MovementSection = MainTab:CreateSection("Movement")

MovementSection:CreateToggle({
    Name     = "Fly",
    Callback = function(state) end,
})

MovementSection:CreateSlider({
    Name     = "Walk Speed",
    Min      = 16,
    Max      = 500,
    Default  = 16,
    Callback = function(val)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = val
    end,
})

-- ── Tab: Visual ───────────────────────────────
local VisualTab = Window:CreateTab({ Name = "Visual", Icon = "👁" })
local ESPSection = VisualTab:CreateSection("ESP")

ESPSection:CreateToggle({
    Name     = "Player ESP",
    Callback = function(state) end,
})

ESPSection:CreateColorPicker({
    Name     = "ESP Color",
    Default  = Color3.new(1, 1, 1),
    Callback = function(color) end,
})

-- ── Tab: Settings ─────────────────────────────
local SettingsTab = Window:CreateTab({ Name = "Settings", Icon = "⚙" })
local GenSection = SettingsTab:CreateSection("General")

GenSection:CreateTextInput({
    Name        = "Config Name",
    Placeholder = "MyConfig",
    Callback    = function(text, enter)
        if enter then print("Saving as:", text) end
    end,
})

GenSection:CreateKeybind({
    Name     = "Toggle Menu",
    Default  = Enum.KeyCode.RightShift,
    Callback = function(key)
        print("Hotkey:", key.Name)
    end,
})

GenSection:CreateButton({
    Name     = "Save Config",
    Callback = function()
        Window:Notify({
            Title    = "Config Saved",
            Desc     = "Settings saved successfully!",
            Type     = "Success",
            Duration = 3,
        })
    end,
})

-- ── Startup notification ───────────────────────
VoidUI:Notify({
    Title    = "VoidUI Loaded",
    Desc     = "Script initialized successfully.",
    Type     = "Info",
    Duration = 4,
})
```

---

## Theming

VoidUI's colors are stored in the `Theme` table inside the library. You can access and modify them after loading:

```lua
local VoidUI = loadstring(game:HttpGet("YOUR_RAW_URL"))()

-- Override accent color (default is white/monochrome)
VoidUI.Theme.Accent     = Color3.fromRGB(100, 180, 255)  -- blue accent
VoidUI.Theme.ToggleOn   = Color3.fromRGB(100, 180, 255)
VoidUI.Theme.SliderFill = Color3.fromRGB(100, 180, 255)

-- Then create your window as normal
local Window = VoidUI:CreateWindow({ Title = "My Script" })
```

> ⚠️ Theme changes must be made **before** creating any windows or elements, as colors are read at creation time.

**Available theme keys:**

| Key | Default | Description |
|---|---|---|
| `Background` | `10, 10, 10` | Main window background |
| `Surface` | `18, 18, 18` | Title bar and sidebar |
| `SurfaceAlt` | `24, 24, 24` | Element row background |
| `Border` | `40, 40, 40` | Stroke/outline color |
| `Accent` | `220, 220, 220` | Primary accent (toggle, slider, dot) |
| `AccentDim` | `130, 130, 130` | Secondary accent |
| `Text` | `230, 230, 230` | Primary text |
| `TextMuted` | `120, 120, 120` | Secondary/label text |
| `Success` | `80, 200, 120` | Notification success color |
| `Warning` | `220, 180, 60` | Notification warning color |
| `Error` | `220, 70, 70` | Notification error color |
| `Info` | `80, 160, 220` | Notification info color |

---

<div align="center">

Made with 🖤 using VoidUI

</div>
