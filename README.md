# Samsung_USBC_Headset_20190816_Linux_Fix
Microphone fix for the Samsung_USBC_Headset_20190816 on Linux that uses PipeWire.

# How to apply fix
Step 1: Run ``nano ~/.config/wireplumber/main.lua.d/50-usb-audio.lua``
Step 2: Paste in 
```
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
Step 3: Run ``systemctl --user restart wireplumber pipewire pipewire-pulse`` and the microphone will be fixed.
