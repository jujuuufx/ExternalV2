# 🌌 External V2 UI Library

A modern, clean, and highly functional dark-theme UI library for Roblox scripts.
Features an **Azure Blue accent**, smooth tweens, built-in window resizing, drag support, and an intuitive API.

---

## 📑 Table of Contents

* [Bootstrapping (Loading the UI)](#-bootstrapping)
* [Creating the Window](#-creating-the-window)
* [Tabs & Sections](#-tabs--sections)
* [UI Elements](#-ui-elements)

  * Label
  * Button
  * Toggle
  * Slider
  * TextBox
  * Dropdown
* [Manipulating Elements Programmatically](#-manipulating-elements-programmatically)
* [Using the Flags System](#-using-the-flags-system)
* [Built-in Features](#-built-in-features)
* [Full Example Script](#-full-example-script)

---

# 🚀 Bootstrapping

To start using the UI library, load it using `loadstring`.
The library returns:

* `Library` → Main UI handler
* `Flags` → Table storing element states

```lua
-- Replace "YOUR_RAW_URL_HERE" with your raw script link
local Library, Flags = loadstring(game:HttpGet("YOUR_RAW_URL_HERE"))()
```

---

# 🖥️ Creating the Window

The window is the main container for your UI.

```lua
local Window = Library:Window({
    Prefix = "External.",     -- Accent-colored title text
    Suffix = "wtf",          -- White title text
    Size = UDim2.new(0, 680, 0, 500) -- Default window size
})
```

---

# 🗂️ Tabs & Sections

## Creating a Tab

Tabs appear on the left sidebar.

```lua
local MainTab = Window:Tab({
    Name = "Main Features",
    Icon = "rbxassetid://112730572155522"
})
```

## Creating a Section

Sections must specify `"Left"` or `"Right"` (2-column layout).

```lua
local LeftSection = MainTab:Section({
    Name = "Player Movement",
    Side = "Left" -- "Left" or "Right"
})
```

---

# 🧩 UI Elements

## Label

```lua
LeftSection:Label({
    Name = "This is a warning label!"
})
```

---

## Button

```lua
LeftSection:Button({
    Name = "Print Hello",
    Callback = function()
        print("Hello World!")
    end
})
```

---

## Toggle

```lua
LeftSection:Toggle({
    Name = "Aimbot",
    Flag = "AimbotToggle",
    Default = false,
    Callback = function(Value)
        print("Aimbot is now:", Value)
    end
})
```

---

## Slider

```lua
LeftSection:Slider({
    Name = "WalkSpeed",
    Flag = "WalkSpeedSlider",
    Min = 16,
    Max = 200,
    Default = 16,
    Intervals = 1,        -- Use 0.1 for decimals
    Suffix = " studs/s",
    Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = Value
    end
})
```

---

## TextBox

```lua
LeftSection:Textbox({
    Name = "Target Player",
    Flag = "TargetInput",
    Default = "Username",
    Callback = function(Text)
        print("Target set to:", Text)
    end
})
```

---

## Dropdown

### Single Select

```lua
LeftSection:Dropdown({
    Name = "Hitbox Part",
    Flag = "HitboxDropdown",
    Options = {"Head", "Torso", "HumanoidRootPart"},
    Default = "Head",
    Multi = false,
    Callback = function(Value)
        print("Selected Hitbox:", Value)
    end
})
```

### Multi Select

```lua
LeftSection:Dropdown({
    Name = "ESP Options",
    Flag = "ESP_Options",
    Options = {"Boxes", "Names", "Health", "Distance"},
    Default = {"Boxes", "Names"},
    Multi = true,
    Callback = function(SelectedTable)
        for _, option in pairs(SelectedTable) do
            print("Selected:", option)
        end
    end
})
```

---

# ⚙️ Manipulating Elements Programmatically

Elements return objects with usable methods.

```lua
local MySlider = LeftSection:Slider({
    Name = "FOV",
    Flag = "FOV_Slider",
    Min = 70,
    Max = 120,
    Default = 70
})

-- Change value later
MySlider.Set(90)
```

## Refreshing a Dropdown

```lua
local PlayerDropdown = LeftSection:Dropdown({
    Name = "Select Player",
    Flag = "PlayerList",
    Options = {"None"}
})

-- Update options dynamically
PlayerDropdown.Refresh({"Player1", "Player2", "Player3"})
```

---

# 🚩 Using the Flags System

The `Flags` table automatically stores element values.

No globals needed.

```lua
local Library, Flags = loadstring(game:HttpGet("..."))()

LeftSection:Toggle({
    Name = "Auto Heal",
    Flag = "AutoHeal",
    Default = false
})

game:GetService("RunService").Heartbeat:Connect(function()
    if Flags["AutoHeal"] then
        -- Healing logic
    end
end)
```

You can also access flags like:

```lua
if Flags.AutoHeal then
```

---

# 🎛️ Built-in Features

### 🖱 Window Dragging

Drag the top title bar to move the UI.

### 📏 Window Resizing

Drag the bottom-right blue corner dot to resize.

### 👁 Hide / Show

A floating toggle button is automatically created on the right side of your screen.

---

# 📜 Full Example Script

```lua
local Library, Flags = loadstring(game:HttpGet("YOUR_RAW_URL_HERE"))()

local Window = Library:Window({
    Prefix = "External.",
    Suffix = "wtf",
    Size = UDim2.new(0, 600, 0, 450)
})

local MainTab = Window:Tab({ Name = "Local Player" })

local MovementSection = MainTab:Section({
    Name = "Movement",
    Side = "Left"
})

local MiscSection = MainTab:Section({
    Name = "Misc",
    Side = "Right"
})

MovementSection:Slider({
    Name = "WalkSpeed",
    Flag = "WalkSpeed",
    Min = 16,
    Max = 100,
    Default = 16,
    Callback = function(val)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = val
    end
})

MovementSection:Toggle({
    Name = "Infinite Jump",
    Flag = "InfJump",
    Default = false
})

MiscSection:Button({
    Name = "Print Flags Table",
    Callback = function()
        for k, v in pairs(Flags) do
            print(k, "=", tostring(v))
        end
    end
})

game:GetService("UserInputService").JumpRequest:Connect(function()
    if Flags.InfJump then
        game.Players.LocalPlayer.Character
            :FindFirstChildOfClass("Humanoid")
            :ChangeState(Enum.HumanoidStateType.Jumping)
    end
end)
```

---
