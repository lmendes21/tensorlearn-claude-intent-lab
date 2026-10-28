# Intent: Results-ready notification (PRD N-17)

Sources: `inputs/01-approved-prd.md` (approved PRD N-17, 2026-09-18) and `inputs/02-current-meeting.md` (discovery notes, 2026-09-20). The early request in `inputs/03-superseded-request.md` is superseded by PRD N-17 and is used only as history; nothing in this intent is approved on its basis.

## Problem

The fictional clinic receives avoidable calls from patients checking whether their results are ready. These status-only calls take up staff time even though the answer could reach the patient without a call.

> "The fictional clinic receives avoidable calls from patients checking whether results are ready." (PRD)

## Proposed outcome

- **I-1: Patient is told when a result is ready to view.** The patient is notified when a result is ready for their review in the existing authenticated portal.
  > "Notify the patient when a result is ready for their review in the existing authenticated portal." (PRD)
- **I-2: "Ready" means reviewed and released by a doctor.** A notification becomes eligible only after a doctor has reviewed **and** released the result. Completion of the laboratory upload alone never triggers a notification, and neither does review without release.
  > "A doctor must review and release the result before any notification is eligible." (PRD)
  > "'Ready' must not mean that the laboratory upload merely finished." (product owner, discovery notes)

### How success will be observed

- **Status-only calls:** the number of calls asking whether results are ready goes down, counted separately from clinical questions and compared against a measured baseline.
  > "count status-only calls separately from clinical questions; compare against a measured baseline." (support lead, discovery notes)
- **Delivery failures:** notification delivery failures are measured and go down.
  > "Measure reduction in results-status calls and delivery failures." (PRD)
- **No target is approved.** The baseline and the numerical target are still to be agreed during discovery, so this intent sets neither a target nor a deadline.
  > "The baseline and numerical target will be agreed during discovery; none is approved yet." (PRD)

## Affected users and systems

- **Patients:** the only approved recipient of the notification.
- **Doctors:** their review and release of a result is the gate for any notification. Their workflow does not change.
- **Support staff:** they handle the status calls this aims to reduce and provide the call counts used to measure success.
- **Existing authenticated portal and its result route:** where the patient views the result.
  > "Use the existing portal result route." (PRD); "the existing portal path is adequate." (engineering lead, discovery notes)
- **Notification delivery and its operational logs:** they send the message, prevent duplicates and handle failures without exposing clinical content.
- **Owners:** product owner, privacy owner, clinical owner, engineering lead and support lead.

### In scope

- **I-5:** use of the existing portal result route.
- Duplicate prevention for the same released result version.
- Handling of delivery failures without duplicate patient messages.

## Out of scope

The following are excluded from this work:

- Portal redesign.
- Appointment scheduling.
- Clinical interpretation of results.
- Notifying third parties.
  > "Portal redesign, appointment scheduling, clinical interpretation and notifying third parties are outside this approved scope." (PRD)
- A new or rebuilt patient dashboard: "A new patient dashboard is a separate project." (engineering lead, discovery notes)
- Anything taken only from the superseded early request: sending on lab upload, putting test results in a message, copying the referring doctor, rebuilding the dashboard and its call-reduction target.

## Constraints

- **I-2: Review-before-notification gate.** No notification before a doctor has reviewed and released the result.
- **I-3: No clinical details.** The notification only says that an update is available and directs the patient to the existing authenticated portal. No diagnosis, test result, clinical measurement or treatment detail appears in the message, the notification logs or the operational logs.
  > "No diagnosis, test result, clinical measurement or treatment detail goes in the message or notification logs." (PRD)
  > "no sensitive content in messages or operational logs." (privacy owner, discovery notes)
- **I-4: Authenticated access only.** Results are viewed only in the existing authenticated portal. No attached results and no public result link.
  > "Authenticate access in the existing portal. Do not attach results or introduce a public result link." (PRD)
- **No duplicates.** At most one notification per released result version, including when delivery fails and is retried.
  > "Avoid duplicate notifications for the same released result version; handle delivery failures without duplicate patient messages." (PRD)
- **Recipients.** Only the patient is approved. No other recipient is added until its open question is decided by its owners.

## Open questions

- **OPEN-1: Channel (SMS, email or portal push).** Owners: **product owner and privacy owner**. They decide after the consent and delivery review. Until then no channel is assumed.
  > "Channel (SMS, email, portal push): product owner and privacy owner must decide after consent/delivery review." (PRD)
  > "Consent and the actual channel are still under review." (privacy owner, discovery notes)
- **OPEN-2: Notifying the referring doctor.** Owners: **clinical owner and privacy owner**. They decide recipient authorization. This is not approved by the PRD and is not inferred from the superseded request.
  > "Notification to the referring doctor: clinical owner and privacy owner must decide recipient authorization. It is not approved by this PRD." (PRD)
  > "do not infer approval for referring-doctor notifications from the old request." (clinical owner, discovery notes)

## Decision record

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```
