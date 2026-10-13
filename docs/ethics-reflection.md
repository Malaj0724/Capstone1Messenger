# Messenger — Ethics Reflection

**Course:** CPS 490 Capstone I, Fall 2026 · **Assignment:** IA 05
**Author:** Mason · **Date:** October 12, 2026

Requirement identifiers (`FR-nn`, `NFR-nn`, `RA-nn`, `OQ-nn`) refer to `docs/requirements.md`; `SOW` refers to `docs/statement-of-work.md`.

## Scope of this analysis

Messenger, as defined in the SOW and requirements, is a text-only messaging system with private conversations and flat groups. Anyone can register (SOW A5), users can find each other by username prefix (FR-10), messages are kept indefinitely (OQ-04), and there is no moderation, blocking, or reporting (RA-16). It is delivered as a local demonstration at small scale (NFR-04, RA-14).

Impacts are judged at two levels. The first is the prototype as delivered, which is run locally for a few users. The second is what would change if the same design were deployed publicly. Where I call an impact limited, I say which of these two levels the judgment holds for.

---

## 1. Design Impact Analysis

### 1.1 Cultural

**Positive.** Messenger gives groups such as friends, classes, and families a shared, persistent place to talk, with history they can return to. Because it has no engagement features such as feeds, reactions, or recommendations, communities set their own norms for how they use it.

**Negative.** The interface is English only (RA-13), so people who do not read English are excluded from using it comfortably. With no moderation (RA-16), nothing accounts for different cultural norms about acceptable speech; a message that is a joke in one group may be abusive in another, and nobody but the participants can judge it.

**Mitigation.** Message text should be stored and displayed as Unicode so people can write in any language, even though the interface itself is English. This is a design decision for `docs/design.md` and costs nothing extra. Localization stays out of scope (SOW §3.2). Cultural context is left to the participants of each conversation, and the lack of moderation is recorded as a limitation (OQ-08).

**Significance.** Moderate for the prototype: the main effect is who can comfortably use it.

### 1.2 Economic

**Positive.** Messenger costs its users nothing and needs no paid external service (SOW §3.2 excludes integrations). A small team or class can coordinate without buying a product, and the single-instance design (RA-12) keeps the cost of running it to one ordinary machine.

**Negative.** There is no direct economic harm at this scale: it handles no payments, shows no advertising, and collects no data to sell. Two smaller effects exist. Users cannot export their history (there is no export function), so a community that builds up a long history is tied to the service. Real-time messaging can also be a distraction from paid or academic work.

**Mitigation.** The lack of engagement mechanics reduces the distraction risk: there are no push notifications (SOW §3.2), unread counters, or presence indicators. Export is recorded as future work, not built.

**Significance.** Limited. This conclusion holds because the system is a free, local, small-scale prototype with no payment, advertising, or data-sale function. It would need to be reconsidered if it were operated as a service with its own costs and business model.

### 1.3 Environmental

**Positive.** Because it is web-based (SOW A2), it runs on devices people already own and needs no new hardware or installed software.

**Negative.** Resource use at the prototype's scale is the energy of one small server process serving about ten users (NFR-04), which is negligible. Two things would grow with scale. If real-time delivery is done by polling (RA-06 leaves this open), every online client makes constant requests whether or not anything has arrived. Keeping all messages forever (OQ-04) also means storage grows without bound.

**Mitigation.** The delivery mechanism should avoid needless requests, either by using a push connection or a polling interval no shorter than the 5-second target requires (FR-17, NFR-01); this belongs in the design document. A retention period, if the client asks for one, would limit storage growth (OQ-04).

**Significance.** Not significant for the prototype, for the reason above. It becomes worth attention only at large scale, where polling frequency and unlimited retention both have real energy and storage costs.

### 1.4 Global

**Positive.** A browser-based system can be reached from anywhere without installation, so geographically scattered groups can talk in one place.

**Negative.** Two things depend on where it runs and who uses it. Message timestamps (FR-15) come from the server's clock, so users in different time zones may read them incorrectly unless the display is handled with care. More importantly, privacy and online-content rules differ by country, and a system holding people's private messages may be subject to rules the project has not examined.

**Mitigation.** Timestamps should be stored in a single standard time and shown in each user's local time; this is a design decision. The legal question is bounded by assumption RA-14: the system is a local demonstration and is not exposed to the public internet. Public deployment would first require a review of applicable rules and transport encryption (OQ-03).

**Significance.** Limited for the prototype because of RA-14, and significant if the system is ever deployed publicly, which this project does not do.

### 1.5 Public health

**Positive.** Staying in touch with friends and family and having a group to talk to can reduce isolation and support mental well-being.

**Negative.** Abuse and harassment through private messages or groups can cause stress, anxiety, and withdrawal; this is the main public-health concern and is analyzed in Section 2.1. Constant, real-time messaging can also disrupt sleep and attention for some users. Messenger has no way to recognize or respond to a user in distress.

**Mitigation.** The harassment mitigations in Section 2.1 apply. Design choices that avoid compulsive use are already in the scope: no push notifications, no presence indicators, no unread counters, and no read receipts that pressure people to reply. The system does not attempt to detect or respond to distress, which would need moderation tooling that is out of scope (RA-16).

**Significance.** Moderate, tied closely to the harassment risk.

### 1.6 Public safety

**Positive.** Group messaging can be used to coordinate everyday logistics and to check on people, such as arranging a ride or confirming that someone arrived.

**Negative.** People might rely on Messenger for urgent communication, but it makes no delivery or uptime promise beyond a single local instance (RA-12). A message that does not arrive could be mistaken for one that was read. Threats or dangerous content can be sent with nobody empowered to see or act on them (RA-16). A stolen account could be used to send misleading messages that appear to come from a trusted person (Section 2.3).

**Mitigation.** The README (SOW D9) should state plainly that Messenger is not an emergency service and offers no delivery guarantee beyond what the requirements say. The storage-failure behavior (NFR-11) at least ensures a sender is told when a message was not sent. The account-security measures in Section 2.3 reduce impersonation. Threat reporting is not built and is recorded as a limitation (OQ-08).

**Significance.** Moderate in principle, limited in practice at this scale and with this audience; the README notice is the proportionate response.

### 1.7 Public welfare

**Positive.** It is free, needs only a browser, and works on modest devices, which makes it available to many people, including students and small community groups.

**Negative.** Some people are left out or locked out. Formal accessibility work is out of scope (SOW §3.2), so users who rely on screen readers or keyboard navigation could find the interface hard or impossible to use. There is no password recovery (RA-15, OQ-07), so a user who forgets a password permanently loses access to their conversations and groups. People without reliable internet access cannot use a real-time web service at all.

**Mitigation.** Basic accessibility costs very little when done during construction: labeled form fields, keyboard operation of every function, and readable contrast. These are design guidelines for the interface, not a certification, so the SOW exclusion stands. The lack of recovery is a documented limitation that the client is asked about (OQ-07). Offline use is not addressed.

**Significance.** Moderate. The main effect is exclusion, and it is partly avoidable at low cost.

### 1.8 Social

**Positive.** Private conversations and groups let people maintain relationships and organize together. Group creators choose their members (FR-21), so groups can be small and trusted.

**Negative.** The same openness enables unwanted contact. Anyone with an account can find any other user by username prefix (FR-10), start a private conversation with them (FR-11), or add them to a group (FR-21) without their agreement. Case-insensitive uniqueness (RA-01) blocks names that differ only by capital letters, but similar names such as "alice_1" and "alice1" can still be used to impersonate someone. A group creator can also exclude a person from a group simply by not selecting them, which can be used to leave someone out deliberately.

**Mitigation.** See Section 2.1. Users can leave any group (FR-27), search is limited so it cannot list everyone (FR-10), and a message rate limit (NFR-12) limits flooding. Exclusion from a group is a normal social action and is not something the system should prevent.

**Significance.** High, because harassment is the most likely serious harm the system could cause.

---

## 2. Connecting Ethics to Engineering

Three negative impacts are significant enough to affect the engineering work.

### 2.1 Unwanted contact and harassment

**Who could be affected.** Any registered user. Those most exposed are people who can be found through search (FR-10), messaged without invitation (FR-11), or added to a group by someone else (FR-21), and members of a group in which another member sends abusive content.

**Potential harm.** Repeated unwanted or abusive messages, flooding a person's conversations, and distress that leads a person to stop using the system. A person added to a group sees its content until they leave.

**Mitigation.**
- *Already in the requirements:* a user can leave any group and then receives nothing further from it (FR-27); only authenticated users can contact anyone (FR-09); search returns at most 20 usernames, requires at least one character, and cannot list everyone (FR-10, RA-07); messages are limited in length (FR-14).
- *New, from this analysis:* a rate limit of 30 messages per 60 seconds per user (NFR-12, verified by AC-26). This limits flooding. It does not stop a determined person from sending a small number of abusive messages, so it reduces the harm rather than removing it.
- *Considered and not adopted:* blocking, reporting, and moderation. A minimal block feature (a blocked user cannot start a conversation with, message, or add the blocker to a group) would address the harm most directly. I have not added it because it touches several functions (conversation opening, sending, and group creation) and the schedule has one developer and one day of contingency (SOW A1, C7, M5). It is recorded as an accepted limitation (RA-16) and put to the client as OQ-08. If the client answers that it is required, block is the first addition.
- *Operational policy:* the system is run locally by an operator (RA-14). A complaint about abuse is handled by the operator removing the account's data directly, outside the application, since the application has no administrator function (RA-16). The README (SOW D9) states this policy.

**What it should influence.** A requirement (NFR-12, AC-26), a scope decision (keeping block and moderation out, with the reason recorded), and an operational policy.

### 2.2 Privacy of messages and personal data

**Who could be affected.** All users, especially those discussing sensitive matters in private conversations, and members of groups.

**Potential harm.** Private messages could be read by the person operating the system, copied from the data store, or intercepted on the network. End-to-end encryption is out of scope (SOW §3.2), so messages are stored in a readable form. Transport encryption is not required for the local demonstration (RA-14, OQ-03). Messages are kept indefinitely and users cannot delete them or their accounts (OQ-04, RA-15), so what is said stays stored. Usernames can be discovered by any registered user (OQ-09).

**Mitigation.**
- *Collect only what is needed:* an account holds a username and a password hash and nothing else, with no email address, real name, or phone number. This is a design decision to be recorded in `docs/design.md`.
- *Protect what is stored:* passwords are never kept in plaintext (FR-05) and are hashed with a salted, deliberately slow algorithm (NFR-05); no response contains a password hash, another user's data beyond a username, or a conversation the requester is not in (NFR-06); only participants and members can read a conversation (FR-20, FR-28).
- *Be honest about the limits:* a notice on the registration page says that messages are stored without end-to-end encryption and can be read by the operator (FR-32, verified by AC-27). Users who know this can decide what to write.
- *Bound the exposure:* the system runs only as a local demonstration (RA-14). Before any public deployment, transport encryption would be required, which is put to the client as OQ-03. A retention period and a way to delete data are put to the client as OQ-04.
- *Operational policy:* the operator does not read users' messages, and removes a user's data on request, outside the application. The README (SOW D9) states this policy.

**What it should influence.** A requirement (FR-32, AC-27), a constraint (the demonstration-only assumption RA-14), a design decision (minimal data, the named hash algorithm), and an operational policy.

### 2.3 Account takeover and impersonation

**Who could be affected.** Users whose passwords are guessed or stolen, and their contacts, who may be deceived by messages that appear to come from someone they trust.

**Potential harm.** An attacker reads a victim's history and sends messages in their name. The victim cannot regain the account because there is no password recovery (RA-15, OQ-07). A session left open on a shared computer stays valid because sessions never expire (OQ-06), and nothing slows repeated password guesses.

**Mitigation.**
- *Already in the requirements:* passwords must be 8–128 characters (FR-03); they are stored as slow, salted hashes so that stolen data is expensive to crack (FR-05, NFR-05); a failed login gives the same message whether or not the username exists (FR-07, RA-11); logging out ends the session (FR-08).
- *Design decision:* session identifiers should be random and unguessable. This goes in `docs/design.md`.
- *Considered and not adopted:* locking an account after repeated failed logins. A lockout can itself be used to keep a victim out of their own account, so it needs the client's view of the trade-off. Session expiry and password recovery are also left to the client (OQ-06, OQ-07).

**What it should influence.** Existing requirements (FR-03, FR-05, FR-07, FR-08, NFR-05), a design decision (session identifiers), and documented limitations.

### 2.4 Changes made to the requirements

This analysis led to two additions in `docs/requirements.md`:
- **FR-32**, a registration notice about unencrypted storage and operator access, verified by AC-27.
- **NFR-12**, a message rate limit, verified by AC-26.

Both are small enough for the schedule, and the SOW now lists them as in scope (SOW §3.1). The operator policies and the not-an-emergency-service notice go in the README (SOW D9). Block, report, and moderation were considered and deliberately left out, for the reasons in Section 2.1.

---

## AI-Assisted Work

I used Claude, an AI assistant, throughout the Messenger assignment sequence. I used it to help draft and revise the Statement of Work, the requirements, the PlantUML source and analysis for IA-04, and this reflection, and to review each document against the assignment instructions and against the other documents.

One output that needed meaningful review: the assistant's first draft of the Statement of Work said end-to-end encryption was out of scope and cited "Constraint C5" as support. When I checked the draft against its own tables, C5 turned out to be about the course timeline and developer effort, so the reference did not support the claim. The same draft also numbered the constraints and dependencies I added out of order (C3, C7, C8, C4). I asked why the identifiers were out of order, removed the false reference, and renumbered the lists so they run in order.

A second case: its first requirements draft contained eighteen non-functional requirements with load targets of 20 users and 500-message histories. I cut that to ten requirements with figures I can actually test (10 users, 100 accounts, 100-message histories).

To verify and decide, I compared each document with the assignment's required content and grading criteria, and traced identifiers between the Statement of Work, requirements, acceptance criteria, and analysis. I corrected or rejected anything I could not explain or defend. For this reflection, every statement about how Messenger behaves is tied to a requirement identifier so it can be checked against `docs/requirements.md`. I treated the assistant's mitigation suggestions as proposals and decided which to adopt by weighing them against the project's scope and schedule, which is why block, report, and moderation are not built.
