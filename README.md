# ZMM_MyMusic Custom Cassette Pack (for Zen’s Music Mod)

Add a curated set of custom cassette tapes for **DayZ**. This pack plugs straight into **Zen’s Music Mod** so your tracks play in boomboxes, Walkmans, and vehicles - fully synced for everyone nearby.

> **Hard dependency:** **Zen’s Music Mod**. The music system, boomboxes, Walkman, vehicle playback, and client sync all come from Zen’s mod and must be installed on **both client and server**.

---

## What’s inside

- New cassette items configured to work with Zen’s playback system  
- Ready-to-use classnames for static boombox rotations and (optional) world loot spawns  
- Clean examples and snippets for quick integration

Zen’s base mod provides: cassettes, boombox, Walkman, vehicle playback, and networked song sync.

---

## Requirements

- **Zen’s Music Mod** installed and loaded on **client + server**
- Load this pack **after** Zen in your server’s mod list (or declare `ZenMusicBase` in `requiredAddons[]` - see Dev Notes)

---

## Install (server)

1. Install **Zen’s Music Mod** on the server (players must have it too).
2. Add this **ZMM_MyMusic Cassette Pack** to your server’s mods (@MyMusic).
3. Copy this pack’s `.bikey` into your server `keys/` folder (standard DayZ mod step).
4. Start the server once. Zen will auto-create its music config JSON in your **profile** folder, typically:
   ```
   <ServerProfile>/Zenarchist/ZenMusic.json
   ```
   (Some versions may use `ZenMusicConfig.json` — use the file Zen generates.)
5. (Optional) Add our cassette classnames to Zen’s JSON so static boomboxes can auto-play them.
6. (Optional) If you want tapes to spawn as loot, add the `types.xml` snippet below.

---

## Quick Start (static boombox playlist)

Edit your Zen music JSON (in `<ServerProfile>/Zenarchist/`):

```json
{
    "CONFIG_VERSION": "1",
    "AllowCarInventory": 1,
    "StaticBoomboxAutoPlay": 1,
    "StaticBoomboxSongs": [
        "Zen_Cassette_S1",
        "Zen_Cassette_S2",
        "Zen_Cassette_S3"
        // add more of our classnames here
    ]
}
```

- **`AllowCarInventory`**: lets players open inventory in vehicles to swap tapes  
- **`StaticBoomboxSongs`**: playlist of cassette **classnames** boomboxes can play

> Tip: Zen supports invisible static boomboxes for ambient music at bases/traders.

---

## (Optional) World loot spawns

If you want our tapes to appear as loot, add entries like this to
`mpmissions/<your_mission>/db/types.xml`.  
*(Example uses your preferred indentation style: 4 spaces for `<type>` line, 8 inside.)*

```xml
    <type name="Zen_Cassette_S1">
        <nominal>5</nominal>
        <lifetime>3888000</lifetime>
        <restock>0</restock>
        <min>2</min>
        <quantmin>-1</quantmin>
        <quantmax>-1</quantmax>
        <cost>100</cost>
        <flags count_in_cargo="0" count_in_hoarder="0" count_in_map="1" count_in_player="0" crafted="0" deloot="0" />
        <category name="tools" />
    </type>
```
Repeat for each cassette classname in this pack.

---

## Dev Notes (for mod pack maintainers)

If you’re merging this into your own pack or you publish a derivative, ensure your `config.cpp` declares a dependency on Zen so the audio system initializes first:

```cpp
class CfgPatches
{
    class Freethought_Cassettes
    {
        requiredVersion = 0.1;
        requiredAddons[] =
        {
            "DZ_Data",
            "ZenMusicBase"   // Ensure Zen’s music base loads before this pack
        };
        units[] = {};
        weapons[] = {};
    };
};
```

**About `playSeconds`:**  
Each cassette class defines `playSeconds` and it **must equal the actual `.ogg` duration**. Zen uses this for cross-client sync (wrong lengths cause cut-offs, overlap, or drift).

**Minimal example (trimmed):**
```cpp
class CfgVehicles
{
    class Zen_Cassette_Base;
    class Zen_Cassette_S1 : Zen_Cassette_Base
    {
        scope = 2;
        displayName = "My cool song";
        descriptionShort = "A cool song from the past few decades";
        playSeconds = 192; // exact .ogg length for sync
        // isMusic = 1;     // optional: affects music/voice settings handling
        // copyrighted = 0; // optional: streamer-friendly flag if supported
    };
};

class CfgSoundShaders
{
    class Zen_Cassette_SoundShader_Base;
    class Zen_Cassette_S1_SoundShader : Zen_Cassette_SoundShader_Base
    {
        samples[] = { { "FreethoughtMusic\data\sounds\s1", 1 } };
    };
};

class CfgSoundSets
{
    class Zen_Cassette_S1_SoundSet
    {
        soundShaders[] = { "Zen_Cassette_S1_SoundShader" };
    };
};
```

---

## Known Classnames (examples)

Replace or extend with your exact set:

```
Zen_Cassette_S1
Zen_Cassette_S2
Zen_Cassette_S3
Zen_Cassette_S4
Zen_Cassette_S5
...
```

---

## Troubleshooting

- **No music / items missing**  
  Make sure **Zen’s Music Mod** is loaded on **client + server** and this pack loads **after** Zen.

- **Static boombox is silent**  
  Confirm `StaticBoomboxSongs` includes valid cassette **classnames** from this pack.

- **Desync, early cut-off, or looping issues**  
  Set each cassette’s `playSeconds` to the exact `.ogg` duration.

- **Config file not found**  
  Start the server once; Zen auto-creates its JSON in `<ServerProfile>/Zenarchist/`.

- **.bikey signature errors**  
  Copy this pack’s `.bikey` into the server’s `keys/` folder.

---

## Credits & License

- **Zenarchist** — author of **Zen’s Music Mod** (dependency) and original playback system.  
- **ZMM_MyMusic** — cassette content, config, and integration for this pack.

This cassette pack is released under **MIT** (recommended).  
> *Note:* Zen’s mod remains under its own license; please respect the upstream license and attribution requirements.

---

## Helpful Links

- Zen’s Music Mod — Steam Workshop (search “Zen’s Music Mod”)  
- Zen’s documentation/wiki for music config, cassettes, boomboxes, and vehicle playback

*NC OSL‑A 1.0 © William Furtado. See `LICENSE`.*

DOWNLOAD HERE: https://drive.google.com/file/d/1lRQUsjq438-_Env6EEXaJjpbsUEg-UOi
