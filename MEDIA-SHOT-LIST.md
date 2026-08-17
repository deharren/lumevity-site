# Media shot list

The lesson media framework is built and wired. What is missing is the assets.

This list is generated from the `MEDIA` map in `training.html` — that map is the
source of truth, so update the map and regenerate rather than editing this file
in isolation.

## How to deliver an asset

1. Drop the file in `media/` in this repo (create the folder).
2. Open `training.html`, find the `MEDIA` entry for the lesson, and add `src`:

```js
iv_procedure:[
  {k:"video", src:"media/iv-insertion.mp4", poster:"media/iv-insertion.jpg",
   need:"...", alt:"Peripheral IV insertion", cap:"<b>Insertion, start to finish.</b> ...",
   credit:"Filmed at ... , 2026"}
]
```

3. That is all. The slot renders automatically as a lesson step, lazy-loads, and
   opens in a lightbox on tap.

Until `src` is set, the slot is **skipped entirely** — learners never see an
empty frame. To preview the outstanding list in the app, set
`MEDIA_PLACEHOLDERS=true` in `training.html`.

## Format notes

- **Photo:** JPEG or WebP, 1600px wide is plenty, 16:10 crops best.
- **Video:** self-hosted **MP4 (H.264 + AAC)** with a `poster` still. Keep clips
  under ~60s and under ~8 MB where possible. Video is `preload="none"`, so
  nothing downloads until a learner taps.
- **No third-party embeds.** YouTube/Vimeo iframes would introduce third-party
  tracking on a page carrying learner records. Self-host.
- **Always set `alt`.** It is the only description a screen-reader user gets.
- **Set `credit`** where a source or filming location must be attributed.

## Consent and licensing — read before shooting

- Any identifiable person needs a signed model release covering educational use.
- Anything filmed on real patients needs documented patient consent; prefer
  training arms, injection pads and manikins, which sidestep this entirely.
- Stock injection photography is frequently **wrong on technique** (bad angle,
  dorsogluteal sites, recapping, no Z-track). Do not buy stock without a
  clinician reviewing the actual frame against the lesson it illustrates.
- Every asset should be signed off by a qualified clinician before it ships. A
  photo that contradicts the lesson text teaches the wrong thing more
  effectively than the text teaches the right one.

---

## Outstanding assets — 14 slots across 12 lessons

### IV Insertion

| # | Lesson | Type | Brief |
|---|--------|------|-------|
| 1 | Venous anatomy & physiology | Photo | Forearm with a tourniquet applied, veins visible, arrows marking the cephalic, basilic and median cubital veins. |
| 2 | Patient assessment & consent | Photo | Two-hand palpation technique on a forearm, showing the assessor feeling rather than looking for the vein. |
| 3 | Site selection strategy | Photo | Side-by-side of a good straight forearm target versus a vein crossing the wrist joint. |
| 4 | Preparation & asepsis | **Video** | 30–45s: hand hygiene, glove technique, chlorhexidine applied with friction, and the full air-dry wait shown in real time. |
| 5 | The insertion, step by step | **Video** | 45–60s close-up on a training arm: bevel-up entry at 10–30°, flashback, angle dropped, advance 2–3 mm, catheter threaded, tourniquet released, stylet withdrawn and safety activated, flush and secure. |
| 6 | The insertion, step by step | Photo | Macro of the flashback chamber at the moment blood appears. |
| 7 | Complications & troubleshooting | Photo | Clinical photos: infiltration (pale, swollen, cool) beside phlebitis (red, warm, palpable cord). |

### IM Injection

| # | Lesson | Type | Brief |
|---|--------|------|-------|
| 8 | Needle, syringe & drawing up | Photo | Needle lengths laid out side by side (⅝, 1, 1½ inch) with a scale, and a 3 mL syringe drawn to 1.5 mL read at eye level. |
| 9 | Site selection & landmarking | **Video** | 30s: ventrogluteal landmarking — palm on the greater trochanter, index to the anterior superior iliac spine, middle finger along the iliac crest, injecting into the V. |
| 10 | Site selection & landmarking | Photo | Deltoid site measured 2.5–5 cm below the acromion, with the landmark marked. |
| 11 | Z-track technique & aspiration | **Video** | 30–45s: Z-track on a training pad — skin displaced ~2.5 cm, 90° insertion, slow injection, 10s pause, withdrawal, then release. |

### Subcutaneous Injection

| # | Lesson | Type | Brief |
|---|--------|------|-------|
| 12 | Site selection & rotation | Photo | Abdomen with the 5 cm periumbilical exclusion zone marked and a rotation pattern overlaid. |
| 13 | Pinch, angle & injection | **Video** | 25–35s: pinched abdominal skin fold, 45° and 90° insertions compared, injection, and release of the fold. |
| 14 | Complications & troubleshooting | Photo | Lipohypertrophy at an over-used injection site, ideally with raking light so the raised area reads clearly. |

**Totals:** 9 photos, 5 videos. Slots 4, 5, 9, 11 and 13 are the ones that carry
the most teaching weight — if the budget only covers part of the shoot, film
those five and add photos later.

Slot 7 (infiltration vs phlebitis) and slot 14 (lipohypertrophy) are the hardest
to obtain ethically, since they need real affected tissue. Options are a clinical
image library with an education licence, or a consented patient photograph. Do
not approximate these with makeup or illustration and present them as clinical
photographs.
