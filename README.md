### Winlator-Bionic-101²-
Guide for new users to setup games using 
[Ludashi](https://github.com/StevenMXZ/Winlator-Ludashi), [Winnative](https://github.com/WinNative-Emu/WinNative), [Gamenative](https://github.com/utkarshdalal/GameNative/releases), and so on.

Mainly catered to adreno gpu users. Mali and Xclipse gpu users can still benefit from this guide a lot.

[Go here to get links of various components of relevance that are mentioned in this guide, they are mostly in .wcp format](https://github.com/qsh3525/Winlator-Bionic-101-/blob/main/components.md)
## Proton/wine

**Proton 9** is the original and default, and should be used first. 

**Proton-10arm64ec** should also be used as a default fallback; as most cutscenes and fmv's not working are fixed in this version.

**Proton 11 and above;** these versions can be used to gain performance in some games and provide more compatibility.


## Win components

These can stay mostly untouched, unless you're having audio issues, you can try switching xaudio, directmusic to native or builtin.


## Environmental variables

these are in some cases, crucial for some games to work, you can either find the variables in protondb or winehq, here are a few that may be of note. 

`WINEESYNC=1` ; May be needed to be turned off; `=0`, for some games to boot, but with very heavy performance repercussions.

`TU_DEBUG`= ; When using turnip, for mature GPUs like 6xx and 7xx series, you can disable gmem and  sysmem to enable autotuner on the turnip driver, which switches between both to improve stutter/fps in rare cases. For 8xx, some of the variables like sysmem are a must have, and some games require noubwc or nolrz to render properly.

`WINEDLLOVERRIDES`= ; a shortcut for dll overrides in winecfg, required for some games/mods to boot.

`DXVK_HUD=fps,dev info` ; An accurate fps counter, that should be used for most performance comparisons, for directX 8-11 games utilizing DXVK, and can show which direct x version the game/app is using.

`GALLIUM_HUD=simple,fps` ; This is an accurate fps counter for OPENGL graphics API games.


## Drives
Self explanatory, you can set a folder path to a drive in the container, to a folder of your choice, to run an exe from the container, or to copy files from and in the container.

## Audio driver
There lies Pulseaudio and ALSA; with pulseaudio being the most preferable one, as it works for 80% of games without audio stuttering and cracking. Do note, some emulators have the environmental variable `PULSE_LATENCY_MSEC` and this WILL conflict during the usage of ALSA, so make sure to delete it before using ALSA.


## Graphics 
To avoid wasting time troubleshooting for the simplest things; ALWAYS google up the name of your game and what graphics API it uses. 

# Wrappers
A very important component that is used to load your graphics driver to run vulkan apps, and spoofs extensions unsupported by original GPU driver for compatibility.

List of wrappers of relevance in forks:
*'Wrapper' ⭐* - The most up to date wrapper, good for adreno gpu devices, also supports bcn texture decomp needed for Xclispe and Mali GPUs.

*Wrapper-v2* - Old and unfinished, found in [cmod-v13](https://github.com/coffincolors/winlator/releases/tag/cmod_v13.1) and [Gamenative](https://github.com/utkarshdalal/GameNative/releases), Shouldn't be used, as 'wrapper' performs better and have more fixes.

*Wrapper-leegao* - found in gamenative, it is (Lee gao's bionic wrapper)[https://github.com/leegao/bionic-vulkan-wrapper/releases/tag/v0.0.5r5], for mali and Xclipse GPUs, better alternative exists by now.

*Wrapper-Gamenative ⭐* - found in many forks now; best wrapper for Mali and xclipse, contains new improvements by Lee gao regarding running dxvk 2.0+ and VKD3D on new Mali driver blobs. ***Not recommended for adreno users.***

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
in winlator bionic there's 2 options

**zink** - default for most forks before [ludashi 4.0 EA](https://github.com/StevenMXZ/Winlator-Ludashi/releases/tag/v4.0); it is automatically used on adreno and Mali gpus, it is an opengl to vulkan translation layer. Please note, *Mali gpu users are not able to run games that are above opengl 3.3 API*.

**Freedreno KGSL** - introduced in [ludashi 4.0 EA](https://github.com/StevenMXZ/Winlator-Ludashi/releases/tag/v4.0) it is the mesa opengl driver for adreno gpu, provides 3x more performance than zink in many cases, but it is still quite new implementation, and may have compatibility problems.

*Recommended setup for adreno:*
*EGL+Freedreno*; *DisplayX perf mode+Freedreno*


# Vulkan API Games

These games are fairly simple to run, and does not need much for graphical configuration, as it is directly running vulkan.

**RDR2** - Requires VulkanRT to be installed in the container;

**DOOM(2016)** - Requires you to use Wrapper leegao in gamenative, while in winnative and Ludashi by StevenMXZ to have the present mode be anything else other than mailbox.

# Relevant DirectX Wrappers for DX Games ⭐

**DXVK** - Translates DX8-11 to Vulkan
*Recommended for adreno: 2.4.1.1 or newer DXVK Tagged with 'binsem' or 'pre-regress', without this patch, newer dxvk have a big performance regression that occured after 2.4.1 versions.*

*Recommended for Mali and PowerVR: DXVK Sarek, or dxvk 2.7.1 stable if you have a new Mali driver blob, using wrapper-gamenative.*

 **VKD3D** - Translates DX12 to Vulkan
 *Recommended for adreno: Latest VKD3D stable/nightly, 2.8 or 2.14.1 are safe fallbacks.*

*Recommended for Mali with new driver blob: VKD3D 2.14.1*

**D7VK** Translates DirectX 6/7 to Vulkan.
*How to use? Download [D7VK](https://github.com/WinterSnowfall/d7vk/releases), Copy the ddraw.dll into your game folder, and use `DXVK_HUD` to see if game is using D7VK.*


## CPU Translation and emulation ⭐⭐⭐⭐⭐
There's 2 CPU Translators, Fexcore on Proton/wine arm64ec & Box64 on Proton/wine X86_64.
On proton arm64ec You have the option to use wowbox64 as the 32 bit emulator, which may perform better in some cases.
This guide will cover Fexcore, as it generally uses less ram and has less overhead and it's preset as it has lesser parameters and is easier to configure.

# TSO Behaviour on snapdragon chips;

A7xx gen 1 (A725 and A730,735) Mostly don't need TSO for many games.
A7XX gen 2 & 3 and 8xx gen 1 (740,750,830) Needs TSO for some games
A8xx gen 2 (sm8850) NEEDS TSO for ALL games to have a shot at running.
For SM8750 and SM8850 Users, when using tso, use this variable ``WRAPPER_DMAHEAP_CACHED=1`` to regain some performance.

# Fexcore Presets roundup on various kinds of game engines by @Tranquility

Tested on Snapdragon 8 gen 3, proton 9arm64ec Fex (2601, as a few games don't work with anything newer, such as Silksong, so 2601 is a bit more compatible) with the Extreme Preset or Extreme + TSO enabled. 
32-bit emulator should be FEXCore

The main differientiator is mainly just engines, so that's what I'll be using as a basic guide. Everything is using FEXCore unless stated otherwise.

Unreal Engine 1: (Unreal Tournament 1999, Deus Ex): Extreme
Unreal Engine 2: (Postal 2, Unreal Tournament 2004): Extreme
Unreal Engine 3 (Mortal Kombat X, A Hat in Time, Batman Arkham series): Extreme + TSO
Unreal Engine 4 (Final Fantasy VII Remake, Star Wars Jedi Fallen Order): Extreme
Unreal Engine 5 (Dragon Ball Sparking Zero, Palworld, Black Myth Wukong): Extreme
Unity: Depends on game, but most will require Extreme + TSO. Some outliers that can use the normal extreme preset are Hollow Knight and Hollow Knight Silksong. Best to keep it on Extreme + TSO unless you need more performance.
Godot: Extreme

Other engines:

4A (Metro Trilogy): Extreme + TSO
RE Engine: (Resident Evil Games, Devil May Cry games, Monster Hunter games, Pragmata): Extreme + TSO
Creation Engine (Fallout series, Elder Scrolls games): Extreme (although Fallout New Vegas, being the unstable mess that it is may need Extreme + TSO)
Source Engine (Half Life 2, Portal): Extreme

