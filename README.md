# Winlator-Bionic-101²-
This is a guide for winlator bionic and forks for NEW users, mostly for adreno, specifically highlighting the use of Proton-arm64ec and fexcore. there will be no workarounds for niche games, but will help you troubleshoot problems yourself when trying to configure games. 

### Proton/wine

Proton 9 is recommended; however you may switch if a game is having cutscene issues; or a game/launcher has patches to work on a newer proton, the proton10-arm64ec2(gamenative) or Proton10-arm64ec(legacy) in ludashi, If both of these don't work, then try the other versions.


### Win components

These can stay mostly untouched, unless you're having audio issues, you can try switching xaudio, directmusic to native or builtin.


### Environmental variables

these are in some cases, crucial for some games to work, you can either find the variables in protondb or winehq, here are a few that may be of note. 

`WINEESYNC=1` ; May be needed to be turned off; `=0`, for some games to boot, but with very heavy performance repercussions.

`TU_DEBUG`= ; When using turnip, for mature GPUs like 6xx and 7xx series, you can disable gmem and  sysmem to enable autotuner on the turnip driver, which switches between both to improve stutter/fps in rare cases. For 8xx, some of the variables like sysmem are a must have, and some games require noubwc or nolrz to render properly.

`WINEDLLOVERRIDES`= ; a shortcut for dll overrides in winecfg, required for some games/mods to boot.

`DXVK_HUD=fps,dev info` ; An accurate fps counter, that should be used for most performance comparisons, for directX 8-11 games utilizing DXVK, and can show which direct x version the game/app is using.

`GALLIUM_HUD=simple,fps` ; This is an accurate fps counter for OPENGL graphics API games.


### Drives
Self explanatory, you can set a folder path to a drive in the container, to a folder of your choice, to run an exe from the container, or to copy files from and in the container.

### Audio driver
There lies Pulseaudio and ALSA; with pulseaudio being the most preferable one, as it works for 80% of games without audio stuttering and cracking. Do note, some emulators have the environmental variable `PULSE_LATENCY_MSEC` and this WILL conflict during the usage of ALSA, so make sure to delete it before using ALSA.


### Graphics 
To avoid wasting time troubleshooting for the simplest things; ALWAYS google up the name of your game and what graphics API it uses. 

# Wrappers
These are used to load your graphics drivers to run vulkan apps; It may contain various kinds of spoofs and support for Vulkan extensions that may not be present on the graphics driver. 
In most forks, 'Wrapper' is the default and most updated wrapper by pippeto-crypto, which is best for adreno users, and also allows for bcn emulation required for Mali and xclipse GPU users.
'Wrapper-v2', present in Cmod v13 & Gamenative is an unfinished and old wrapper that is almost 2 years old now.
'Wrapper-leegao'- Lee Gao's bionic wrapper for Xclipse and Mali gpus, quite old now.
'Wrapper-Gamenative' - The most updated wrapper for Mali and Xclispe GPU devices. allows Mali gpu users to load Dxvk 2.0+ and VKD3D (dx12) if their driver is new enough. not of note to adreno users.

# Graphics Drivers
for a6xx - [Amaral turnip](https://github.com/rickamaral94/Amaral-Adreno-Tools) and [StevenMXZ](https://github.com/StevenMXZ/Adreno-Tools-Drivers/releases)

for a7xx - [StevenMXZ](https://github.com/StevenMXZ/Adreno-Tools-Drivers/releases), [Winnative, specifically the P variant drivers, which keeps gpu frequencies and usage high, otherwise use StevenMXZ](https://github.com/nicholasx417/WinNative-Components/releases/tag/WinNative-Turnip)

for a8xx - [StevenMXZ](https://github.com/StevenMXZ/Adreno-Tools-Drivers/releases), [Whitebelyash](https://github.com/whitebelyash/AdrenoToolsDrivers/releases)


# Display Renderers; 
**OpenGL** - Deprecated in most forks

**Vulkan** - Offers better frame pacing, and negligible fps increase when toggling present mode to Mailbox.

**Surfaceflinger** - Offers better frame pacing than Vulkan; at the cost of compatibility.

**EGL** - Best for OpenGL games, offering the best FPS for opengl API games.

**DisplayX** - Better than surfaceFlinger, developed by pippeto crypto, offers vastly better frame pacing than any other implementations; However at the time of making this guide, the 'Bypass x11' option within DisplayX will give a 10-30% fps loss upon usage.

# OpenGL API games
In adreno, there is now 2 ways to run opengl games; using **zink**, which translates OpenGL to Vulkan, which the turnip drivers will run.
This approach is slow, starting from ludashi 4.0, the introduction of the **freedreno** driver increases performance by up to 3x.
Recommended settings for opengl : ***EGL+Freedreno, DisplayX+Freedreno***
For mali users, please note that <ins>opengl3.3+ API games will not work, and freedreno will not work, you are limited to zink for now.

# Vulkan API Games

These games are fairly simple to run, and does not need much for graphical configuration, as it is directly running vulkan.
**RDR2** - Requires VulkanRT to be installed in the container;
**DOOM(2016)** - Requires you to use Wrapper leegao in gamenative, while in winnative and Ludashi by StevenMXZ to have the present mode be anything else other than mailbox.

## DirectX Wrappers for DX Games ⭐

**DXVK** - Translates directX 8-11 API to vulkan, is very fast. For turnip users, it is recommended to use dxvk 2.4.1 for best performance, and newer dxvk's that have leegao's disable binary semaphores patches to retain performance on turnip; these mostly go by 'dxvk-pre-regress' or dxvk binsem.

**VKD3D** - Translates DirectX12 to Vulkan. Generally recommended to use the latest version; however if that fails, 2.14.1 is a safe and good fallback.
