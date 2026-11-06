---
title: "KAOSAMPLE v0.31.1 User Guide"
subtitle: "A feature layer for the stock MPC Sample app (firmware 1.3.0.12)"
author: "Omar Huertas LLC / kaosbeatz"
---

# KAOSAMPLE v0.31.1 User Guide

**Tester beta. Private copy, please do not share or repost.**

Where this guide says **(check on device)**, the written notes for this build did not settle the exact button, label or behavior. Look at the screen, and tell Omar what you see.

---

# 1. What it is, safety notes and the beta warning

## 1.1 What KAOSAMPLE is

KAOSAMPLE adds tools on top of the stock Akai MPC Sample app. Everything the MPC does today still works. KAOSAMPLE adds new pages, new tools and its own master processing (EQ, Loudness, Color).

It runs from the SD card and lives in the MPC's memory only. It does not change anything inside the MPC itself. Take the card out, or delete the `autorun` folder from the card, and your MPC is 100% stock again.

Made by Omar Huertas LLC / kaosbeatz. It uses its own code and its own artwork.

## 1.2 What you need

| Need | Detail |
|---|---|
| MPC Sample | Firmware **1.3.0.12** exactly. On any other firmware KAOSAMPLE turns itself off and the MPC runs stock. |
| SD card | In the MPC, with free space. Some tools write big files (stems, templates, loops). |
| A computer | To copy the `autorun` folder onto the card and to read the logs if something goes wrong. |
| A MIDI keyboard (optional) | Only for the "Keys" mode in Perform. |

## 1.3 Beta warning (please read)

* **This is a beta. It has not been fully tested on a real MPC. It may crash.**
* The honest test status of each tool is in the table below. Everything from v0.21 up was tested in an emulator (a computer simulation of the MPC) and **has not run on a real unit yet**. Your testing on the real machine is the point of this build.
* **Back up your SD card first.** Copy the whole card to your computer before you install.
* **Crash fallback:** if KAOSAMPLE crashes twice within 2 minutes, it gives up and starts the stock app on its own. You will see the normal MPC with none of the KAOSAMPLE tools. Wait a few minutes and restart the MPC to try again.
* **Lost project?** KAOSAMPLE saves a copy of your project every 5 minutes into `KAOSAMPLE/snapshots` on the card. See chapter 4.5 (Snapshots and recovery).

### Test status by tool

| Tool | Version | Status |
|---|---|---|
| Master EQ and spectrum, overwrite-record fix | 0.15 | Tested on hardware |
| Undo history, snapshots, system monitor, explode, load kit from card | 0.16 | Tested on hardware |
| Step edit (copy, paste, bar menu, zoom) | 0.17 | Tested on hardware |
| Track tools (solo, rename, move, delete, mute scenes) | 0.18 | Tested on hardware |
| Sample tools (chop, extract, reverse, tail, fade) | 0.19 | Tested on hardware |
| Pad sound editing | 0.20 | Tested on hardware |
| Pad inserts (FX) and Loudness | 0.21 | Emulator only |
| Stems and Song to Seq | 0.22 | Emulator only |
| Auto-kit | 0.24 | Emulator only |
| Groove | 0.25 | Emulator only |
| Retro Rec, tap tempo, Save version | 0.26 | Emulator only |
| Clean up (purge, mono) | 0.27.1 | Emulator only |
| Perform (scale, chord, arp, Keys) | 0.28 | Emulator only |
| Looper (Loop Rec) | 0.29 | Emulator only |
| Files and templates | 0.30 / 0.30.1 | Emulator only |
| Color, Duck, Reference A/B | 0.31 | Emulator only |

## 1.4 Safety notes

* **Back up the card first** (see above). Back up your important projects to your computer as well.
* **Do not rename or edit** the files in the `autorun` folder. The MPC checks them.
* **Delete track cannot be undone.** Use Snapshots to get a deleted track back.
* **Record a sample = no undo yet.** Recording a sample cannot be undone by KAOSAMPLE.
* **Stock insert effects on tracks and master do nothing** on this unit (a limit of the MPC Sample, not a bug). Use KAOSAMPLE's own master EQ, Loudness and Color instead. Insert effects on **pads** do work.
* **Two quick restarts = stock app.** KAOSAMPLE restarts the MPC app itself for a few jobs (restoring a snapshot, New from template). If the app exits twice less than 2 minutes apart, KAOSAMPLE switches itself off and the stock app runs. KAOSAMPLE refuses those jobs until the app has been up for about 2 minutes.
* **Some data lives on the card, not in the project.** Slice sets, mute scenes, recorded mutes, grooves and chord sets are kept on the card, not inside the project file. If you move a project to another card, they do not come along.

---

# 2. Install, update and uninstall

## 2.1 Install

1. Power off the MPC. Take the SD card out and put it in your computer.
2. **Back up the card** (copy everything to a folder on your computer).
3. Copy the **`autorun`** folder from the zip to the **top level** of the card. The card should then have:

   ```
   /autorun/autorun.json
   /autorun/autorun.sh
   /autorun/autorun.check
   /autorun/kaos-run.sh
   /autorun/kaos-mem.sh
   /autorun/kaoslayer.so
   /autorun/kaosplash
   ```

   Do not rename or edit any file.
4. Eject the card safely and put it in the MPC.
5. Turn the MPC on. It starts KAOSAMPLE by itself. You will see the KAOSAMPLE splash, then the normal MPC screen.
6. A **`KAOSAMPLE`** folder appears on the card. Your logs and your KAOSAMPLE files live there.

## 2.2 Check that it is running

* You saw the KAOSAMPLE splash at start-up.
* **SHIFT + SCREEN 3** opens the KAOSAMPLE menu (a grid of tiles).
* A `KAOSAMPLE` folder exists on the card.

If you see only the normal MPC and no KAOSAMPLE menu, see chapter 6 (Troubleshooting).

## 2.3 Update to a new build

The tester notes only describe a fresh install. The simple way to update is the same as installing:

1. Power off the MPC and put the card in your computer.
2. Delete the old `autorun` folder from the card.
3. Copy the new `autorun` folder to the top of the card.
4. Leave the `KAOSAMPLE` folder alone. It holds your snapshots, slices, grooves, templates and settings.
5. Put the card back and start the MPC.

(check on device) that your settings and snapshots are still there after the first start of a new build.

## 2.4 Uninstall

* Delete the **`autorun`** folder from the card, **or** just start the MPC without the card.
* Your MPC is stock again.
* You can keep or delete the `KAOSAMPLE` folder. It is only your data and logs.

---

# 3. Getting around

## 3.1 The controls this guide uses

| Name in this guide | What it is |
|---|---|
| **K1, K2, K3** | The three knobs. On a KAOSAMPLE page they edit the three values of the selected row. |
| **S1, S2, S3** | The **SCREEN 1, SCREEN 2, SCREEN 3** buttons. On a KAOSAMPLE page they run the actions named in the top bar and in the hint line. |
| **DATA** | The data wheel. **Turn** it to move up and down. **Push** it to press it in. |
| **ERASE** | The ERASE button. |
| **SHIFT** | The SHIFT button, held while you press another control. |
| **Pads and PAD BANK** | The 16 pads. PAD BANK A to H gives you 128 pads. |
| **PLUS / MINUS** | The PLUS and MINUS buttons. |

## 3.2 Opening the KAOSAMPLE menu

Press **SHIFT + SCREEN 3**. A grid of tiles opens. Pick a tile and open it. (check on device) which controls move the highlight and open a tile; the notes do not spell this out.

The menus are linked like this:

1. The first menu, titled **KAOSAMPLE**.
2. **KAOS TOOLS** (the second menu). The tester notes say to open "KAOS TOOLS" from the menu, but the first menu has no tile with that name, so (check on device) which tile or button opens KAOS TOOLS.
3. **KAOS TOOLS 2** (the third menu). Open it with the **More** tile on KAOS TOOLS. The **Back** tile on KAOS TOOLS 2 returns.

In short: **SHIFT + SCREEN 3 > KAOS TOOLS > More > KAOS TOOLS 2**.

### First menu: KAOSAMPLE

Mixer, Pad Mixer, Tracks, Track Mute, Sequences, Song, Track View, Pad View, Record, Theme, Meter, Header, About, Close, and **More** (opens KAOS TOOLS). The old **iPad** tile was removed in v0.31.1: it only showed an address and did nothing.

**Four tiles are switches, not pages.** Push one and it flips at once; the menu stays open. Each switch shows a **1.5 second message** in the middle of the screen, and a switch that is **On** is drawn with a bright border and a green marker in the tile corner. The big info box at the bottom right of the menu shows the state of the tile you are on.

| Switch | What it does | Message |
|---|---|---|
| **Record** | Chooses what a plain **SEQ REC** press does. **Overwrite** (default): REC erases the old notes the loop passes over, and **SHIFT + REC** adds without erasing. **Overdub**: REC adds without erasing, and **SHIFT + REC** erases. The setting is read at the next REC press. | "Record: Overdub" or "Record: Overwrite" |
| **Theme** | Each push steps to the next colour theme (Stock, MPC60, MPC3000, MPC2000XL, MPC1000, MPC Live). It tints the whole screen, the stock screens too. | "Theme" and the theme name |
| **Meter** | A small output level bar at the bottom right of every screen (green, red near the top; it holds the peak for about 0.3 s). It follows the MPC's main output. | "Meter: On" or "Meter: Off" |
| **Header** | A one-line strip "S01 T01 A01" (sequence, track, pad) under the tabs of the stock screens. It is only drawn where that strip is empty, and only when the project has at least one track. Default is On. | "Header: On" or "Header: Off" (when the project has no tracks: "shows when a project has tracks") |

These four switches are not remembered after a restart (Header starts On, the rest start Off or Overwrite).

This guide covers **Track View** and **Pad View** (4.6), **Tracks** and **Track Mute** (4.7), **Mixer** (explode, 4.8) and **About** (monitor, 4.20). The others (Pad Mixer, Sequences, Song) are not described in the notes for this build, so they are not covered here.

### KAOS TOOLS (second menu)

| Tile | What it opens | Chapter |
|---|---|---|
| Master | (check on device) | |
| Pad Sound | Envelopes, filter, choke, layers for one pad | 4.10 |
| Seq Ops | Sequence tools, includes Explode | 4.8 |
| MIDI | MIDI page (not covered) | |
| Looper | The older looper page that drives the stock looper | 4.18 |
| Stock screens | Shortcuts to stock MPC screens (not covered) | |
| Programs | Programs page (not covered) | |
| EQ Spectrum | Master EQ and spectrum | 4.1 |
| Undo History | Named list of what you did | 4.4 |
| Snapshots | Restore a saved project copy | 4.5 |
| FX | Pad insert effects | 4.10 |
| Loudness | Master loudness | 4.2 |
| Tracks Pro | Solo, rename, move, delete | 4.7 |
| Mute Scenes | Save and recall mute patterns | 4.7 |
| Sample Tools | Chop, extract, reverse, tail, fade | 4.9 |
| More | Opens KAOS TOOLS 2 | |

### KAOS TOOLS 2 (third menu)

| Tile | What it opens | Chapter |
|---|---|---|
| Stems | Export every track and the master as WAV files | 4.11 |
| Song to Seq | Turn a song into one sequence | 4.12 |
| Groove | Swing, humanize and groove templates | 4.14 |
| Auto-kit | Build a 16-pad kit automatically | 4.13 |
| Retro Rec | Grab what you just played; tap tempo; Save version | 4.15 |
| Perform | Scales, chords, arp, Keys | 4.17 |
| Clean Up | Purge unused samples, make mono | 4.16 |
| Loop Rec | The new multi-pass looper | 4.18 |
| Files | Templates, new folders, random names, browser jump | 4.19 |
| Color | Tape and vinyl color, duck, reference A/B | 4.3 |
| Back | Back to KAOS TOOLS | |

The two menus have some empty tiles. They do nothing.

## 3.3 Page conventions: K1-K3, S1-S3, DATA and ERASE

Most KAOSAMPLE pages (Color, Perform, Groove, Retro Rec, Clean Up, Files, Looper, Auto-kit, Pad Sound) follow the same habits.

| Control | What it does on a KAOSAMPLE page |
|---|---|
| **DATA turn** | Picks the **row** in the list on the page. |
| **K1, K2, K3** | Edit the **three values of the selected row**. The names of K1, K2, K3 for that row show at the bottom of the screen. |
| **S1, S2, S3** | Run actions. The names show in the top bar (left, middle, right) and in the hint line. They change with the row. |
| **DATA push** | Runs the row's main action (load, run, open a menu), when the page has one. |
| **ERASE** | Clears or cancels something small. See each page. |
| **ERASE twice** ("ERASE x2") | Confirms a bigger clear or delete. Press it again within a few seconds (about 4 seconds on Tracks and Looper). The first press shows a message such as "ERASE again". |
| **SHIFT + DATA turn** | On many pages, picks the **previous or next track**. On Pad Sound it picks the previous or next pad. |
| **SHIFT + ERASE** | A reset on several pages (EQ, Loudness, Color, Name editor). On the grids and mixer it is **Explode**. |
| **Pads** | Keep playing on every page. A few pages also use pads as a keyboard or selector. |

Good to know:

* **Knobs jump.** On Color and Loudness the knobs are absolute: the value jumps to where the knob is.
* **Pad Sound has "soft takeover"**: after you change row or pad, the knob must come close to the shown value before it takes over. You see the message "Knob: move to the shown value to pick it up".
* **A message line** appears for a few seconds at the bottom of the page. Read it, because refusals are explained there.
* **Doing two-press things.** "S2 twice" or "S3 twice" means press the same button twice within the time shown on screen (6 seconds for Purge, 5 seconds for Snapshots).

## 3.4 The SHIFT table

Hold **SHIFT** on a KAOSAMPLE page and an overlay lists the SHIFT combos for that page. It disappears when you let go. It never delays or blocks a combo. It is the quickest way to check a control.

### Everywhere

| Combo | Action |
|---|---|
| SHIFT + SCREEN 3 | Open the KAOSAMPLE menu |
| SHIFT + MINUS | Undo (stock) |
| SHIFT + PLUS | Redo (stock) |
| SHIFT + SAMPLE RECORD | Retro Rec: drop what you just played into the current track (chapter 4.15) |
| SHIFT + TAP TEMPO | Tap tempo (plain TAP TEMPO is still the stock button) |
| PAD FX, pressed twice | Mixer pages (tabs: SCREEN 1 Pad Mixer, SCREEN 2 Channel Mixer, SCREEN 3 track and pad grid) |
| PAD BANK, hold | Tracks grid (a pad selects that track) |
| SHIFT + PAD BANK, hold | Track Mute grid (a pad toggles mute for that track) |
| SHIFT + PAD BANK, tap | Create a new track |
| PAD BANK or PAD FX, single tap | The stock function, as normal |

The "PAD FX x2", "PAD BANK" and "SHIFT + PAD BANK" combos copy habits Omar reported from other MPC add-ons so your muscle memory carries over.

### By page

| Page | SHIFT combos |
|---|---|
| Mixer, Track View, Pad View, Seq Ops, Tracks Pro | SHIFT + ERASE = Explode the current track |
| Pad Mixer | SHIFT + pad = mute that pad |
| Track View, Pad View | SHIFT + DATA push = select the whole row. SHIFT + DATA turn = move the row. SHIFT + K1 / K2 = nudge / length in steps of 1/16. SHIFT + CHOP = Cut. SHIFT + LOOP = Duplicate |
| EQ Spectrum | SHIFT + ERASE = reset EQ flat (all gains 0, all kills off) |
| Loudness | SHIFT + ERASE = turn Loudness off |
| Color | SHIFT + ERASE = reset Color, Duck and Trim to defaults (undoable) |
| Tracks Pro | SHIFT + DATA turn = move the track. SHIFT + pad = mute that track |
| Name editor | SHIFT + pad = lower case. SHIFT + ERASE = clear the name |
| Mute Scenes | SHIFT + pad (bottom two rows) = store scene. SHIFT + pad (top two rows) = solo. SHIFT + ERASE = clear last scene |
| Pad Sound | SHIFT + DATA turn = previous or next pad |
| Slice view | SHIFT + DATA turn = destination start pad. SHIFT + pad = extract the selected slice to that pad |
| FX list | SHIFT + ERASE = clear the name filter |
| Groove, Retro Rec, Clean Up, Looper | SHIFT + DATA turn = previous or next track |
| Files (in stock Sample Select and Project Select) | SHIFT + DATA turn = jump 5, 10 or 20 rows |
| Programs | SHIFT + DATA push = Save kits (not covered here) |

---

# 4. The features

Each feature has the same four parts: **What it does**, **Step by step**, **Controls** and **Tips and limits**. Test status for each is in chapter 1.3.

---

## 4.1 Master EQ, kill EQ and spectrum

### What it does

A 3-band equalizer on the **master output**: low, mid and high, each from minus 12 to plus 12 dB. Each band also has a **kill** switch that silences just that band (handy for DJ-style drops). A 24-band spectrum analyzer shows what your mix is doing. Settings are saved on the card and come back after a restart.

### Step by step

1. **SHIFT + SCREEN 3 > KAOS TOOLS > EQ Spectrum.**
2. Play your sequence.
3. Move the band controls to shape the master. Watch the spectrum.
4. Use a band's kill to drop it out, press again to bring it back.
5. **SHIFT + ERASE** resets everything flat.

### Controls

| Control | Action |
|---|---|
| K1, K2, K3 | Band gains (low, mid, high). (check on device) which knob is which band |
| Kill buttons | One kill per band. (check on device) which buttons |
| SHIFT + ERASE | Reset EQ flat: all gains 0, all kills off |

### Tips and limits

* The EQ sits on the **master only**. There is **no per-track kill EQ**: the MPC Sample does not give KAOSAMPLE separate audio for each track.
* The EQ costs very little processing (well under 1% of the audio time budget in the emulator).
* The EQ is not in the stock undo list.

---

## 4.2 Loudness

### What it does

A mastering chain on the master output, after the EQ. It has:

* a **3-band compressor** (low, mid, high),
* **saturation** (warmth and drive),
* a **lookahead brickwall limiter** with the ceiling at **-0.3 dBFS**,
* presets **Off, Glue, Punch, Loud, Max** and a one-knob **Loud** macro,
* a **level-matched A/B** so you compare the processed sound fairly (the unprocessed side is not made quieter than it is; the processed side is never boosted past the ceiling).

Settings are saved on the card.

### Step by step

1. **SHIFT + SCREEN 3 > KAOS TOOLS > Loudness.**
2. Play your sequence.
3. Press **S3** to step through presets, or pick the **Main** row and turn **K1** (Preset) or **K2** (Loud).
4. Press **S1** to flip between processed and bypassed. Press **S2** to turn level match on, so both sound equally loud.
5. Watch the gain-reduction bars (Low, Mid, High and the limiter) and the output peak against the ceiling.
6. **SHIFT + ERASE** turns Loudness off.

### Controls

DATA turn picks the row (check on device). K1, K2, K3 edit the row's three values.

| Row | K1 | K2 | K3 |
|---|---|---|---|
| Main | Preset | Loud (macro) | Push (extra gain into the limiter) |
| Low | Threshold | Ratio | Makeup |
| Mid | Threshold | Ratio | Makeup |
| High | Threshold | Ratio | Makeup |
| Time | Attack | Release | Detect (RMS or peak) |
| Sat / AB | Drive | Sat | LvlMatch |

| Button | Action |
|---|---|
| S1 | A/B: processed or bypassed |
| S2 | Level match on or off |
| S3 | Next preset |
| SHIFT + ERASE | Loudness off |

### Tips and limits

* Presets get louder in this order: Glue (gentle), Punch, Loud, Max. Max is not much louder than Loud, because the ceiling is already reached; the extra push goes into limiting.
* The ceiling holds. In the emulator test no sample went over -0.3 dBFS in a 10-minute stress run.
* Bypass is a short crossfade, so switching does not click.
* Loudness is not in the stock undo list.
* Offline stem renders skip the EQ and Loudness. See chapter 4.11.

---

## 4.3 Color, Duck and Reference A/B

All three live on one page: **KAOS TOOLS 2 > Color**.

### What it does

**Color** gives the master output tape or vinyl character with one **Mix** knob.

* **Tape:** saturation, head bump (low end warmth), high roll-off, wow and flutter (slow and fast pitch wobble).
* **Vinyl:** warmth, mono below about 120 Hz (a tighter low end), and crackle (random clicks).
* **Mix at 0 is exactly off and costs nothing.** Mix 0 means the sound is bit-for-bit unchanged.

**Duck** (sidechain) dips the output when a chosen track or pad plays, like a kick pumping a pad. Depth up to 24 dB, attack 0.1 to 50 ms, release 10 to 1000 ms, three curve shapes.

**Reference A/B** plays a WAV from your own music next to your mix, **matched by loudness** (a LUFS-style measure) so louder does not fool you into thinking it sounds better. A trim knob lets you nudge the match.

Where it sits in the signal path: Color goes before Loudness (so the limiter ceiling still holds). Duck and Reference go after the master volume.

### Step by step: Color

1. Open **KAOS TOOLS 2 > Color**.
2. Pick the **Master** row. Turn **K2** up (upper half) to turn Tape on. Turn **K3** up to turn Vinyl on.
3. Raise **K1 (Mix)** until you hear it.
4. Pick the **Tape**, **Wow/Flut** or **Vinyl** rows and set the amounts.
5. **S2** undoes your last change. **S3** redoes it.

### Step by step: Duck

1. Pick the **Duck src** row. Set **K1 (Source)** to **Track** (any note of that track ducks) or **Pad** (one pad on that track). Then choose the track on **K2** and the pad on **K3**.
2. Pick the **Duck env** row. Set **Depth, Attack, Release**. Pick the curve on the **Shape** row.
3. Push **DATA** on a duck row to fire a test duck so you can hear it without playing.
4. Watch the "duck" meter on the page.

### Step by step: Reference A/B

1. Copy WAV files into **`KAOSAMPLE/refs`** on the card (do this with the card in your computer).
2. Open **Color** and pick the **Ref** row.
3. Turn **K1** to choose a file. Push **DATA** to load it. The page says "loading" and then "ready".
4. Press **S1** to switch between **A: MPC** (your mix) and **B: REF** (the reference).
5. If the match sounds off, nudge **K2 (Trim)** between -6 and +6 dB.

### Controls

| Row | K1 | K2 | K3 |
|---|---|---|---|
| Master | Mix | Tape on / off | Vinyl on / off |
| Tape | Sat | Bump | HF (high roll-off) |
| Wow/Flut | Wow | Flutter | - |
| Vinyl | Warmth | Crackle | Mono below 120 Hz on / off |
| Duck src | Source (Off, Track, Pad) | Track | Pad |
| Duck env | Depth | Attack | Release |
| Shape | Linear, Exp, S-curve | - | - |
| Ref | File | Trim | - |

For on / off knobs, the upper half of the knob is ON.

| Button | Action |
|---|---|
| S1 | A/B: your mix or the reference |
| S2 | Undo |
| S3 | Redo |
| DATA turn | Pick the row |
| DATA push | On the Ref row: load the file. On a duck row: test duck |
| SHIFT + ERASE | Reset Color, Duck and Trim (A/B returns to your mix). It is undoable |

### Tips and limits

* **Setting is saved** and comes back after a restart. The **reference file name is remembered but never loaded on its own**, and A/B **always starts on your mix**.
* **Undo/redo (32 steps) is lost at restart.** Only the current state is saved.
* Duck sources are the duck track's notes and live pad hits on that track. It does **not yet** duck from other tracks, the arp or MIDI input.
* Duck from the sequence reacts within about 2.6 ms of the true note time. Live pad hits react within 2.9 ms.
* The loudness meter is **LUFS-style, not a certified meter.** It needs a few seconds of music to settle. Until then the reference plays at the trim only.
* If the reference is very loud and would clip, its level is capped and the page says **"peak cap"**. The match is then not exact.
* If you move the master volume, the matched reference follows slowly.
* The reference list shows up to 48 files. WAV only: 16, 24 or 32-bit PCM, or 32-bit float. Mono or stereo, any sample rate.
* Fast knob sweeps on Color can **zipper** slightly. High Wow or Flutter with a middle Mix can comb a little.
* Stem renders include Color but **not** the duck or the reference (chapter 4.11).

---

## 4.4 Undo, redo and the history list

### What it does

* A **popup** shows on every page when you undo or redo, for example "Undo 2/3", with the name of the step.
* The **Undo History** page is a named list of your steps. Newest is on top. You can jump to any step.
* Nearly every KAOSAMPLE edit is a normal stock undo step. Several multi-part jobs (paste, quantize, a bar insert, a groove apply, an auto-kit) are **one** undo step.

### Step by step

1. Press **SHIFT + MINUS** to undo and **SHIFT + PLUS** to redo, on any page. The popup shows where you are.
2. For the list, open **KAOS TOOLS > Undo History**.
3. Turn **DATA** to scroll. The green bar is the last step applied. Dim rows are undone.
4. Push **DATA** (or press **S2**) to go to that step.

### Controls

| Control | Action |
|---|---|
| SHIFT + MINUS | Undo |
| SHIFT + PLUS | Redo |
| DATA turn (History page) | Scroll the list |
| DATA push or S2 (History page) | Go to the selected step |
| S1 (History page) | Undo one step |
| S3 (History page) | Redo one step |

### Tips and limits

* **Track create cannot be undone** by the stock history. KAOSAMPLE refuses a redo that would cross it and shows "Redo not possible".
* **Delete track cannot be undone.** KAOSAMPLE refuses ("Cannot undo - Delete Track: use Snapshots").
* **Explode** is only partly undoable (the copied notes go back, the empty tracks stay).
* **EQ, Loudness, Color and Duck settings are not in this list.** Color has its own undo (chapter 4.3).
* **Recording a sample cannot be undone** yet.
* The stock history holds about 512 steps. Old steps scroll off.
* Insert effects: Undo does not reliably remove the effect. Use ERASE on the insert row (chapter 4.10).
* If the stock app shows a red "Command failed" box, press SCREEN 1, 2 or 3 to clear it. This can happen if you redo past a track create with SHIFT + PLUS.

---

## 4.5 Snapshots and recovery

### What it does

Every 5 minutes KAOSAMPLE saves a **copy of your project to the card** in `KAOSAMPLE/snapshots`. It keeps the newest **30**. If the MPC's own storage goes bad, a project is lost, or you delete a track, you can bring a snapshot back **from the MPC itself**.

### Step by step: restore a snapshot

1. Open **KAOS TOOLS > Snapshots**.
2. The list shows snapshots newest first, with date, time, number and size. The top shows how many seconds ago the live project was saved.
3. Turn **DATA** to pick one.
4. Push **DATA** (or press **S3**) once. The page says "Press again: restore #N".
5. Push **DATA** (or **S3**) **again within 5 seconds** to confirm.
6. The screen goes black for about **35 to 40 seconds** while the app restarts. Wait. Your restored project loads.

Before it restores, KAOSAMPLE saves a **safety copy** of your current project in the same folder (the name ends in "-pre"), so you can go back.

### Controls

| Control | Action |
|---|---|
| DATA turn | Scroll snapshots |
| DATA push or S3, twice within 5 s | Restore the selected snapshot |
| S1 | Refresh the list |

### Tips and limits

* The app must have been **running for at least about 2 minutes** (125 seconds). If not, the page says "wait N s". Two restarts less than 2 minutes apart make KAOSAMPLE fall back to the stock app.
* If a restored project's samples are missing, the stock app shows "Sample not found" with ABORT and RESOLVE. This is the stock message.
* A restore replaces the open project. Copies of your work are in the "pre" file.
* Snapshots are plain project files. You can also copy one off the card to your computer.

---

## 4.6 Step edit

### What it does

Edit notes on a grid, with copy, cut, paste, duplicate, a bar menu, zoom and a "stamp" for fast hi-hat rolls.

* **Track View**: tracks are the rows.
* **Pad View**: the pads of the current bank are the rows. Banks follow the stock PAD BANK, and SHIFT + DATA turn or K1 walks through all 16 pads of the bank. The sample name shows inside each row.

Open them from the first menu (**SHIFT + SCREEN 3**): **Track View** or **Pad View**.

### Step by step: edit notes

1. Open **Track View** or **Pad View**.
2. Turn **DATA** to move the cursor along the grid.
3. Hit a **pad** to add or remove a note at the cursor (toggle). Push **DATA** to select the note under the cursor.
4. With notes selected, use **K1** to nudge the time, **K2** for length and **K3** for velocity.
5. Press **16 LEVELS** to quantize the selection (to 1/16).
6. **ERASE** clears the selection.

### Step by step: copy, cut, paste

1. Select notes.
2. **CHOP** copies. **SHIFT + CHOP** cuts.
3. Move the cursor. **LOOP** pastes. **SHIFT + LOOP** duplicates right after the selection.

### Step by step: bar menu

1. Move the cursor into the bar you want.
2. Press **SAMPLE SELECT**. The menu lists: Copy bar, Paste bar (replace), Insert empty bar, Insert copied bar, Delete bar.
3. Turn **DATA** to pick. Push **DATA** to run it.
4. **ERASE** or **SAMPLE SELECT** closes the menu.

### Step by step: stamp a roll

1. Hold **NOTE REPEAT**.
2. Turn **K2** to set the rate (1/4, 1/8, 1/16, 1/32, 1/64). The page shows "Rpt 1/n".
3. Hit a pad. Notes of that pad fill from the cursor to the end of the bar at that rate.

### Controls

| Control | Action |
|---|---|
| DATA turn | Move the cursor |
| DATA push | Select the note under the cursor |
| SHIFT + DATA push | Select the whole row |
| SHIFT + DATA turn | Move the row (Pad View: pad row; Track View: track). The selection stays and can span tracks |
| Pad hit | Toggle a note at the cursor |
| K1 / K2 / K3 | Nudge / length / velocity of the selection |
| SHIFT + K1 / SHIFT + K2 | Nudge / length in steps of 1/16 |
| 16 LEVELS | Quantize the selection |
| ERASE | Clear the selection |
| CHOP | Copy |
| SHIFT + CHOP | Cut |
| LOOP | Paste at the cursor |
| SHIFT + LOOP | Duplicate after the selection |
| SAMPLE SELECT | Open or close the bar menu |
| PLUS / MINUS | Zoom in / out: 1/64, 1/32, 1/16, 1/8, 1/4, 1/2 and 1 bar. At the widest it shows 16 bars |
| NOTE REPEAT held + pad | Stamp notes |
| NOTE REPEAT held + K2 | Stamp rate |
| SHIFT + ERASE | Explode the current track (chapter 4.8) |

### Tips and limits

* **4/4 only.** The bar tools assume four beats to the bar. Quantize still snaps to 1/16, not the zoom column.
* Selection and clipboard hold up to **256 notes**. Bar tools refuse tracks with more than **4000 events**. The grid shows the first 512 events of a track.
* Paste skips a note that already exists at the same time and pitch, and drops notes past the sequence end. The popup says "some skipped".
* Most jobs are one undo step. A cut or other multi-note edit that was done before the grouping may take several presses.
* Pad hits recorded live show up on the grid after a short delay. A "ghost" note shows right at the hit and is replaced when the real note arrives.
* **Overwrite recording** now clears all old notes the loop passes over (it used to clear only one). Press REC a second time to punch out.

---

## 4.7 Track tools: solo, names, move, delete, mute scenes

### What it does

* **Solo** a track.
* **Rename** a track by typing on the pads.
* **Move** a track up or down the list.
* **Delete** a track.
* **Mute scenes**: save and recall up to 8 mute patterns per sequence.
* **Recorded mutes**: record mute changes into the sequence while you record, and play them back.
* **Paged track grids**: 16 tracks per page, so you can reach all 32 tracks.

### Step by step: Tracks Pro

1. **SHIFT + SCREEN 3 > KAOS TOOLS > Tracks Pro**. You see all tracks, with flags M (mute), S (solo), R (record).
2. Turn **DATA** to pick a track.
3. **S1** mutes, **S2** solos, **S3** renames.
4. **SHIFT + DATA turn** moves the track up or down.
5. **ERASE twice within 4 seconds** deletes the track. The first press shows "ERASE again: delete NAME".
6. Push **DATA** for a track menu: Rename, Move up, Move down, Move to top, Move to bottom, Delete, Clear all solo, Mute scenes, Close.

### Step by step: rename (name editor)

1. Press **S3** on Tracks Pro (or pick Rename in the menu).
2. Type on the pads. **PAD BANK A to D** gives four pages of 16 characters:
   * A: letters A to P
   * B: letters Q to Z and digits 0 to 5
   * C: 6 7 8 9 and the symbols `space - _ . + # & ' ! ( ) /`
   * D: the symbols `: , @ $ % * = ? < > [ ] ~ ^ | "`
3. **SHIFT + pad**, or **PAD BANK E to H**, types lower case.
4. **DATA turn** moves the cursor. **ERASE** is backspace. **SHIFT + ERASE** clears the name. **S2** types a space. **PLUS / MINUS** step the character under the cursor.
5. **S3** or **DATA push** accepts. **S1** cancels.
6. Names up to 20 characters. Duplicate names get a number ("Kick 2").

### Step by step: mute scenes

1. **KAOS TOOLS > Mute Scenes**. The page shows the 4x4 pads.
2. The **bottom two rows** are **scenes 1 to 8** for the current sequence. Hit a pad to **recall** a scene. **SHIFT + pad** **stores** the current mutes into that scene.
3. The **top two rows** are mute pads 9 to 16 for the tracks of the current page. Hit a pad to mute. **SHIFT + pad** solos.
4. **S1** arms **REC MUTES**. While the sequencer records, every mute change is recorded.
5. **S3** turns recorded-mute playback on or off for this sequence.
6. **S2** unmutes all. **ERASE twice** clears this sequence's recorded mutes. **SHIFT + ERASE** clears the last scene.

### Controls: Tracks Pro

| Control | Action |
|---|---|
| DATA turn | Select a track |
| SHIFT + DATA turn | Move the track |
| S1 / S2 / S3 | Mute / Solo / Rename |
| Pad | Select a track (16 per page: pads = tracks 1 to 16, then 17 to 32) |
| SHIFT + pad | Mute that track |
| ERASE twice (4 s) | Delete track |
| DATA push | Track menu |
| SHIFT + ERASE | Explode the track |

### Controls: Mute Scenes

| Control | Action |
|---|---|
| Pad, bottom two rows | Recall scene 1 to 8 |
| SHIFT + pad, bottom two rows | Store current mutes as that scene |
| Pad, top two rows | Mute track 9 to 16 of the page |
| SHIFT + pad, top two rows | Solo that track |
| S1 | REC MUTES on or off |
| S2 | Unmute all |
| S3 | Recorded-mute playback on or off |
| DATA turn | Select a track |
| DATA push | Mute the selected track |
| ERASE twice | Clear recorded mutes for this sequence |
| SHIFT + ERASE | Clear the last scene |

### Controls: Tracks and Track Mute grids

| Control | Action |
|---|---|
| PAD BANK, hold | Tracks grid. A pad selects that track |
| SHIFT + PAD BANK, hold | Track Mute grid. A pad toggles mute for that track |
| SHIFT + PAD BANK, tap | Create a new track |
| K1 | Page (16 tracks per page). The page shows "Page n/m" |

### Tips and limits

* **Delete cannot be undone.** Restore with Snapshots. Delete refuses the last track, and refuses a track that **shares its kit** with another track. The message says "kit shared with N track(s): save+reload". Save and reload the project, then delete.
* **Solo works per program (kit).** Tracks that share a kit solo together.
* Renaming a track that shares a kit with another track changes the name without an undo step.
* **Mute scenes and recorded mutes live on the card**, not in the project.
* Pad mutes are recorded for the **current track only**. Scenes are track mutes only.
* **32 tracks maximum.** With full kits, 32 tracks used about 1.7 GB of memory in the lab. The real RAM of the unit is recorded in `mem-probe.txt` (chapter 5).
* Track create cannot be undone (the stock history refuses it).

---

## 4.8 Explode

### What it does

Splits the current drum track into several tracks so you can mix or mute the parts separately. The new tracks are named after their samples. The source track is kept and **muted** (default). (check on device) the exact split rule (the notes describe the result as "N tracks" created from the track).

### Step by step

1. Select the track you want to explode.
2. On the Mixer, Track View, Pad View, Seq Ops or Tracks Pro page, press **SHIFT + ERASE**.
3. A toast says "Exploded N tracks / source muted". If there is not enough room (32 tracks max), it says so instead.

### Controls

| Control | Action |
|---|---|
| SHIFT + ERASE | Explode the current track (Mixer, Track View, Pad View, Seq Ops, Tracks Pro) |

### Tips and limits

* **Only partly undoable.** Undo puts the notes back, but the empty new tracks stay. Delete them in Tracks Pro or restore a snapshot.
* Counts toward the 32-track maximum.

---

## 4.9 Sample tools: chop, slices, tail, reverse, extract

### What it does

Work on the sample of a pad **without ever changing the WAV file**.

* **Chop** a sample into up to **128 slices**, with a waveform view. Chop by equal regions or by detecting hits. Markers can snap to zero crossings.
* **Extract** a slice to a pad. The pad plays that slice of the same sample (start and end only, no new file).
* **Reverse** a pad.
* **Tail:** the stock **Tail Length**, which is a **looped tail**, not a fade.
* **Fade:** makes a **faded copy** as a new file with a chosen fade out and fade in.
* **Copy pad:** copy a pad (sample, start, end, reverse, tail, levels) to another track.
* The **sample name shows under each pad** on the Pad View, Pad Mixer, Pad Sound and Pad tools pages.

Open **KAOS TOOLS > Sample Tools**. It shows the **Slice view** of the last pad you hit. The same page has **Pad tools**.

### Step by step: chop a loop

1. Hit the pad whose loop you want to chop.
2. Open **Sample Tools**.
3. Push **DATA** and pick **Chop Regions N** (equal pieces) or **Chop Threshold** (finds hits).
4. For regions, set the count with **K1** (2, 4, 8, 16, 32, 64, 128). For threshold, set sensitivity with **K2** and the smallest allowed slice with **K3**.
5. Zoom with **PLUS / MINUS** to adjust markers.
6. **S1** adds a marker at the cursor. **S2** toggles move mode: DATA then moves the nearest marker. **S3** deletes the nearest marker (it merges with its neighbor).

### Step by step: extract slices to pads

* **One slice to one pad:** hit a pad to select slice number (bank x 16 + pad). Then **SHIFT + pad** sends the selected slice to the pad you hit.
* **All slices:** turn **SHIFT + DATA** to choose the destination start pad. Push **DATA** and pick **Extract ALL to pads**. Pick **Extract ALL (overwrite)** to replace pads that already hold samples.
* **One slice to a chosen pad:** use **Extract slice to Dest** in the menu.

### Step by step: Pad tools (reverse, fade, copy)

1. In the DATA menu pick **Pad tools**, or push DATA from Pad tools to go back to the Slice view.
2. Turn **DATA** or hit a pad to select a pad.
3. **S1** flips Reverse. **S2** makes a fade copy (set the fade out with **K2** from 5 to 1000 ms, and the fade in with **K3** from 0 to 50 ms). **S3** starts Copy: choose the destination track with **DATA**, then hit the destination pad. **K1** sets the stock Tail Length.

### Controls: Slice view

| Control | Action |
|---|---|
| S1 / S2 / S3 | Add marker / Move nearest marker / Delete nearest marker |
| DATA turn | Move the cursor |
| SHIFT + DATA turn | Set the destination start pad for Extract ALL |
| DATA push | Menu: Chop Regions N, Chop Threshold, Link slices ON/OFF, Snap to zero ON/OFF, Extract ALL to pads, Extract ALL (overwrite), Extract slice to Dest, Clear slices, Pad tools, Close |
| K1 | Number of regions (2, 4, 8, 16, 32, 64, 128) |
| K2 | Threshold sensitivity, 0 to 100% |
| K3 | Chop guard: minimum slice length, 2 to 250 ms |
| PLUS / MINUS | Zoom in and out (up to 16384 times; never below 256 frames) |
| Pad | Select slice number (bank x 16 + pad). The cursor jumps to its start |
| SHIFT + pad | Extract the selected slice to that pad (undoable; overwrites that pad) |
| ERASE twice | Clear all slices (back to one slice, the whole file) |

### Controls: Pad tools

| Control | Action |
|---|---|
| DATA turn or pad | Select a pad |
| S1 | Reverse (all layers with a sample). One undo step |
| S2 | Fade: render a faded copy as `name_fadeN.wav` and load it on the same pad |
| S3 | Copy pad: DATA picks the destination track, a pad hit picks the destination pad |
| K1 | Stock Tail Length (looped tail) |
| K2 / K3 | Fade out / fade in time (used by S2) |
| DATA push | Open the Slice view for this pad |

### Tips and limits

* **Link slices ON:** slices touch each other, and moving a marker moves the end of the slice before. **OFF:** each slice owns its start and end, and Threshold chop also ends each slice where the hit falls silent.
* **Snap to zero** moves each marker to the nearest zero crossing within 2 ms to avoid clicks.
* Chop guard: at most 128 slices, never shorter than the minimum, and a chop that would break the rule is refused (the old slices stay).
* **The stock Tail is a loop tail, not a fade.** Use Fade for a fade. Fade **adds a sample to the project** (a new file named `name_fadeN.wav`).
* **Extract writes layer 1 only.** A pad with samples on layers 2 to 4 keeps them.
* **Slice sets are stored on the card, not in the project** (24 sets kept). They are lost with the card file.
* The Slice view reads the WAV from disk. A missing or odd-format file shows "wav not readable".
* Detection is tuned on test hits. Noisy loops need Sensitivity and Min length tweaks.
* Slice sets are matched by file name and length. Two different WAVs with the same name and length would share one set.
* Undo for Fade is the stock Load Sample step. It undoes the pad assignment but leaves the new sample in the project.

---

## 4.10 Pad sound editing: envelopes, filter, choke, layers and inserts

### What it does

**Pad Sound** is one page for one pad. It covers level, pan and tune; the amp envelope; the filter and its envelope; **choke groups**; up to **4 velocity layers**; and a menu of resets and tools. The separate **FX** page adds up to **4 insert effects per pad** from the stock catalogue of 97 effects.

Every edit is a normal stock undoable change. Drum tracks only.

### Step by step: shape one pad

1. Hit the pad you want to edit.
2. **SHIFT + SCREEN 3 > KAOS TOOLS > Pad Sound.**
3. Turn **DATA** to pick a row. Turn **K1, K2, K3** to set the row's values (the names are at the bottom).
4. **SHIFT + DATA turn** goes to the previous or next pad. A pad hit also selects that pad.
5. The header shows the pad, kit and sample name. Values still waiting are orange.
6. **S2** resets the current row to the stock default.

### The rows

| Row | K1 | K2 | K3 |
|---|---|---|---|
| Pad | Level | Pan | Tune (plus or minus 12 semitones) |
| Amp AHD | Attack | Hold | Decay |
| Amp SR | Sustain | Release | Decay mode |
| Filter | Type | Cutoff | Resonance |
| FltEnv | Amount | Attack | Decay |
| FltEnv2 | Hold | Sustain | Release |
| FltMode | (check on device) | | |
| EnvType | Amp envelope type | Filter envelope type | - |
| Choke | Target 1 | Target 2 | Target 3 |
| Choke 2 | Target 4 | Mutual | Learn |
| Layer n "vel" | Vel from | Vel to | Semitone |
| Layer n "mix" | Fine tune | Level | Pan |

EnvType shows **AD** or **AHDSR** for each of the two envelopes. The knob order on the FltMode row is not listed in the notes.

**Filter types** (shown by name): Off*, Low 1, 2, 4, 6, 8; High 1, 2, 4, 6, 8; Band 2, 4, 6, 8; BS (bandstop) 2P, 4P, 6P, 8P; BB (bandboost) 2P, 4P, 6P, 8P; Model 1, 2, 3; Vocal 1, 2, 3; MPC3000. The "Off*" name is inferred.

### Step by step: choke groups

1. Pick the **Choke** row. Use the knobs to set up to 4 pads that this pad will **mute** when it plays.
2. Or push **DATA** and pick **Choke learn**, then **hit the pads** you want as targets. Hit a pad again to remove it.
3. Pick **Choke mutual** to make both pads mute each other. **Clear choke** removes the choke.
4. **ERASE** cancels learn mode.

### Step by step: velocity layers

1. Push **DATA** and pick **Split layers 2 / 3 / 4 way** to divide the velocity range among layers. **Layers full range** puts every layer on 0 to 127.
2. Pick a layer row. Set **Vel from / Vel to**, the **semitone**, **fine**, **level** and **pan**.
3. To fill an empty layer, pick **Layer sample from pad** (the source is another pad's sample) or **Layer sample from pool** (browse the project's samples, type a name filter on the pads, push DATA to assign).

### Step by step: insert effects

1. **KAOS TOOLS > FX.**
2. The page shows the selected pad (a pad hit selects it) and 4 insert rows.
3. Push **DATA** on a row to open the list of effects.
4. **K1** picks the family: All, Delay+Reverb, Dynamics, Filter+EQ, Drive+Misc, Modulation, Vintage.
5. Type on the pads to filter by name. **ERASE** is backspace. **SHIFT + ERASE** clears the filter.
6. Turn **DATA** to scroll. Push **DATA** to assign.
7. **ERASE** on a row clears that insert. **S1, S2, S3** bypass an insert (check on device which button belongs to which slot).
8. The FX page also has links to the stock **Color Compressor**, the **Loudness** page and the **EQ**.

### Controls: Pad Sound

| Control | Action |
|---|---|
| DATA turn | Select a row |
| SHIFT + DATA turn | Previous or next pad (128 pads) |
| K1 / K2 / K3 | The row's three values. A turn applies live while you move it. The change closes into one undo step 220 ms after the knob rests |
| S1 | Mute the pad |
| S2 | Reset the current row to the stock defaults |
| S3 | Solo the pad |
| Pad hit | Select that pad (it still plays). In choke learn: toggle a mute target. In layer assign: its sample goes to the layer |
| DATA push | Menu: Reset amp envelope, Reset filter + env, Reset all sound, Split layers 2 / 3 / 4 way, Layers full range, Layer sample from pad, Layer sample from pool, Choke learn, Choke mutual, Clear choke, Close |
| ERASE | Close the menu; cancel assign or learn |

### Controls: FX

| Control | Action |
|---|---|
| Pad hit | Select the pad |
| DATA push | Open the effect list for the selected row |
| K1 (in list) | Effect family |
| Pads (in list) | Type the name filter |
| DATA turn (in list) | Scroll |
| DATA push (in list) | Assign the effect |
| ERASE (on a row) | Clear that insert |
| S1, S2, S3 | Bypass (check on device which slot each button controls) |

### Tips and limits

* **Hold, Sustain and Release** need the **EnvType** row set to **AHDSR**. In the default "AD" type they do nothing. **Release may still not be audible** on drum pads (check on device). Hold and Sustain were proven in the emulator.
* On hardware, **Hold, Sustain and Release were silent on drum pads in v0.20**; v0.21 added the EnvType fix and it is emulator-tested only.
* Choke has 4 slots per pad. Learn refuses a fifth. Choke between pads of different kits is not possible.
* Layers: only layers that hold a sample are editable, plus the next empty one.
* **Only pad inserts work.** Track and master inserts do nothing on this unit.
* Insert **parameters are not editable**. Inserts run with their default settings. The page cannot read back what an insert slot holds from an earlier session; it shows "(active, set before)".
* **Undo of an insert is unreliable.** Use **ERASE** to remove it.
* Inserts are saved in the project and came back after a power cycle in the emulator.
* About 0.3% of the audio time budget is used by the chain in the emulator.

---

## 4.11 Stems

### What it does

Exports **every track and the master as separate WAV files** to the card, all the same length, so they drop into a DAW lined up. Files are 24-bit, 44.1 kHz stereo.

There is no per-track export in the stock MPC Sample, so KAOSAMPLE makes stems by muting the other tracks and rendering one track at a time. It has two ways to render:

* **Offline:** fast, through the stock Song Export. About 1 second per 21 seconds of audio. It respects your mutes but **does not include** KAOSAMPLE's EQ or Loudness.
* **Real time:** plays the song once per file and records the output. Slower (about the song length per file) but can include the EQ and Loudness on the master.

### Step by step

1. Make sure the sequence you want is the current one, and that the card has free space. The page refuses if there is not enough room.
2. **Bypass the stock Color Compressor** first: open **KAOS TOOLS > FX**, push **DATA** on the Color Compressor row, and press **S3** (Bypass). Otherwise stems will not add up to the master. Press the **SAMPLE** button to leave. (check on device) that the compressor shows as bypassed.
3. Open **KAOS TOOLS 2 > Stems**.
4. Set the rows (see below).
5. Press **S1** to start.
6. The page shows the pass number, the file, a progress bar and the time left. The panel is locked while it works.
7. To stop early, press **SCREEN 1** or **PAD FX**. Files already finished stay. The file being made is deleted.
8. When it ends your mutes are restored.

Files appear in **`KAOSAMPLE/stems/<project>-<date>-<time>/`** as `T01-<track name>.wav`, `T02-...`, and `master.wav`, with a `render.txt` that lists frames, range, engine and the peak of each file.

### Controls

DATA turn picks the row. Use K1, K2, K3 to change the value. (check on device) exactly which knob changes each row; the notes name the rows and say K2 and K3 set the bar range.

| Row | Choices |
|---|---|
| Content | Stems + master, master only, stems only |
| Range | Whole sequence, or bars a to b (K2 and K3) |
| Master | Dry, or after EQ + Loudness |
| Tail | 0 to 10 seconds |
| Engine | Auto (offline), or real time |

| Button | Action |
|---|---|
| S1 | Start |
| SCREEN 1 or PAD FX | Cancel while rendering |

### Tips and limits

* **Do not mix offline and real-time files.** They differ by about 4 dB.
* The stock export adds a fixed **3 second tail** and always starts at bar 1. Longer tails come from adding empty bars. A bar range is cut from the full render.
* Choosing "after EQ + Loudness" for the master forces the real-time engine.
* Stems include **Color** but not the duck or the reference.
* **If you already have a stock song export named "Song"** sitting in the Export folder, the page refuses to start. Move or rename it.
* If the app is killed in the middle, the track mutes can stay changed. Restore them in Tracks Pro or from a snapshot.
* Tracks that share one kit retrigger each other's pads (stock behavior), so the master can have fewer voices than the sum of stems.
* Real-time renders can differ by a frame here and there between passes.

---

## 4.12 Song to Seq

### What it does

Takes a **song** (your list of sequences played in order) and **bounces it into one new sequence**. Every note, with all repeats, lands in one long sequence. It is one undo step, and the result is checked by a count and a checksum of all notes.

### Step by step

1. Build your song in the stock Song page.
2. Open **KAOS TOOLS 2 > Song to Seq**.
3. Press **S1**.
4. The page tells you the result and checks. If any note was lost, it says so.

### Controls

| Control | Action |
|---|---|
| S1 | Bounce the song to a new sequence |

### Tips and limits

* Refuses if the song is not 4/4, if a track has over 4000 events, if the total is over 40,000 events or 999 bars, or if there is **no empty sequence slot**. The popup says which.
* **Undo is one step.** The new slot goes back to a plain copy of the first song step. The slot stays marked as used, so delete or reuse it yourself.

---

## 4.13 Auto-kit

### What it does

Finds the hits in a **loop** (or in a **folder of WAVs**), sorts them into **kick, snare, hat and other**, and builds a **16-pad kit**. A preview lets you correct anything before it commits.

### Step by step

1. Open **KAOS TOOLS 2 > Auto-kit**.
2. **Source:** *Loop* uses the sample of the **last pad you hit**, so hit the loop pad first. *Folder* uses the WAVs in `KAOSAMPLE/kits/<name>`, `KAOSAMPLE/sounds` or `KAOSAMPLE/sounds/<name>` (up to 128 files).
3. Set **Sens** (how many hits it finds) and **Min hit** (the shortest hit).
4. Pick a **Kit** layout and the **start bank**.
5. Press **S1** to analyse.
6. On the preview, check each hit. Turn **DATA** to pick one. Change its class with **K1** (kick, snare, hat, perc, other) or push **DATA** for the next class. **S2** skips or unskips a hit.
7. Press **S1** to **commit** and build the kit. Hit the pads to audition (there is no audition on the page).
8. **S3** goes back to setup. **Undo** reverses the whole kit in one press.

While it analyses a folder, **SCREEN 1** cancels.

### Controls: setup page

DATA turn picks the row; K1 and K2 change the value.

| Row | K1 | K2 |
|---|---|---|
| Source | Loop or folder (check on device) | |
| Folder | Pick the folder (check on device) | |
| Sens / Min hit | Sensitivity 0 to 100% | Minimum hit length 10 to 250 ms |
| Kit | Layout | Start bank, A to H (default A) |

| Button | Action |
|---|---|
| S1 | Analyse |

Kit layouts (pads for kick / snare / hat / other): 4/4/4/4, 3/3/6/4, 5/5/3/3, 2/2/4/8, 6/4/4/2, 8/4/2/2.

### Controls: preview page

| Control | Action |
|---|---|
| DATA turn | Select a hit |
| K1 | Override the class of the hit |
| DATA push | Next class for the hit |
| K2 / K3 | Layout / start bank (re-places at once) |
| S1 | Commit: build the kit |
| S2 | Skip or unskip the hit |
| S3 | Back to setup |

### Tips and limits

* Default placement: kicks on 1 to 4, snares (and claps) on 5 to 8, hats (and cymbals) on 9 to 12, perc and other on 13 to 16 of the start bank. Inside a group, pads are ordered by brightness (hats by length, closed to open).
* The class guess is about **88% right on one-shots** and **82% on loop hits**. Toms, rims, shakers and vocals can land anywhere. **Always check the preview.**
* Overlapping hits, clap double-bursts and open-hat decays can cause misses and extras. Sens and Min hit are your knobs.
* Two hits at the same instant (kick and hat on one 16th) count as one.
* A hit ends at the next hit or its audible tail, so an open hat closed by the next hit is cut there.
* Commit **overwrites the pads it places** on the current track (Undo restores them). Pads that get no hit keep what they had.
* Folder mode analyzes only the first 0.8 seconds of each file, is not recursive, and takes 16-bit, 24-bit or float WAV. The card files are not changed; the stock load copies them into the project.
* In loop mode the commit is refused if the source pad changed since you analysed.

---

## 4.14 Groove

### What it does

Changes the **feel** of the current track.

* **Swing:** 50% (straight) to 75%, on a 1/8 or 1/16 grid.
* **Humanize:** random timing and velocity changes. The same seed gives the same result.
* **Groove extract:** reads the feel out of a 1 or 2 bar **loop sample**.
* **Groove apply:** puts that feel on the track, with a strength knob.
* **Groove library:** save, load and delete grooves as files on the card.
* **Live:** lets you change values freely without stacking changes on top of each other.

Everything acts on the **current track**. One Undo press reverts a whole apply.

### Step by step: swing

1. Open **KAOS TOOLS 2 > Groove**. Pick the **Swing** row.
2. **K1** sets the amount, **K2** the grid (1/8 or 1/16).
3. **S1** applies it to the track.

### Step by step: copy the feel of a loop

1. Hit the pad that has your loop. The loop must be **exactly 1 or 2 bars** and start on the downbeat.
2. Pick the **Extract** row. **K1** sets 1 or 2 bars. **K2** sets sensitivity.
3. Press **S1** to extract. The page draws the feel: green bars for velocity, yellow for late, blue for early.
4. Pick the **Groove** row. **K1** sets strength. **S1** applies.
5. **S2** saves the groove to the card. **K2** picks a saved groove. **S3** loads it.

### Controls

DATA turn picks the row. SHIFT + DATA turn picks the previous or next track.

| Row | K1 | K2 | K3 | S1 | S2 | S3 |
|---|---|---|---|---|---|---|
| Swing | Amount 50 to 75% | Grid 1/8 or 1/16 | | Apply | | |
| Humanize | Timing 0 to 40 ms | Velocity 0 to 40 | Seed 0 to 127 | Apply | | |
| Extract | Loop length 1 or 2 bars | Sensitivity | | Extract from the last-hit pad | | |
| Groove | Strength 0 to 100% | Pick a saved groove | | Apply | Save | Load |
| Live | On or off | | | Back to the original | | |

On the Groove row, **ERASE twice deletes** the picked groove file.

### Tips and limits

* The grid for extract and apply is **fixed at 1/16**.
* **Applying swing twice stacks** (as any swing does). Use **Live**, or undo.
* **Live mode** keeps a copy of the original notes. While it is on, every change re-applies from the original. Each settled change is one undo step. It switches itself off if the track or sequence changes, or the notes were edited elsewhere. S1 on the Live row restores the original exactly.
* Grooves live in `KAOSAMPLE/grooves/` as small text files. A name that exists gets "-2", "-3". Nothing is overwritten.
* Acts on **all notes of the track** (no selection or bar range). Notes only.
* Up to **4000 events** per track. Assumes 4/4.
* Tested with test loops, not with real drum loops. Treat results with care.
* Live mode is not true real-time swing. It re-applies from the stored original.

---

## 4.15 Retro Rec (retrospective record, tap tempo, Save version)

### What it does

* **Retro record:** KAOSAMPLE secretly remembers your pad hits. Press **SHIFT + SAMPLE RECORD** and the last 1, 2, 4 or 8 bars you played drop into the current track, quantized, **even if you were not recording**.
* **Tap tempo:** **SHIFT + TAP TEMPO** taps a tempo. Plain TAP TEMPO is still the stock button.
* **Loop BPM:** estimates the tempo of the loop on the last pad you hit and sets it.
* **Save version:** one press saves `name-v1`, `name-v2` and so on. Nothing is overwritten.

### Step by step: retro record

1. Play pads. The sequence can be playing, or stopped.
2. Press **SHIFT + SAMPLE RECORD** on any screen. The notes land in the current track.
3. For more control open **KAOS TOOLS 2 > Retro Rec**, pick the **Retro** row, then set bars, quantize and source. Press **S1** to drop. **S2** clears the memory.
4. **Undo** removes the whole drop in one press.

### Step by step: tap tempo

1. Tap **SHIFT + TAP TEMPO** at least 3 times in rhythm. A popup shows the BPM on each tap.
2. About 1 second after the last tap, the sequence tempo is set once.
3. On the page, **S1** (or DATA push) taps, **S2** resets, **S3** applies now.

### Step by step: loop BPM

1. Hit the pad with the loop.
2. Pick the **Loop BPM** row.
3. **K1** chooses /2, x1 or x2. **K2** chooses a range (75 to 170, 60 to 120, or 100 to 200).
4. **S1** finds the tempo. **S2** sets it.

### Step by step: Save version

1. Pick the **Version** row.
2. Press **S1**. KAOSAMPLE drives the stock Save screen for you and types the name. This takes about **15 to 20 seconds**, and the panel is locked meanwhile. Do not touch anything.

### Controls

DATA turn picks the row. SHIFT + DATA turn picks the previous or next track.

| Row | K1 | K2 | K3 | S1 | S2 | S3 |
|---|---|---|---|---|---|---|
| Retro | Bars: Auto, 1, 2, 4, 8 | Quantize: off, 1/8, 1/16, 1/32 | Source: this track or all tracks | Drop | Clear the memory | |
| Tap | | | | Tap (or DATA push) | Reset | Apply now |
| Loop BPM | /2, x1, x2 | Range | | Find tempo | Set tempo | |
| Version | | | | Save version | | |

| Combo (any screen) | Action |
|---|---|
| SHIFT + SAMPLE RECORD | Retro drop |
| SHIFT + TAP TEMPO | Tap tempo |

### Tips and limits

* **Auto** uses the loop length while playing, or the phrase you just played when stopped.
* Only **pad hits** are remembered. Not MIDI keyboard notes, not note repeat, and not 16 Levels notes beyond what the pads map.
* The drop **refuses while the stock recorder is recording** ("stop recording first").
* **4/4 assumed.**
* Tap tempo works between 30 and 300 BPM, and it is refused while recording.
* Loop BPM is a guess for loops that are not exact bars, and for double or half time. It needs about 4 hits. It likes straight 16th loops; swung or triplet loops are less reliable.
* **Save version** was tuned on the emulator, because it types blind into the stock Save screen. On a real panel it may be faster or slower. Names are **lowercase** with dashes, digits and spaces only ("My_Beat" becomes "my-beat-v1"). If typing goes wrong, a wrongly named project may exist on the card (it never overwrites anything), and the page says "WRONG NAME" or "NOT saved".
* Whether the stock app uses SHIFT + TAP TEMPO and SHIFT + SAMPLE RECORD for something else is unknown. KAOSAMPLE takes those presses.

---

## 4.16 Clean up: purge and mono

### What it does

* **Purge unused:** finds samples in the project that no pad uses, lists them with total size, and removes them from the project in one undo step.
* **Make mono:** finds stereo samples that are really mono (or one-sided, or with the channels in opposite polarity) and makes a **mono copy**, saving memory. It points the pads at the copy.

### Step by step: purge

1. Open **KAOS TOOLS 2 > Clean Up**. The **Purge** row is on top.
2. Press **S1** to scan. The list shows names, sizes and the total.
3. **K3** scrolls the list.
4. Press **S2** **twice within 6 seconds** to purge. The second press re-scans and refuses if the list changed.
5. **Undo** brings the samples back in one step.

### Step by step: make mono

1. Pick the **Mono** row.
2. **K1** picks the program (kit). **K2** chooses *near-mono only* (default) or *all stereo*.
3. Press **S1** to analyse. The list shows each sample as MONO, ONE (one-sided) or wide, with its side level.
4. Press **S2** twice to convert.
5. The copies are named `name_mono.wav` and saved in `KAOSAMPLE/work`. The originals are never changed.
6. **Undo** returns the stereo samples and every pad value in one step.

### Controls

DATA turn picks the row. SHIFT + DATA turn picks the previous or next track.

| Row | K1 | K2 | K3 | S1 | S2 |
|---|---|---|---|---|---|
| Purge | | | Scroll the list | Scan | Press twice: purge |
| Mono | Program | Near-mono or all stereo | Scroll the list | Analyse | Press twice: convert |

### Tips and limits

* Purge refuses when the project has a **program that is not a drum program** (keygroup programs).
* **Purge removes samples from the project only.** Files on the card are not deleted.
* **Make mono** works on **one program** at a time. Other programs keep their stereo originals until you purge.
* The "RAM saved" number is an **estimate**. The real saving needs a hardware check.
* "All stereo" mode folds wide material down to mono, which can lower the level. That is why near-mono only is the default.
* WAV loop and cue chunks are not copied to the mono file.

---

## 4.17 Perform: scale, chord, arp and Keys

### What it does

Turns the 16 pads of your drum program into a **playable instrument** by copying the source pad's sample onto all 16 pads at different pitches. The source sample is treated as **C**.

* **Scale:** pad N plays scale degree N. 16 scales: Chromatic, Major, Minor, Dorian, Phrygian, Lydian, Mixolydian, Locrian, Harmonic minor, Melodic minor, Major pentatonic, Minor pentatonic, Blues, Whole tone, Diminished (whole-half), Phrygian dominant. Any root and octave.
* **Chord:** each pad plays a chord. Diatonic triads or 7ths, or **your own set** (16 chord shapes, one per pad). Own sets are saved per sequence on the card.
* **Arp:** an arpeggiator on a scale kit. Patterns Up, Down, UpDown, Random, AsPlayed. Rates 1/4 to 1/32 plus triplets. Gate, 1 to 3 octaves, latch, chord expand.
* **Keys:** play the pad program from a **MIDI keyboard** over 16, 32 or 48 pads.

The pitch changes use the normal pad tuning, so pads play, record and undo like any other. **Everything is one undo step ("Perform setup").**

### Step by step: scale kit

1. Hit the pad whose sample you want to use, then open **KAOS TOOLS 2 > Perform**.
2. Pick the **Mode** row. Set **K1** to *Scale*.
3. Press **S2** to take the last-hit pad as the source.
4. Pick the **Key** row. Set the root with **K1**, the scale with **K2**, the octave with **K3**.
5. Go back to **Mode** and press **S1** to **Apply**. (Any value change also re-applies after about 0.6 seconds.)
6. Play the pads. Record them as usual.

### Step by step: chords

1. Set **Mode** to *Chord*.
2. **Chord** row: **K1** triads or 7ths, **K2** diatonic or your own set.
3. For your own set use the **Edit** row: **K1** root, **K2** shape, **K3** pad. **S1** saves, **S2** loads, **S3** resets.

### Step by step: arp

1. Set **Mode** to *Arp* (the same kit as Scale, with the arpeggiator on).
2. **Arp** row: **K1** pattern, **K2** rate, **K3** gate. **Arp 2** row: **K1** octaves, **K2** latch, **K3** chords.
3. Hold pads in the kit bank. They are arpeggiated on the sequencer grid. The stock recorder records the result.
4. **ERASE** clears held arp pads.

### Step by step: Keys

1. On the MPC set **Settings > MIDI > Pad MIDI In = On**. **The MPC resets this at every start**, so set it every time. Make sure the right MIDI port is chosen.
2. Set **Mode** to *Keys*.
3. **Keys** row: **K1** base note, **K2** span 16, 32 or 48.
4. Play the keyboard. Notes become pad hits, and the stock app plays and records them.

### Controls

DATA turn picks the row.

| Row | K1 | K2 | K3 | S1 | S2 | S3 |
|---|---|---|---|---|---|---|
| Mode | Mode: Off, Scale, Chord, Arp, Keys | Bank | Octave | Apply | Take the last-hit pad as source | Go to the bank |
| Key | Root | Scale | Octave | | | |
| Chord | Triads or 7ths | Diatonic or user | | | | |
| Edit | Root | Shape | Pad | Save | Load | Reset |
| Arp | Pattern | Rate | Gate | | | |
| Arp 2 | Octaves | Latch | Chords | | | |
| Keys | Base note | Span 16 / 32 / 48 | | | | |

On the Mode row the top bar shows S1 "Apply", and S3 shows "Bank". On the Edit row S3 shows "Reset". ERASE clears held arp pads.

### Tips and limits

* A kit **overwrites the pads** it uses (Undo restores). Other layers on those pads are cleared.
* The sample is assumed to be C. Pitches beyond plus or minus 48 semitones clamp.
* **Gate** sets the recorded note length. One-shot drum pads **keep their tail**, so gate cuts the sound only on pads that allow it.
* **Arp works only while the current bank is the kit bank.**
* **Keys and Arp are exclusive**: you cannot use both at once.
* **Keys** needs Pad MIDI In On every boot. A USB keyboard was only tested through the lab's MIDI port, not on a real unit.
* A chord apply writes about 180 values at once. This was fine in the emulator but not stress-tested on the unit.
* In the Arp, the first note waits for the next grid step while playing.

---

## 4.18 Looper

### What it does

A **multi-pass looper** synced to the sequence. Record a loop, overdub more passes (up to 8), undo a pass, change pass levels, and **export** the result to a pad or into the sequence. It records the **MPC's own output** (a resample of the master), so it hears your pads, sequence and anything else the MPC plays. The loop never records itself, so there is no feedback.

Two looper tiles exist:

* **Loop Rec** on KAOS TOOLS 2 is the new one described here. Its page title is "LOOPER".
* **Looper** on KAOS TOOLS is the older page that drives the stock looper. It is still there and unchanged. (check on device) its controls; the notes for this build do not describe them.

### Step by step

1. Press **PLAY** so the sequence runs. **The looper only works while the transport plays.**
2. Open **KAOS TOOLS 2 > Loop Rec**.
3. Turn **K1** (while the loop is empty) to choose the length: the sequence length, or 1, 2, 4 or 8 bars.
4. Press **S1**. The state shows "WAITING FOR BAR 1", then "RECORDING" when the next loop boundary arrives.
5. After the first pass the looper carries straight on into the next pass (overdub), starting on the same beat. Press **S1** while it is recording to say "stop after this pass". Press it again to keep overdubbing.
6. Press **S2** to undo the last pass. **K1** picks a pass, **K2** sets its level from 0 to 150%. **K3** sets the loop's playback volume.
7. Press **S3** to mute or unmute the loop. Recording keeps going.
8. To keep the loop, turn **DATA** to a row at the bottom and push it:
   * **To pad:** make a 24-bit WAV on the **next empty pad** of the current track.
   * **To Seq (this track):** the same, plus a note at bar 1 on the current track.
   * **To Seq (NEW track):** the same, on a new drum track.
9. Each export is **one undo step**.
10. **ERASE twice** clears the loop.

### Controls

| Control | Action |
|---|---|
| K1 | Empty loop: length (Seq, 1, 2, 4, 8 bars). Otherwise: select a pass |
| K2 | Level of the selected pass, 0 to 150% |
| K3 | Loop playback level |
| S1 | Record or Overdub. While armed: Disarm. While recording: stop after this pass |
| S2 | Undo last pass. While recording: drop the running pass |
| S3 | Mute or unmute the loop |
| DATA turn | Pick a row: To pad, To Seq (this track), To Seq (NEW track) |
| DATA push | Run the row |
| ERASE twice | Clear the loop |
| SHIFT + DATA turn | Previous or next track (the track that gets the pad or note) |

Pads, transport and SHIFT combos pass through, so you can play pads into a pass.

### Tips and limits

* **Records the master resample, not the line-in.** EQ, Loudness and master volume are **not baked in**.
* **Needs the transport playing.** Stopping the transport stops the loop and drops a running pass.
* **Tempo change:** the loop keeps its length in time, and the page warns **"tempo changed: re-record the loop"**.
* Only **4/4** was tested.
* A pass starts within about one pulse of the boundary.
* Max **8 passes** (fewer on long loops, to protect memory). Loops up to 16 bars at 60 BPM fit.
* An export freezes the screen for **140 to 260 ms** and the loop relocks once afterwards (a tiny glitch in the loop output).
* Exports are saved in `KAOSAMPLE/work` and in the project's data folder, named like `loop_<bars>b_<bpm>_<time>`. They are never overwritten. Free space is checked on the card and the internal storage.
* Redo of a new-track export can fail when the stock history is full. Nothing is lost. Run the export again.
* **Sample-record undo is not done.**

---

## 4.19 Files and templates

### What it does

* **Save as template:** keep the whole project, with all its kits, as a template on the card.
* **New from template:** start a new project from a template. The template itself is never changed.
* **Default template:** from a fresh project, builds a starter template from your first four kits (`KAOSAMPLE/kits`), at 90 BPM and 4 bars.
* **New folder:** make a folder on the card or the internal storage, named like the Name row.
* **Random project names:** an adjective plus a noun, such as "dusty-kick" or "warm-loop". It never reuses a name that is taken.
* **Browser jump:** SHIFT + DATA jumps 5, 10 or 20 rows at a time in the file browsers.

### Step by step: save a template

1. Open **KAOS TOOLS 2 > Files**. Pick the **Template** row.
2. Pick the **Name** row. **K1** chooses *random* or *from project*. **S1** rolls another name.
3. Go back to **Template**. Press **S1** (**Save**). KAOSAMPLE uses the stock Save screen to type the name and save, then copies the project onto the card. This takes a while and shows a progress line.

### Step by step: start from a template

1. **Wait until the MPC has been running for about 2 minutes.** The page refuses earlier ("wait Ns").
2. On the **Template** row, use **K1** to pick the template.
3. Press **S2** twice (**New**). The first press warns you: "replaces the open project (kept as kaos-prev)".
4. The screen goes **black for about 35 to 40 seconds** while the app restarts. Do not turn the MPC off.
5. The new project opens. It is saved as a copy, so the template stays untouched.

### Step by step: other rows

* **Folder row:** **K1** chooses where (KAOSAMPLE, Card Projects, Internal Projects). **S1** creates a folder named like the Name row.
* **Browser row:** **K1** sets the jump size: off, 5, 10 or 20. Then, in the stock **Sample Select** and **Project Select** browsers, **SHIFT + DATA turn** jumps that many rows.

### Controls

DATA turn picks the row.

| Row | K1 | S1 | S2 | S3 |
|---|---|---|---|---|
| Template | Pick the template | Save as template | New from template (press twice) | Default template (press twice) |
| Name | Style: random or from project | Roll another name | | |
| Folder | Where: KAOSAMPLE, Card Projects, Internal Projects | Create folder | | |
| Browser | Jump size: off, 5, 10, 20 | | | |

The top bar shows S1 as Save, Roll, Create (depending on the row), S2 as "New" and S3 as "Default" on the Template row.

### Tips and limits

* **A template is a full copy**: about **90 MB** each with the stock starter kit, and about the same again for the new project. The layer refuses when space is short, but it **never deletes your old projects**.
* New from template **restarts the app** and **replaces the open project**. The old one is kept as a `kaos-prev` copy and a safety snapshot. There is no "unsaved?" check. **Save first.**
* **Safety (v0.30.1):** New from template refuses unless the app is healthy. It keeps a safety copy of the new project's samples on the card (in `KAOSAMPLE/safety`) and repairs the project after the restart if the stock app drops sample files. Old safety copies are pruned (the newest 2 are kept). In the lab, the stock app once deleted sample files from a swapped-in project, for a reason nobody has found yet. **The template folder on the card is never touched, so it is the copy to trust.**
* If the app had already fallen back to stock (two quick exits), the layer is not running after the restart and the job cannot finish.
* Template names must pass the stock editor: lowercase letters, digits, and `- . #`. Typing costs about 0.8 seconds per character, so names stay short.
* Jump works only in the stock **Sample Select** and **Project Select** browsers. Other pickers (Sample Save/Export folder chooser, SD Card Access) are not covered.
* **Remember the last folder** after a restart is not done. The stock app does remember the folder per browser while it runs.
* **Default template** needs a fresh project (2 tracks or fewer) and kits laid out as `KAOSAMPLE/kits/<kit>/<kit>.xpm`.
* **Kits on the card:** KAOSAMPLE can load a kit from `KAOSAMPLE/kits/<name>/` onto a new track (all 16 pads). (check on device) which page has the load command; the notes only say it is the same kit loader that Default template uses.

---

## 4.20 Monitor: CPU and RAM

### What it does

A small system monitor on the **About** page. It reads once a second, and only while the page is open.

It shows:

* **CPU** use in percent,
* **RAM** used and total,
* **battery** percent and charging status,
* **temperature**,
* the app's own memory use,
* **Xrun** count: audio dropouts. It turns red when it is above 0.

### Step by step

1. **SHIFT + SCREEN 3 > About.**
2. Watch CPU and RAM while you use the heavy tools (Looper, Stems, 32 tracks).

### Controls

There are no controls. It is a read-out.

### Tips and limits

* **Battery** is unverified on hardware ("no sensor" is shown if the unit has none). Temperature shows "n/a" if there is no sensor.
* **RAM cannot be stretched.** The unit's kernel has no swap or compressed RAM. Track count and loop length are bounded by real RAM.
* **Xrun counter:** only changes during steady playback mean something. The emulator gave start-up dropouts, so the number says little about the hardware yet.
* Every boot writes a `mem-probe.txt` file to the KAOSAMPLE folder with the unit's real RAM size and kernel facts. **Please send this file** so 48 or 64 tracks can be considered (chapter 6).
* A low-memory plan: use Clean Up (4.16) to purge unused samples and make near-mono samples mono.

---

# 5. The card folder map

KAOSAMPLE uses two places on the SD card.

## 5.1 Top of the card

| Item | What it is |
|---|---|
| `autorun/` | The KAOSAMPLE program itself: the seven files you copied (chapter 2). Delete this folder and the MPC is stock again. Never edit the files. |
| `KAOSAMPLE/` | Created on first start. Your data and logs. Safe to keep or delete (chapter 2.4). |
| `MPC-Sample/Projects/` | The stock projects folder on the card. The Files page lists it as "Card Projects" for New folder. |

## 5.2 Inside the KAOSAMPLE folder

### Folders

| Folder | What is in it | Made by |
|---|---|---|
| `snapshots/` | Project copies saved every 5 minutes, newest 30 kept. Named `snap-NNNN-MMDD-HHMM.xpj`. A name ending in `-pre` is a safety copy made just before a restore. | Auto, and Snapshots restore |
| `stems/` | One folder per render, `<project>-<date>-<time>/`, with `T01-<track>.wav` ... `master.wav` and `render.txt`. | Stems |
| `refs/` | **You** put reference WAV files here for Color's Reference A/B. | You |
| `kits/` | Card kits, one folder per kit: `kits/<name>/<name>.xpm` plus its WAV files. Also a source folder for Auto-kit and Default template. | You, and the kit saver |
| `sounds/` | Folders of one-shot WAVs for Auto-kit's folder mode (`sounds/` or `sounds/<name>`). | You |
| `templates/` | One folder per template: `<name>.xpj`, `<name>_[ProjectData]/` (all WAVs) and `template.info`. A folder without `template.info` is an unfinished copy and is not listed. | Files |
| `work/` | New WAV files made by the tools: fade copies (`name_fadeN.wav`), mono copies (`name_mono.wav`), and loop exports (`loop_<bars>b_<bpm>_<time>.wav`). | Sample tools, Clean Up, Looper |
| `grooves/` | Saved grooves, `<sample name>.grv`, plain text. | Groove |
| `chords/` | Your own chord sets, one file per sequence (`S<n>.chd`). | Perform |
| `safety/` | Safety copies of a new project's samples made by New from template. The newest 2 are kept. | Files |

### Files

| File | What it is |
|---|---|
| `fx.txt` | Saved settings: master EQ, Loudness, and the Color, Duck and Reference-file-name lines. Written about 2 seconds after a change. |
| `slices.txt` | Your chop slice sets. 24 are kept. |
| `mutes.txt` | Mute scenes and recorded mutes. |
| `files-pending.txt`, `files-safety.txt` | Short-lived notes used during New from template. They vanish when the job ends. |
| `layer-events.log` | When the app started and exited, and the exit codes. **Send in bug reports.** |
| `layer-last-exit.log` | What was happening at the last exits and crashes. **Send in bug reports.** |
| `layer-app.log` | The recent running log of the app. **Send in bug reports.** |
| `mem-probe.txt` | Written at every start: the unit's real RAM size and kernel facts. **Send in bug reports.** |
| `crash-last.txt` | Written after a crash with the details. Send it too if it exists. |
| `device-content.txt` | A one-time listing of files on the MPC, made for Omar. Not needed for bug reports. |

Things that live **in the project** and travel with it: pad sounds, step edits, inserts, tempo, extracted slices (as start and end settings). Things that live **only on the card**: slice sets, mute scenes, recorded mutes, grooves, chord sets, settings in `fx.txt`.

---

# 6. Troubleshooting and how to report a bug

## 6.1 Troubleshooting

| Problem | Likely cause and fix |
|---|---|
| I see only the normal MPC, no KAOSAMPLE splash | The firmware is not 1.3.0.12 (KAOSAMPLE turns itself off). Or it crashed twice within 2 minutes and fell back to stock. Or the `autorun` folder is not at the top of the card, or a file was renamed. Check these, then restart the MPC. |
| SHIFT + SCREEN 3 does nothing | KAOSAMPLE is not running (see above). Check that the splash showed. |
| A job says "wait N s" or "refused: app exited N s ago" | Restore, New from template and similar jobs need the app to have been running for about 2 minutes, with no recent exit. Wait, then try again. |
| The screen is black for about 40 seconds after a restore or New from template | Normal. The app is restarting. Wait. Do not turn the MPC off. |
| Stock message "Sample not found" with ABORT and RESOLVE after a restore | The restored project is missing samples. This is the stock message. |
| A red "Command failed" box appears and the buttons stop working | Press SCREEN 1, 2 or 3 to clear it. Avoid redo (SHIFT + PLUS) past a track create. |
| "Cannot undo - Delete Track" or "Redo not possible" | These are on purpose. Track create and delete cannot be undone. Use Snapshots. |
| Delete track says "kit shared with N track(s)" | Save and reload the project, then delete again. |
| Keys mode plays nothing | On the MPC set Settings > MIDI > Pad MIDI In = On. It resets every start. Check the MIDI port. |
| Arp does nothing | The current bank must be the kit bank, and the mode must be Arp. |
| Looper says "WAITING FOR PLAY" | Press PLAY. The looper only runs while the transport plays. |
| Looper says "tempo changed: re-record the loop" | You changed the tempo after recording. Record the loop again. |
| Stems do not add up to the master | Bypass the stock Color Compressor first (4.11). Do not mix offline and real-time files. |
| Stems refuse to start | Not enough card space, or a stock export named "Song" is sitting in the Export folder. Free space, or move that file. |
| Auto-kit put a hit in the wrong group | Normal at about 82 to 88% accuracy. Fix it in the preview with K1 before you commit. |
| Reference A/B says "load a reference first" | Put WAVs in `KAOSAMPLE/refs`, pick one on the Ref row, push DATA to load, then press S1. |
| Reference sounds quieter or louder than expected | The loudness meter needs a few seconds of music. Look for "peak cap" on the page, and try the Trim knob. |
| Color knob zippers a little | Known. Move the knob slowly. |
| Track count stops at 32 | That is the maximum. |
| Out of memory, slowdowns, or dropouts | Use Clean Up: purge unused samples and make near-mono samples mono. Close heavy pages. Check About for CPU, RAM and Xrun. |
| A save-version name looks odd | The stock editor allows only lowercase letters, digits, dash, dot, hash and space. Names are changed to fit. |
| I lost a project | Open Snapshots (4.5). If the unit will not start KAOSAMPLE, copy a file from `KAOSAMPLE/snapshots` on the card to your computer and send it to Omar. |
| I want to go back to stock | Delete the `autorun` folder from the card (chapter 2.4). |

## 6.2 How to report a bug

After a crash or weird behavior, send Omar:

1. **What you were doing**: which screen and which buttons.
2. **The time it happened**.
3. **These files from the card's `KAOSAMPLE` folder:**
   * `layer-events.log`
   * `layer-last-exit.log`
   * `layer-app.log`
   * `mem-probe.txt`
   * `crash-last.txt` too, if it exists.

To get them: power off the MPC, put the card in your computer, and copy the files out of the `KAOSAMPLE` folder.

Tips for a good report:

* Say which tool (for example "Looper, overdub pass 2").
* Say how long the app had been running when it happened.
* If the screen froze or went black, say for how long.
* If you restored a snapshot, say which one.
* Please do not wipe the logs before you send them. They are small, and they are overwritten by newer events.
* Do not share this build with others.

---

# 7. Known limits

## 7.1 Hard limits of the MPC Sample

These are walls of the platform, not bugs to be fixed later.

* **Stock inserts on tracks and master do not process audio.** Only pad inserts work. KAOSAMPLE ships its own master EQ, Loudness and Color.
* **No per-track audio.** There is no per-track kill EQ or processing. Stems are made by muting tracks and rendering.
* **No extra memory.** The unit has no swap and no compressed RAM, so memory cannot be stretched. Track count and loop memory are bounded by the real RAM. **32 tracks maximum.** 48 or 64 tracks are not offered until the real RAM size is confirmed from `mem-probe.txt`.
* **No licensing bypass.** KAOSAMPLE does not touch licensing or the boot chain. Features that the stock product locks (for example keygroup programs and MIDI tracks) stay unavailable.
* **The stock app owns undo.** Track create cannot be undone. Delete track cannot be undone. Explode is only partly undoable. EQ, Loudness, Color and Duck settings are not in the stock undo list (Color has its own 32-step undo that is lost at restart). Recording a sample cannot be undone yet.

## 7.2 Beta status

* Everything from v0.21 up has been tested only in an emulator (see the table in 1.3). Timing, speed and button feel on the real unit are not yet known.
* An earlier display crash seen on hardware was hardened but its root cause was not found. An earlier case where the playhead appeared frozen on hardware was not reproduced; extra logging was added to catch it next time.

## 7.3 Limits by feature

| Feature | Limit |
|---|---|
| Master EQ | Master only, no per-track EQ |
| Loudness | Not in undo. Skipped by offline stem renders |
| Color / Duck / Reference | Loudness meter is LUFS-style, not certified. Duck sources are one track's notes and live pad hits only (not the arp, MIDI input or other tracks). Knob zipper on fast sweeps. Reference list max 48 files. Color undo is lost at restart |
| Undo | History holds about 512 steps. Several actions cannot be undone (7.1) |
| Snapshots | 30 kept. Restore needs about 2 minutes of app uptime. About 35 to 40 seconds of black screen |
| Step edit | 4/4 only. Quantize snaps to 1/16. Selection 256 notes. Bar tools refuse tracks over 4000 events. Grid shows 512 events per track |
| Track tools | Delete not undoable. Delete refuses a shared-kit track. Solo is per kit. Mute scenes and recorded mutes live on the card. Pad mutes record for the current track only |
| Explode | Only partly undoable. Counts toward 32 tracks |
| Sample tools | Slice sets live on the card (24 kept). Extract writes layer 1 only. Tail is a loop tail, not a fade. 128 slices max |
| Pad Sound | Drum tracks only. Release may be silent on drum pads. Choke has 4 slots. No effect parameters. Undo of an insert is unreliable |
| Stems | Fixed 3 second stock tail. Offline skips EQ and Loudness. Offline and real-time differ by about 4 dB. Track mutes can stay changed if the app is killed |
| Song to Seq | 4/4 only. Needs an empty sequence slot. Max 4000 events per track, 40,000 total, 999 bars |
| Auto-kit | About 88% class accuracy on one-shots, 82% on loops. Folder mode 128 files, not recursive, 0.8 s analyzed per file |
| Groove | Whole track only. Grid fixed at 1/16. Loop must be exactly 1 or 2 bars. Tested only on test loops |
| Retro Rec | Pad hits only. Refuses while recording. 4/4 assumed. Save version tuned on the emulator |
| Clean up | Purge needs drum programs only. RAM saving is an estimate. Make mono works on one program at a time |
| Perform | Arp only in the kit bank. Arp and Keys cannot be on together. Keys needs Pad MIDI In = On at every start. Gate does not cut drum tails. USB keyboard path not yet tested on a real unit |
| Looper | Records the master only, not the line-in. Needs the transport playing. 4/4 only. 8 passes max. Export blocks the screen for 140 to 260 ms |
| Files | Templates are about 90 MB each. New from template restarts the app and replaces the open project. Jump works in two stock browsers only. The last folder is not remembered after a restart |
| Monitor | Battery reading unverified |

## 7.4 What is not in this build

* The **KAOSAMPLE Companion** Mac app (card status, log reader, kit sync, stems browser, project backups) is a separate project. It is not part of this tester build and has not been used with a real card yet.
* This guide does not cover the Pad Mixer, Sequences, Song, MIDI, Programs and Stock screens tiles, because the notes for this build do not describe their controls.

Thank you for testing KAOSAMPLE. Your reports on the real machine are what turn this beta into a release.
