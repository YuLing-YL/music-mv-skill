# MV Self-Check Checklist

Verify item by item after each rendered shot and before final delivery. If any item fails, do not proceed to the next step.

## 1. Beat Verification

- [ ] Storyboard timing covers the specified scope: full-song mode starts at 00:00.00 and ends at song end; clip mode covers clip_start~clip_end (or the range derived from the section name). No gaps, no overlaps.
- [ ] Every marked beat trigger actually lands on a beat point (cross-check beat/downbeat times in the beats JSON).
- [ ] Downbeats map to flash/zoom/particle burst; low-frequency sections to slow flow; sustained notes to slow push + gradient.
- [ ] Chorus cutting frequency is clearly higher than verses; quiet sections use slow camera movement.

## 2. Style Consistency Verification

- [ ] All shots use the same locked style descriptor (locked once).
- [ ] Core imagery/characters keep the same design and palette in every shot — no mid-way style switch.
- [ ] Color tone may vary in brightness/saturation across sections, but the color family never changes.

## 3. Shot Pacing Verification

- [ ] Verse shots 8~15s each; chorus 2~5s; quiet sections may stretch to 20s.
- [ ] Adjacent shots are not identical (composition or camera movement must differ in at least one aspect).
- [ ] The first chorus shot has a clear visual burst (flash / zoom / particle burst — at least one).

## 4. Timing Error Verification

- [ ] Every beat trigger is within 0.1s of its beat point.
- [ ] Concatenated total duration differs from audio duration by ≤ 0.5s (clip mode: within 0.5s of the clip duration).
- [ ] Segments share identical frame rate, resolution, and encoding parameters — no frame jumps at splice points.

## 5. Prohibitions

- ❌ Rendering any shot when need_confirm=true and the user has not confirmed.
- ❌ Adding subtitles without an explicit user request.
- ❌ Switching visual style or core imagery/characters mid-way.
- ❌ Stuffing full beat data or per-frame data into context (persist to JSON, keep only summaries).
- ❌ Fabricating lyrics (if LRC is missing, ask the user first).
- ❌ Delivering without self-check, or hiding degraded shots from the report.
- ❌ Generating the whole song in one pass (must render shot by shot).
- ❌ Retrying indefinitely after more than 2 failed self-checks (degrade and log instead).
