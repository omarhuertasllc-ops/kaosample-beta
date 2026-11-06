# KAOSAMPLE — public beta

KAOSAMPLE adds tools on top of the stock **MPC Sample** app: stems, looper, auto-kit, groove,
retro record, scales/chords/arp, master EQ + loudness + tape/vinyl color, sidechain duck,
reference A/B, templates, full undo history and more.

It runs from the SD card, in RAM only, and changes nothing inside the MPC.
**Remove the `autorun` folder (or boot without the card) and your MPC is 100% stock.**

> ⚠️ **Beta.** Not fully tested on real hardware yet. It may crash. **Back up your SD card first.**
> Requires MPC Sample firmware **1.3.0.12**. On any other firmware it switches itself off.
> KAOSAMPLE is an independent project by Omar Huertas LLC / kaosbeatz.
> It is **not made, endorsed or supported by Akai Professional or inMusic.**

## Download
Get the latest zip from **[Releases](../../releases)**. Full instructions are in `READ-ME-FIRST.txt` inside the zip.

## Install
1. Back up your SD card (copy the whole card to your computer).
2. Copy the `autorun` folder from the zip to the **top** of the card. Don't rename or edit any file.
3. Eject the card safely, put it in the MPC, and turn the MPC on.
   You'll see the KAOSAMPLE splash, then the normal MPC screen.

## Uninstall
Delete the `autorun` folder from the card.

## What's in it
```
WHERE THE NEW STUFF IS
Open KAOS TOOLS (and KAOS TOOLS 2) from the menu. Tiles include:
- Stems: export every track + master as WAVs to KAOSAMPLE/stems
- Song to Seq: turn a song into one sequence
- Groove: swing/humanize per track, extract groove from a loop
- Auto-kit: chop a loop or folder into a 16-pad kit automatically
- Retro Rec: grab what you just played even if you weren't recording
- Perform: scales, chords, arp, and Keys (play a pad melodically from a MIDI keyboard)
    For Keys: Settings > MIDI > Pad MIDI In = On (MPC resets this every start)
- Clean up: purge unused samples, make kits mono to save memory
- Looper: record loops in sync with the sequence, overdub, export to a pad or the sequence
- Files: project templates, new folders, fast browsing (SHIFT+DATA jumps), random names
- Color: tape/vinyl master color, sidechain duck, reference track A/B
    (put reference WAVs in KAOSAMPLE/refs)
Also: master EQ + Loudness, undo/redo history, step edit, track tools, full pad sound
editing, sample chop/tail/reverse, and auto project snapshots in KAOSAMPLE/snapshots.
Nearly everything is undoable with the stock Undo.
```

## Known limits
```
KNOWN LIMITS
- Stock track/master insert effects don't process audio. Use KAOSAMPLE's master EQ/Loudness/Color.
- Looper records the MPC's own output, not line-in.
- Recording a sample can't be undone yet.
- 32 tracks max.
```

## Report a bug
Open an **[Issue](../../issues/new/choose)** using the bug-report form. Attach these files from the card's `KAOSAMPLE` folder:
`layer-events.log`, `layer-last-exit.log`, `layer-app.log`, `mem-probe.txt`.

## Changes
See [CHANGELOG.md](CHANGELOG.md).

## Terms
Free beta for testing. No warranty. Use at your own risk; you're responsible for backing up your work.
Don't re-upload the zip. Link to this page instead.
This repository contains no Akai firmware and no source code.
MPC and MPC Sample are trademarks of their respective owners.
