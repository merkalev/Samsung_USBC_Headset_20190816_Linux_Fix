# Samsung_USBC_Headset_20190816_Linux_Fix

Fixes microphone issues for the Samsung USB-C headset on Linux (PipeWire).

## How to apply

**1. Open config file**

```bash
nano ~/.config/wireplumber/main.lua.d/50-usb-audio.lua
```

**2. Paste this**

```lua
alsa_monitor.rules = alsa_monitor.rules or {}

table.insert(alsa_monitor.rules, {
  matches = {
    {
      { "node.name", "equals", "alsa_output.usb-Samsung_USBC_Headset_20190816-00.analog-stereo" },
    },
  },
  apply_properties = {
    ["audio.format"] = "S16LE",
    ["audio.rate"] = 48000,
  },
})

table.insert(alsa_monitor.rules, {
  matches = {
    {
      { "node.name", "equals", "alsa_input.usb-Samsung_USBC_Headset_20190816-00.mono-fallback" },
    },
  },
  apply_properties = {
    ["audio.format"] = "S16LE",
    ["audio.rate"] = 48000,
  },
})
```

**3. Restart services**

```bash
systemctl --user restart wireplumber pipewire pipewire-pulse
```

Enjoy!
