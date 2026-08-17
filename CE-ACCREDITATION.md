# CE accreditation — what is built and what is not

**IV Rejuve is not an accredited CE provider.** Nothing in the app claims CE
credit. The platform below is the machinery an accredited provider needs; the
accreditation itself is an application to an accrediting body and is not
something code can deliver.

## What the app does today

| Requirement | Status | Where |
|---|---|---|
| Contact-hour tracking | Built | `ce_activities.contact_hours`, shown on the certificate |
| Conflict-of-interest disclosure | Built | Shown and acknowledged **before** the first lesson of a course |
| Mandatory post-activity evaluation | Built | Blocks the certificate until submitted |
| 6-year retrievable learner records | Built | `ce_records`, append-only, `retain_until` = completion + 6 years |
| CE Broker (Florida) reporting | **Export built, spec unconfirmed** | Admin → CE records → Export CE Broker CSV |
| Accreditation itself | **Not done — see below** | — |

## How the flow runs

1. **Disclosure.** Opening the first lesson of a course presents the
   conflict-of-interest and disclosure statement from `ce_disclosures`. The
   learner must tick to acknowledge; the acknowledgement records the disclosure
   *version*, so publishing a new version re-prompts everyone.
2. **The activity.** Lessons, interactive practice, knowledge checks, the skills
   rehearsal, then the final exam at 80%.
3. **Evaluation.** Passing the exam routes to a required 8-question evaluation
   covering objectives met, relevance, teaching effectiveness, **commercial
   bias**, actual time taken, and intended practice change. The certificate
   cannot be reached until it is submitted.
4. **Record.** Issuing the certificate writes a `ce_records` row: learner,
   course, contact hours, exam score, credential ID, completion timestamp,
   `retain_until`, and `accredited_at_issue`.
5. **Reporting.** Admin → CE records lists every completion with filters for
   not-reported and missing-licence, plus two exports.

## Design decisions worth knowing

**Records are append-only.** `ce_records` grants learners INSERT and SELECT and
grants admins UPDATE (only to stamp `reported_at` / `report_batch`). **No role
holds a DELETE policy**, so a completion record cannot be erased through the API
— which is the point of a retention obligation.

**`accredited_at_issue` is stored per record.** A completion issued today, before
accreditation, is permanently marked as such. When accreditation lands, old
records cannot be silently re-read as accredited CE.

**One switch governs all CE wording.** `ce_activities.accredited` is false for
every course. `ceIsAccredited()` in `training.html` reads it, and it is the only
thing that should ever be allowed to turn "contact hours" into "CE credit".

## Before this can be used for real CE

1. **Get accredited.** For nursing this is typically an ANCC-accredited approver
   or a state board provider approval; Florida CE for nurses runs through the
   Board of Nursing and CE Broker. This is an application with fees, planning
   documentation, and evidence of the exact processes above. Budget months.
2. **Fill in the identifiers.** Once issued, set `provider_number` and
   `course_number` on each row of `ce_activities`, and mirror them into
   `CE_PROVIDER_NUMBER` / `CE_COURSE_NUMBERS` in `admin.html`.
3. **Confirm the CE Broker file spec.** The export writes the fields their
   reporting has required (licence number, licence type, state, name, completion
   date, hours, provider number, course number). **Their template is issued per
   provider and has changed over time — confirm the current columns with CE
   Broker before filing.** Treat the current export as a strong starting point,
   not a verified submission format.
4. **Collect licence numbers.** `profiles` now has `license_number`,
   `license_type` and `license_state`, but nothing in the learner UI asks for
   them yet, because there is no real sign-in. Reporting to CE Broker is
   impossible without them — the admin list flags records missing one. Add these
   fields to the profile step when Google OAuth goes live.
5. **Turn the switch.** Set `accredited=true`, `approved_from` and `approved_to`
   on the approved activities. Only then does the app describe anything as CE.

## Known gaps

- **No real authentication yet.** `training.html` runs `DEMO_LOGIN=true`, so CE
  state persists to `localStorage` and the Supabase writes no-op. The sync
  functions are written and will start working the moment a real session exists
  — but until then there is no server-side record, and **`localStorage` is not a
  6-year retention store**. Real records begin at real sign-in.
- **Licence capture is not in the learner UI** (see step 4).
- **Evaluation responses are not yet surfaced in admin.** They are stored in
  `ce_evaluations`; there is no reporting screen over them. Accreditors ask for
  aggregate evaluation data, so this will be needed.
- **No certificate revocation path.** Append-only was the deliberate choice; if a
  completion ever needs voiding, that wants a separate `ce_voids` table rather
  than a delete.
