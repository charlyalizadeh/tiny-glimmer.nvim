# 🌟 tiny-glimmer.nvim

A tiny Neovim plugin that adds subtle animations to yank operations.

![Neovim version](https://img.shields.io/badge/Neovim-0.10+-blueviolet.svg)
![License](https://img.shields.io/badge/license-MIT-blue.svg)

## ✨ Features

![tiny_glimmer_demo](https://github.com/user-attachments/assets/f662b9d3-98f5-4683-97e4-c74fe98e2f0e)

- Smooth animations for yank operations
- Multiple animation styles:
  - `fade`: Simple fade in/out effect
  - `bounce`: Bouncing transition
  - `left_to_right`: Linear left-to-right animation
  - `pulse`: Pulsating highlight effect
  - `rainbow`: Rainbow transition
  - `custom`: Custom animation that you can define

## 📋 Requirements

- Neovim >= 0.10

## 📦 Installation

Using [lazy.nvim](https://github.com/folke/lazy.nvim):
```lua
{
    "rachartier/tiny-glimmer.nvim",
    event = "TextYankPost",
    opts = {
        -- your configuration
    },
}
```

Using [packer.nvim](https://github.com/wbthomason/packer.nvim):
```lua
use {
    'rachartier/tiny-glimmer.nvim',
    config = function()
        require('tiny-glimmer').setup()
    end
}
```

## ⚙️ Configuration

Here's the default configuration:
```lua
require('tiny-glimmer').setup({
    enabled = true,
    default_animation = "fade",
    refresh_interval_ms = 6,

    -- Only use if you have a transparent background
    -- It will override the highlight group background color for `to_color` in all animations
    transparency_color = nil,

    animations = {
        fade = {
            max_duration = 250,
            chars_for_max_duration = 10,
        },
        bounce = {
            max_duration = 500,
            chars_for_max_duration = 20,
            oscillation_count = 1,
        },
        left_to_right = {
            max_duration = 350,
            chars_for_max_duration = 40,
            lingering_time = 50,
        },
        pulse = {
            max_duration = 400,
            chars_for_max_duration = 15,
            pulse_count = 2,
            intensity = 1.2,
        },
        rainbow = {
            max_duration = 600,
            chars_for_max_duration = 20,
        },
        custom = {
            max_duration = 350,
            chars_for_max_duration = 40,
            color = hl_visual_bg,

            -- Custom effect function
            -- @param self table The effect object
            -- @param progress number The progress of the animation [0, 1]
            --
            -- Should return a color and a progress value
            -- that represents how much of the animation should be drawn
            effect = function(self, progress)
                return self.settings.color, progress
            end,
        },
    },
    virt_text = {
        priority = 2048,
    },
})
```

For each animation, you can configure the `from_color` and `to_color` options to customize the colors used in the animation. These options should be valid highlight group names, or hexadecimal colors.

Example:
```lua
require('tiny-glimmer').setup({
    animations = {
        fade = {
            from_color = "DiffDelete",
            to_color = "DiffAdd",
        },
        bounce = {
            from_color = "#ff0000",
            to_color = "#00ff00",
        },
    },
})
```

> [!WARNING]
Only `rainbow` animation does not uses `from_color` and `to_color` options.

### Animation Settings

Each animation type has its own configuration options:

- `max_duration`: Maximum duration of the animation in milliseconds
- `chars_for_max_duration`: Number of characters that will result in max duration
- `lingering_time`: How long the animation stays visible after completion (for applicable animations)
- `oscillation_count`: Number of bounces (for bounce animation)
- `pulse_count`: Number of pulses (for pulse animation)
- `intensity`: Animation intensity multiplier (for pulse animation)

## 🎮 Commands

- `:TinyGlimmer enable` - Enable animations
- `:TinyGlimmer disable` - Disable animations
- `:TinyGlimmer fade` - Switch to fade animation
- `:TinyGlimmer bounce` - Switch to bounce animation
- `:TinyGlimmer left_to_right` - Switch to left-to-right animation
- `:TinyGlimmer pulse` - Switch to pulse animation
- `:TinyGlimmer rainbow` - Switch to rainbow animation
- `:TinyGlimmer custom` - Switch to your custom animation

## 🛠️ API

```lua
-- Enable animations
require('tiny-glimmer').enable()

-- Disable animations
require('tiny-glimmer').disable()

-- Toggle animations
require('tiny-glimmer').toggle()
```


## ❓FAQ

### Why is there two animations playing at the same time?
You should disable your own `TextYankPost` autocmd that calls `vim.highlight.on_yank`

## 📝 License

MIT

