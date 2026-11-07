# KAOSAMPLE changelog

KAOSAMPLE is an add-on layer for the stock Akai MPC Sample app (firmware 1.3.0.12). It loads next to the stock app (LD_PRELOAD), so every stock feature keeps working and the layer adds pages, tools and audio processing on top. By Omar Huertas LLC / kaosbeatz.

Newest version first. Most tools live under SHIFT+SCREEN 3 > KAOS TOOLS > More > KAOS TOOLS 2 (tile numbers are given where they matter).

## Current status

* **Latest build: v0.35** (MPC Mode, merged) on top of **v0.34** (Export + Sync, merged) on top of **v0.33** (Finder + Ideas, merged) on top of **v0.32** (audio / perform and files / undo / persistence, merged) on top of v0.31.1 (menu audit) and v0.31 (Color, Duck, Reference). Emulator-tested.
* **v0.33** is the build on the test card (v0.30.1 ran 70 min on hardware with no crash).
* **Hardware tested through v0.20 only.** Everything from v0.21 up has been tested in the emulator (QEMU lab) and has not run on the real unit yet.
* Every version below says which of three states it is in: emulator-tested, on hardware, or not merged.
* Notes on the version line: v0.15 to v0.31 are documented in detail. v0.1 to v0.9 are not recorded in the design notes or git (details not recorded). v0.10 to v0.14 are recorded only in short build notes.

## Known hard limits

These are walls of the platform, not bugs to be fixed later.

* **Stock inserts do not process audio.** Track and master stock inserts do nothing on this unit. Only pad inserts work. The layer ships its own master EQ, Loudness and Color instead.
* **No per-track audio.** The unit has no isolated track audio to tap, so there is no per-track kill EQ or per-track processing. Track stems are made by muting tracks and rendering.
* **No swap and no zram.** The device kernel is built without swap and compressed RAM, so RAM cannot be stretched. Track count and loop memory are bounded by real RAM (32 tracks shipped).
* **No licensing bypass.** The layer does not touch licensing or the boot chain. Features gated by the stock product (for example keygroup programs and MIDI tracks) stay out of reach.
* **The stock app owns undo.** Track create cannot be undone by the stock history, and layer DSP settings (EQ, Loudness, Color) are not in the stock undo list.

---

## v0.35 — MPC Mode (2026-11-07, merged; emulator-tested, not run on hardware)

A **MAIN VOLUME knob mode**, three **pad-grid edit menus**, three new **themes** and a **Remap page with presets**. Version line: KAOSAMPLE v0.35.

* **Volume knob (settings file `KAOSAMPLE/mpc.txt`, `volknob=`).** The knob arrives at the layer as panel CC 19. Modes: **Output** (stock: the knob stays the stock software volume), **Master fader** (the layer swallows the knob and drives the master fader of the Ch. Mixer, the layer master on Out 1/2) and **Both**. Soft takeover (the fader does not jump; the knob has to reach it first), slewed in 0.04 steps at >= 25 ms (no zipper). The master is remembered across restarts. The stock Out 1/2 track volume and the project `mixer.volume` cell were both tried first and are inaudible on this unit.
* **Edit menus: Track / Sequence / Program Edit as 4x4 grids matching the pads** (KAOS TOOLS 2 > MPC Edit, tile 14, or three new remap actions; inside a menu S1 / S2 / S3 switch menu). Press a pad to run, K1-K3 set the values, DATA pushes. Track: Clear, Explode, Double / Half Speed, Humanize, Random Events, Split Events, Pitch Quantize, Copy Track, Bounce to Sample, To Progression, Transpose. Sequence: Erase, Clear, Transpose, Insert Bars, Remove Bars, Half / Double Length, Copy Sequence, Copy Bars, Copy Events. Program: Duplicate, Flatten Pad, Copy Pads, Merge Programs, Pad Note Map, Pad Color, Save Program, Save Sample.
* **One stock undo step per action, or a refusal.** Everything that edits notes, bars, pads or the note map is one stock undo group. Actions the stock history cannot undo (they add a track or a sequence slot, or write a plain field: Copy Track, Explode, Duplicate Program, Copy Sequence, Pad Color) say so on the first press and run on a second press within 15 s. Copy Track, Duplicate Program and Bounce to Sample refuse under 250 MB of free memory; Save Program refuses when the card cannot hold the kit.
* **Themes One / 2000 / Renaissance** (own palettes, own pixel font, no Akai art): all layer pages and the edit menus; the stock screens stay untouched. One: red top bar with BPM / bar / beat. 2000: green-grey LCD, dark ink, block font, K1-K3 soft-key bar. Renaissance: black / charcoal, silver, red accents. They sit after MPC Live in the existing Theme toggle. A theme change asks "keep my remap / use the theme's preset".
* **Remap page** (icon menu page 2, "Remap"): the four combos plus a **Preset** row (Stock, MPC One feel, 2000 feel, Renaissance feel), K1 picks, S3 applies, saved to `remap.txt`.
* **Fixed on the way:** the stock Set-Pad-Note-Map call takes a one-word scope object (the Project), not the command factory; the first version crashed the app (SIGSEGV, exit 139). "Delete Bars" undo was refused by the layer's own track-delete guard because of its name (now "Remove Bars"). Double Length and Copy Events / Copy Track skipped every note (stale length cache, source notes counted as occupants).
* Limits: Flatten Pad: one undo step brings the original sample back, but the trim marks revert to the full sample. Bounce to Sample needs notes on the track and a pad (K1: first empty pad, or a pad you name). Hardware untested.

**Status**
* Emulator only (QEMU lab, Mac mini). **Hardware untested.** Not merged.
* Checked: host tests (mpcedit 15,300, skin 32,439, volknob 40,069 checks, 0 fail; the older suites rerun clean); every menu action run from the lab and undone, themes / remap presets / preset prompt on screen, 10.8 min soak. Evidence and numbers in `design/v035.md`.

---

## v0.34 — Export + Sync (2026-11-06, branch v034-export, not merged)

Two new tiles in KAOS TOOLS 2 (two of the hidden slots): **Export** (tile 12) and **Sync** (tile 13); slot 14 stays hidden. Version line: KAOSAMPLE v0.34.

* **Export: take a project to an MPC X / Live / One / Key or the MPC software.** Pick What (all sequences / current sequence / current track) and Stems (off / on), press S1. It writes `KAOSAMPLE/export/<project>-<date>/` with one Standard MIDI File per sequence (type 1, one track per KAOSAMPLE track, tempo and meter, 960 pulses per quarter), one `.xpm` drum program per drum track with its samples copied next to it (level, pan, tune, envelopes, filter, mute targets, up to four velocity layers), a README with the load steps and a manifest. Notes are `36 + pad` so the MIDI and the program always agree. The card's free space is checked first, S1 again cancels (the half-written package is removed), nothing is written outside `KAOSAMPLE/`. Stems on runs the v0.22 Stems job after the package. A standalone `.xpj` is not written: the format cannot be proven without a target device, so this is the always-safe package.
* **Sync: the missing MIDI clock parts.** The stock already has MIDI Sync Out and Sync In (Settings > MIDI: tempo, Start / Stop, SPP, Continue, one port at a time), so none of that is duplicated. Sync adds: **clock + Start / Stop / SPP / Continue on a second port** (DIN and / or USB) generated from the stock transport with lower jitter than the stock's clock (lab: rms 4 to 300 us against the stock's 0.5 to 1.2 ms), a **phase Offset** row, and a **monitor** (incoming tempo smoothed to 0.1 BPM, tick jitter, Start / Stop / Continue / SPP counters, the stock's own clock out). The generator never adds a second clock to a port the stock is already clocking.
* Limits: hardware untested; no real MPC X / Live / One was available to load the package; the USB clock path could not be seen on a host in the lab; the +9.5 ms default offset is a lab measurement; clock in is a monitor (the stock follows the clock).

**Status**
* Emulator only (QEMU lab, Mac mini). **Hardware untested.** Not merged.
* Checked: host tests (export 1,424, sync 3,409 checks, 0 fail; finder 152, ideas 572,854); all 15 KAOS TOOLS 2 tiles open from the real panel path; export packages validated by an independent reader against the engine's own note dump (0 problems), cancel, refusal on a full card, Stems on; Sync on the lab DIN jack (yield, phase against the stock clock, incoming 110.0 / 96.3 BPM, SPP + Continue); 10.5 min soak with no crash and the same process.

---

## v0.33 — Finder + Ideas (2026-11-06)

Two new tiles in KAOS TOOLS 2 (the two hidden slots): **Finder** (tile 3) and **Ideas** (tile 11). Version line: KAOSAMPLE v0.33.

* **Finder: find samples by sound.** Pick a pad, press S1: the 16 closest samples on the card, in the kits / stems folders and in the open project, nearest first (class, length, brightness, loudness, pitch). Filters: Class (kick / snare / hat / perc / one-shot / loop), Pitch (+-1 to 24 semitones), Length (shorter / longer). S3 auditions at -6 dB (press again to stop), S2 loads the result onto the pad as one undo step. The index (`KAOSAMPLE/finder/index.txt`) is built in the background, 20 files at a time, and only re-reads new or changed files.
* **Ideas: pattern ideas.** Styles boom-bap, trap, house, break, lofi and melodic; Density 1-5, Seed (same seed = same pattern), Bars 1 / 2 / 4, Target current or new track. S1 Make writes the notes into the sequence as one undo step, S2 Again undoes it and makes a new seed, S3 undoes the idea. Drum notes follow the pad classes of the kit; melodies use the Perform scale kit when one is built, otherwise C minor pentatonic; the Groove template (or the style's swing) is applied.
* Limits: a new-track Make leaves the track when you undo the idea (the stock cannot undo a track create from a group). Notes outside the sequence length are not written.

**Status**
* Emulator only (QEMU lab, Mac mini). **Hardware untested.**
* Checked: host tests (finder 152, ideas 572,854 checks, 0 fail); all KAOS TOOLS 2 tiles open from the real panel path; Ideas lab run 101 / 101; Finder search / filters / audition / load / undo; 10.5 min soak (244 actions) with no crash and the same process.

---

## v0.32 — Audio, Perform, Files, Undo (2026-11-06)

Merge of two builds, "audio / perform" (v0.32a) and "files / undo / persistence" (v0.32b). Version line: KAOSAMPLE v0.32.

**Audio and perform**
* **Duck from a set of tracks**, plus MIDI-in notes and arp / Perform hits as triggers (Color page).
* **Color is smoother**: parameter changes no longer click, and wow / flutter stay aligned with the dry signal.
* **Looper**: line-in recording, tap before or after the EQ / Color / Loudness chain, free-run, tempo-follow stretch (WSOLA), meters other than 4/4 (3/4, 6/8, 5/4, 7/8, 12/8). Exporting a loop no longer freezes the screen (187 ms down to 17 ms).
* **Perform**: Arp and Keys together; gate cut (pad parameter, Sample Play 2/3: 4.0 s down to 0.33 s); Pad MIDI In is switched on at boot.

**Files, undo, persistence**
* **Jump in lists** with SHIFT+DATA also in FX Select; **default template** from any project (not only a fresh one) and a STRIP option for the starter kit (81 MB down to 8.6 MB).
* **Sample Record undo / redo** works on the record screen itself (SHIFT+MINUS / SHIFT+PLUS). Undo waits until the stock history has the recording (about 1.2 s after you stop).
* **Mute scenes, slices and chords are saved with the project** (sidecar file next to the project, copied to the card; handled on Load, Save As, New Project, New from template).
* **Color undo history is saved** and comes back after a restart.

**Limits**
* Remembering the last browser folder across restarts is not possible safely (the stock stores nothing and key replay is unsafe). The 5/10/20 jump keys shorten the walk instead.
* Sample Record undo is refused until the stock history entry exists (about 1.2 s); an armed recording that is never triggered keeps undo blocked until you leave the screen.
* Time signature lists are not decoded; meters come from the engine's bar length. The loop is not processed twice when tapped after the chain, and is not limited (hard clip at full scale).
* Stock inserts, per-track audio, swap and licence limits from "Known hard limits" still apply.

**Status**
* Emulator only (QEMU lab). **Hardware untested.**
* Checked: all host tests pass; every KAOS TOOLS 2 tile and the first-menu switches opened from the real panel path; 14.5 min soak (824 + 82 random actions) with no crash, same process, memory flat (about 630 to 645 MB); Sample Record record / undo / redo hammered 20 times, 20 of 20 correct.

---

## v0.31.1 — Menu audit (2026-11-06)

* Reported from the unit: Record, Meter and Header "do nothing", iPad does not work. Every tile of the three icon menus was checked against its handler (table in `design/v0311.md`).
* **Record, Theme, Meter, Header are switches**: each shows a 1.5 s message ("Meter: On", "Record: Overdub", ...) and a tile that is On is drawn with a bright border and a green marker. Record really changes what a plain SEQ REC press does (overwrite eraser or plain overdub). The Meter bar is bigger and holds the peak briefly. Header is On by default, so its first push turns it Off.
* **iPad tile removed** (it only showed an address). About and Close moved up one slot each. Remap ids stay valid.
* Stock screens: "Song" removed (did nothing in the emulator); all other stock screens open. Two-word names no longer clipped. The DATA wheel skips empty slots on every menu page.
* Emulator only. Hardware untested.

## v0.31 — Color + Duck + Reference (2026-11-05)

* **Master Color** on the main output: tape (saturation, head bump, high roll-off, wow, flutter) and vinyl (warmth, mono below 120 Hz, crackle) with one Mix knob. Mix at 0 is exactly off and costs nothing.
* **Sidechain duck**: dips the output when a chosen track or pad plays. Depth up to 24 dB, attack 0.1 to 50 ms, release 10 to 1000 ms, three curve shapes. Triggered by live pad hits and by sequence notes.
* **Reference A/B**: load a WAV from `KAOSAMPLE/refs`, switch between your mix and the reference, and the level is matched by loudness (LUFS-style) so the comparison is fair. Trim knob included.
* New page "Color" (KAOS TOOLS 2, tile 10) with a settings undo/redo (32 steps) and SHIFT+ERASE reset.
* Settings are saved on the card and come back after a restart. The reference file name is remembered but never auto-loaded, and A/B always starts on your mix.
* Color sits before Loudness, so the limiter ceiling still holds. Stem renders include Color but not the duck or the reference.

Limits:
* Duck from the sequence reacts within about 2.6 ms of the true note time. Live pad hits within one audio block (2.9 ms).
* Duck sources are the duck track's notes and live pad hits on that track. It does not duck from other tracks, the arp or MIDI input yet.
* The loudness meter is LUFS-style, not a certified meter. It needs a few seconds of music to settle.
* Fast knob sweeps on Color can zipper slightly. High wow/flutter with mid Mix values combs a little.
* Reference files: 16/24/32-bit PCM or float WAV, up to 48 listed.
* Color/Duck undo is lost at restart (only the current state persists).

Status: emulator-tested, on branch `v031-color`, being merged into main. Not run on hardware.

## v0.30.1 — New from template safety (2026-11-05)

* "New from template" now refuses to run unless the app supervisor is healthy.
* Keeps a safety copy of the template's samples on the card before swapping, and repairs the project after the app relaunches.
* Old safety copies (`kaos-prev-*`) are pruned automatically.
* An exit guard stops the new project from being left half-built.

Limits:
* Born from a lab case where the stock app deleted sample files from a swapped-in project. The exact cause is not understood. The template folder on the card is never touched, so it is the copy to trust.
* The relaunch takes about 35 to 40 seconds of black screen.

Status: emulator-tested. On Omar's card for testing. In main.

## v0.30 — Files + Templates (2026-11-05)

* **Save as template**, **New from template** and **Default template** (templates hold the whole project including kits).
* **New folder** on the card or internal storage, named like the Name row.
* **Random project names** (adjective plus noun, never reuses a taken name), or a name from the current project.
* **Browser jump**: SHIFT+DATA jumps 5, 10 or 20 rows in the file browser.
* New page "Files" (KAOS TOOLS 2, tile 9).

Limits:
* A template is a full copy, about 90 MB each with the stock starter kit, and the same again as the new project. The layer refuses when space is short but never deletes old projects.
* Jump works on the Files page setup, not in every stock browser yet.
* New from template restarts the app.

Status: emulator-tested. Merged into main with Perform and Looper.

## v0.29 — Looper + Record to Seq (2026-11-05)

* **Multi-pass looper** (record, overdub, undo a pass), synced to the sequence, up to 8 passes.
* Records the master resample (what the unit plays).
* **Export to pad** or **To Seq**: the loop becomes a 24-bit WAV on a pad, on the current track, or on a new track, in one undo step.
* New page "Loop Rec" (KAOS TOOLS 2, tile 8). The older KAOS Looper page stays.

Limits:
* Records only the master resample, not the line-in/input. EQ, Loudness and master volume are not baked in.
* Runs only while the transport plays. A tempo change warns "re-record the loop". Only 4/4 tested.
* Pass start lands within about one pulse of the boundary.
* An export blocks the screen for 140 to 260 ms and the loop relocks once afterwards.
* Redo of a new-track export can fail when the stock history is full.
* Sample-record undo was not done.

Status: emulator-tested. Merged into main (v0.29.1).

## v0.28 — Perform (2026-11-05)

* **Scales**: 16 scales, pad N plays scale degree N, any root and octave.
* **Chords**: diatonic triads and 7ths, or your own set (16 chord shapes per pad), saved per sequence on the card.
* **Arp**: arpeggiator on a scale kit with cycle patterns, rates, octave stacking.
* **Keys**: play a pad program from a MIDI keyboard over 16, 32 or 48 pads.
* Gate sets the recorded note length. Everything is one undo step ("Perform setup").
* New page "Perform" (KAOS TOOLS 2, tile 6).

Limits:
* Gate cuts the tail only on one-shot pads that allow it. Drum pads keep their tail.
* Arp works only while the current bank is the kit bank.
* Keys and Arp are exclusive. Keys needs Pad MIDI In set to On.
* USB keyboard path tested only through the lab's MIDI port.
* About 180 parameter writes per chord apply: not stress-tested on the unit.

Status: emulator-tested. Merged into main (v0.29.1 merge).

## v0.27.1 — Clean up (2026-11-05)

* **Purge unused**: scans the project for samples nothing uses, lists them with total size, deletes with one undo step. Needs a double press within 6 seconds.
* **Make mono**: finds near-mono, one-channel and phase-inverted stereo samples and renders a mono copy, then re-points the pads.
* New page "Clean Up" (KAOS TOOLS 2, tile 7).

Limits:
* Purge refuses when a program is not a drum program.
* Files on disk are not deleted, only removed from the project pool.
* Make mono never overwrites the original: it makes `name_mono.wav`.

Status: emulator-tested. In main.

## v0.27 — Memory (2026-11-05)

* **Underrun counter** (audio dropouts) on the About page.
* **Memory probe**: writes the real device RAM size and kernel facts to the card (`mem-probe.txt`) on every boot. This finally records the real RAM.
* Auto-kit icon fix.
* Findings: no swap and no zram on the device kernel (hard limit). 48 tracks looks safe in the lab only if real RAM is at least 2 GB. 64 tracks is not shipped.

Limits:
* Track limit stays at 32.
* The emulator's audio clock gives start-up dropouts, so the counter says little about hardware.
* Opt-in swap code exists but does nothing on this kernel.

Status: emulator-tested. Merged into main.

## v0.26 — Retrospective record + Tap tempo + Save version (2026-11-05)

* **Retro record**: SHIFT+SAMPLE RECORD drops the last 1, 2, 4 or 8 bars of pad hits (or "auto") into the current track, quantized, even if you were not recording.
* **Tap tempo**: SHIFT+TAP TEMPO taps a tempo; plain TAP TEMPO still works as stock.
* **Loop BPM**: estimates the tempo of the last-hit loop (halve, double, or range) and sets it.
* **Save version**: one press saves `name-v1`, `name-v2` and so on, never overwriting.
* New page "Retro Rec" (KAOS TOOLS 2, tile 5).

Limits:
* Save version types the name into the stock Save screen blind, so it was tuned on the emulator. A real panel may be faster or slower. Names are lowercase, with dashes, digits and spaces only.
* A wrongly named project may exist on the card if typing fails (it never overwrites).
* The SHIFT combos were free in the layer, but whether the stock app uses them is unknown.

Status: emulator-tested. In main.

## v0.25 — Groove (2026-11-05)

* **Swing**: 50 to 75 percent on a 1/8 or 1/16 grid.
* **Humanize**: timing and velocity jitter that repeats for a given seed.
* **Groove extract** from a 1 or 2 bar loop sample, **apply** it to a track with a strength knob.
* **Groove library**: save, load and delete grooves as text files on the card.
* One Undo press reverts a whole apply.
* New page "Groove" (KAOS TOOLS 2, tile 2).

Limits:
* Extract needs a loop of exactly 1 or 2 bars that starts on the downbeat. Grid is fixed at 1/16.
* Tested with synthetic loops, not real drum loops.
* Applies to the current track only.

Status: emulator-tested. In main.

## v0.24 — Auto-chop + Auto-kit (2026-11-05)

* **Auto-chop**: finds hits in a loop automatically.
* **Auto-kit**: classifies hits as kick, snare, hat or other, and builds a 16-pad kit from a loop or from a folder of WAVs (up to 128 files).
* Six kit layouts and a start bank.
* A preview page lists each hit with its class and lets you fix it before it commits.
* One undo group per commit.
* New page "Auto-kit" (KAOS TOOLS 2, tile 4).

Limits:
* Classifier is about 88 percent right on one-shots and 82 percent on loop hits. Toms, rims and vocals land anywhere, so use the preview.
* Overlapping hits and clap double-bursts cause some misses and extras. Sensitivity and minimum hit are the knobs.
* Loop mode uses the last-hit pad's sample.

Status: emulator-tested. In main.

## v0.23 — KAOSAMPLE Companion (Mac app) (2026-11-05)

* Separate Mac app plus `kaoscli` (project `companion/`), not part of the MPC layer.
* Card status, layer logs and crash-log reader.
* Kit sync to the card with automatic sample conversion (sox).
* Stems browser for `KAOSAMPLE/stems`.
* Deduplicated project backups and safe eject.

Limits:
* GUI only launch-tested; CLI tested against a fake card.

Status: built on the Mac, not used with the real card yet.

## v0.22 — Stems + Song to Seq (2026-11-05)

* **Stem export**: every track and the master as separate 24-bit WAVs to the card, same length, with a bar range and tail option.
* Two engines: offline through the stock Song Export (fast, about 1 second per 21 seconds of audio, respects mutes), or real time through the layer's audio tap (can include EQ and Loudness).
* Free-space check, panel lock, progress and cancel.
* **Song to Seq**: bounces a song (sequence list) into one new sequence in one undo step, checked by count and checksum.
* New pages "Stems" and "Song to Seq" on a new KAOS TOOLS 2 page (stems page is screen 27, song page 28).

Limits:
* The stock Song Export adds a fixed 3 second tail and always starts at bar 1.
* Offline renders skip the layer's EQ and Loudness. Offline and real-time levels differ by about 4 dB, so do not mix files from both.
* The stock master Color compressor changes the sum. Bypass it before rendering stems or they will not add up to the master.
* Real-time renders differ by one frame here and there between passes.
* Track mutes stay changed if the app is killed mid-render.

Status: emulator-tested. In main.

## v0.21 — FX and Loudness (2026-11-05)

* **Pad inserts**: up to 4 per pad from the stock 97-effect catalogue, grouped by family, with a name filter.
* **Master Loudness**: 3-band compressor, saturation, lookahead limiter, "Loud" macro, presets, and a level-matched A/B.
* Master EQ and Loudness settings are now saved on the card.
* v0.20 limit fixes: Hold/Sustain/Release now work (envelope type), live knob sweep, real filter type names, sample pool browser.
* New tiles "FX" and "Loudness" replace the old Color Comp and Resource Info tiles (the stock screens remain under Stock screens).

Limits:
* Track and master stock inserts do nothing (hard limit). No effect parameters, no readback of a slot.
* Insert undo does not remove the processor (use ERASE).
* Uses a null-pointer guard in the stock command context, same technique as track delete.

Status: emulator-tested. Cost is about 0.3 percent of the audio block budget.

## v0.20 — Pad sound design (2026-11-05)

* One page for one pad: level, pan, tune (plus or minus 12 semitones).
* Amp envelope (AHDSR), filter (type, cutoff, resonance) and filter envelope.
* Choke groups: up to 4 mute targets, mutual, with learn.
* Up to 4 velocity layers with range, semitone, fine, level and pan.
* All edits are stock undoable pad parameters, read back from the engine, which also fixes the tune readback.
* Old page "Pad Edit" became "Pad Sound" (KAOS TOOLS tile 2).

Limits:
* Hold, Sustain and Release are silent on drum pads (fixed in v0.21).
* Knobs apply 220 ms after they rest (fixed in v0.21).
* Drum tracks only.

Status: on hardware (tested through this version). Emulator-tested first.

## v0.19 — Sample tools (2026-11-05)

* **Chop** a sample into up to 128 slices with a waveform view, zero-crossing snap, a minimum-length guard, and threshold detection.
* **Extract** a chop to a pad with no new WAV (start/end only).
* **Reverse** and **Tail**, plus **Fade** (renders a faded copy into a new file).
* **Copy pad** to another track.
* Sample name shown under each pad on the pad grid.
* No WAV in the project is ever changed.

Limits:
* Slice sets are stored on the card (24 kept), not in the project.
* The stock Tail is a loop tail, not a fade. Fade adds a sample to the project.
* Extract writes layer 1 only.
* Detection is tuned on synthetic hits, so noisy loops need Sens/Min adjustments.

Status: on hardware (tested through v0.20, this version included). Emulator-tested first.

## v0.18 — Tracks pro (2026-11-05)

* **Solo**, **rename** (free text typed from the pads), **move** (reorder), and **delete** tracks.
* **Mute scenes**: save and recall per-sequence mute patterns.
* **Recorded mutes**: pad and track mutes can be recorded into the sequence.
* Paged 16-track grids for Tracks and Track Mute.
* Optional track limit build setting (default 32).
* Pages "Tracks Pro" (tile 13) and "Mute Scenes" (tile 14).

Limits:
* Track delete cannot be undone (use Snapshots) and refuses a track that shares its kit.
* Scenes and recorded mutes live on the card, not in the project file.
* Pad mutes record for the current track only. Scenes are track mutes only.
* 32 tracks with full kits use about 1.7 GB in the lab.

Status: on hardware (tested through v0.20). Emulator-tested first.

## v0.17 — Step edit pro (2026-11-05)

* Copy, cut, paste, duplicate notes across bars and tracks (CHOP, SHIFT+CHOP, LOOP, SHIFT+LOOP).
* Bar menu: copy, paste, insert empty bar, insert copied bar, delete bar.
* Zoom from 1/64 to 1 bar (16 bars wide), pad banks B to D.
* One grid for pads and tracks, selection with a white outline.

Limits:
* 4/4 only. Quantize still snaps to 1/16.
* Selection limit 256 notes. Bar ops refuse tracks with over 4000 events.
* A cut of 16 notes needs 16 undo presses.

Status: on hardware (tested through v0.20). Emulator-tested first.

## v0.16 — Undo, snapshots, monitor (2026-11-05)

* **Undo/redo** with a popup (such as "Undo 2/3") and a named **history list** you can jump around in.
* **Snapshot restore** on the device: restore any of 30 card snapshots from the MPC itself.
* **System monitor** on the About page: CPU, RAM, battery, temperature.
* **Load kit from card** (16 of 16 pads, survives save and reload).
* Faster feedback for newly recorded notes (a provisional note is drawn at the pad hit).
* **Explode** from the mixer or grids (SHIFT+ERASE). SHIFT options overlay.
* Crash hardening for a display problem seen on hardware, with better crash logs.

Limits:
* Stock undo cannot undo track create, and explode is only partly undoable.
* Root cause of the hardware display crash was not found.

Status: on hardware (tested through v0.20). Emulator-tested first.

## v0.15 — Overwrite fix + Master EQ (2026-11-05)

* **Overwrite recording fixed**: it used to erase only 1 of about 20 old notes; now it clears all of them, handles loop wrap, and a second REC press punches out.
* **Master EQ**: 3 bands, plus or minus 12 dB, a kill per band, 24-band spectrum analyser (screen 17).
* 16 tracks save and reload.
* Explode keeps the source muted and names tracks after samples.
* Card kit load.

Limits:
* Overwrite on several armed tracks is only tested on one.
* Each erase is one undo step.

Status: on hardware (tested through v0.20). Emulator-tested first.

## v0.10 to v0.14 — Early tools (2026-11-04 to 2026-11-05)

Recorded only in short build notes; detail is partial.

* **v0.14 Step edit**: edit notes on the Pad and Track grids (nudge, length, velocity, quantize, clear selection).
* **v0.13 Save kits**: save a kit to the card the MPC X way (details not recorded).
* **v0.12 Programs page**: a page for programs (details not recorded).
* **v0.11**: (details not recorded).
* **v0.10**: Master strip on the Channel Mixer (DATA past the last track, K1 sets master volume). Pad Edit, Seq Ops (copy, colour, rename), MIDI, and Looper pages. KAOS TOOLS page opened from the "More" tile. Remappable combos.

Status: on hardware (tested through v0.20).

## v0.1 to v0.9 — Foundations

* (details not recorded)

Status: on hardware (tested through v0.20).

---

## Roadmap

### v0.32 — No-limits pass
* Duck: all tracks, arp and MIDI as sources.
* Color: parameter smoothing (no zipper).
* Looper: line-in input, run without the transport, tempo stretch, other meters, no glitch after export.
* Arp and Keys together.
* Gate cuts tails.
* Auto Pad MIDI In.
* Remember the last folder.
* Jump in all browsers.
* Default template from any project.
* Slim templates.
* Sample-record undo.
* Persistent FX undo.
* Mute scenes, slices and chords saved inside the project.
* 48 to 64 tracks if RAM allows.

### v0.33
* Sample finder by sound.
* Pattern ideas.

### v0.34
* Export to X, Live and One.
* MIDI sync.

### v0.35
* Self-heal.
