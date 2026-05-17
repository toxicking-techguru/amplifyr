# AMPLIFYR — Supplementary Logic Note
### Addendum to System Specification v1.0
**Read alongside: AMPLIFYR_System_Specification.md**

---

## Important Clarification: Brand and Branch Are the Same Entity

Before reading anything else in this document, one foundational correction to the main specification must be understood. The main specification treated Brand and Branch as two separate entities — the Brand being the paying client company, and the Branch being the operational unit that manages Creators and Distributors. This separation does not reflect how the system actually works.

In AMPLIFYR, the Brand is the Branch. They are the same entity. A Brand is a company or organisation that joins the platform, answers questions about itself, funds campaigns through escrow, maintains a roster of Creators, connects its own social media accounts for direct posting, and decides whether to use Distributors for platforms it does not have a presence on. All of that — the paying, the managing, the posting, the hiring — belongs to the Brand alone. There is no separate operational layer sitting between the Brand and its Creators.

The hierarchy this creates is clean and direct: the Admin oversees the entire platform, the Manager oversees a portfolio of Brands, and each Brand operates its own content production and distribution pipeline with Creators and Distributors working under it. Wherever the main specification used the word Branch in an operational sense, that word should be read as Brand. Wherever it described something a Branch does — rostering Creators, linking social accounts, assigning Distributors, reviewing content, holding a wallet — all of that belongs to the Brand.

Every section in this note is written with this unified understanding in place.

---

## Purpose

This note exists to close the remaining gaps between the AMPLIFYR System Specification and a fully operational platform. It draws from a technical review of the existing codebase and corrects two specific rules in the main specification that carry real operational risk if left unchanged. Beyond those corrections, it adds six blocks of enforcement logic that the platform needs before any of its core financial and trust features can be considered airtight. Every section here either overrides, tightens, or extends a rule in the main specification. Where this note conflicts with version 1.0 of that document, this note takes precedence.

---

## Part A: Two Rules That Must Be Changed

### A.1 How Distributor Accounts Are Verified

The main specification suggested that a Distributor could be asked to make a test post — publishing a piece of content and then immediately deleting it — as a way to confirm they have genuine posting access on a given platform. This rule is withdrawn.

The reason is straightforward. Social media platforms including TikTok, Instagram, YouTube, and X are designed to detect and penalise accounts that behave in patterns consistent with bots or spam. Posting content and deleting it immediately, especially repeatedly across a verification process, is exactly the kind of behaviour those detection systems are built to flag. A Distributor whose account is shadow-banned or downranked as a result of a verification test is not just useless to the platform — they are actively harmful to any campaign they are assigned to, because their posts will receive suppressed reach from the moment they go live. The test post idea solves a small problem and creates a much larger one.

The correct approach is to verify posting access without making any post at all. When a Distributor connects a social media account to AMPLIFYR, they do so through the standard authorisation flow that every major platform provides. During that flow, the platform requests a specific level of permission — in this case, the permission to create posts, not just read account information. Once the Distributor completes the authorisation, AMPLIFYR checks which permissions were actually granted. If posting permission was granted, the account is confirmed as verified. If the Distributor only granted read access, or if they declined any permission during the flow, the account is marked as unverifiable and the Distributor cannot be assigned to campaigns requiring that platform until they reconnect with the correct permissions.

This verification check repeats every sixty days automatically. If a Distributor's access has expired or their permissions have changed since the last check, they are notified and their account is suspended from active assignments until they re-authorise.

### A.2 How a Distributor's Post Is Confirmed

The main specification described a proof-of-post process in which the Distributor submits both a live URL to the published post and a timestamp screenshot as evidence. The screenshot requirement is removed entirely.

Screenshots are not a reliable form of evidence. The tools available to edit, composite, and fabricate screenshots are widely accessible and require no specialist skill. A screenshot of a post with the correct date, the correct caption, and the correct hashtags can be produced in minutes by anyone with basic editing software. Accepting screenshots as part of a financial verification process introduces a vulnerability that grows more serious as the platform scales, because the incentive to fake a post increases with the size of the Distributor fee.

The correct approach relies entirely on the live URL. When a Distributor submits a post URL, the platform performs a series of automated checks on its own systems — not based on anything the Distributor can influence from their end.

The first check confirms that the URL actually resolves to a live, publicly accessible page. If the URL leads to a deleted post, a private account, an error page, or a login screen, the proof-of-post fails immediately.

The second check reads the content of the post at that URL — the caption text, the hashtags, any account mentions — and compares them against the mandatory inclusions defined in the Campaign Brief for that job. If any required hashtag, brand mention, or keyword is missing, the proof-of-post is flagged as incomplete. The Distributor is notified with a precise list of what is absent, and payment remains blocked until a corrected post URL is submitted.

The third check compares the media file attached to the published post against the original file that was delivered to the Distributor from the Campaign Workspace. Every file has a unique fingerprint based on its content. If the fingerprint of the posted media does not match the fingerprint of the file originally delivered, the system knows the content was modified before posting. This is treated as a serious breach. The Distributor receives an automatic reliability strike, the discrepancy is escalated to the Brand immediately, and the case enters the Dispute Centre.

When all three checks pass, the post is confirmed and the 48-hour payment hold begins, exactly as the main specification defined.

---

## Part B: Six Logic Blocks the System Still Needs

### B.1 The Brand Is the Operational Unit — No Separate Branch Record Needed

The existing codebase handles certain operational functions as part of the Manager record, and the main specification compounded this by splitting them across both a Brand record and a separate Branch record. Both approaches are incorrect. There should be one record for the Brand, and that single record carries everything needed to run the Brand's operations on the platform.

A Brand record must hold all of the following: which Manager oversees it, the full roster of Creators the Brand has hired or onboarded along with the terms of each relationship, the social media accounts the Brand has connected for direct posting, the list of Distributors the Brand works with, the current balance sitting in the Brand's wallet, the Brand's commission arrangement with the Manager, and the contact details of whoever within the Brand's organisation is managing day-to-day operations on the platform. A Brand may designate an internal administrator — someone from their own team who handles the platform on their behalf — without that person needing Manager-level authority over the wider platform.

The reason this consolidation matters so much is that every piece of enforcement logic in this system depends on querying one authoritative record for a Brand and getting complete, reliable answers. Exclusivity checks query who a Creator is currently rostered with. Payment waterfalls query who is owed what from a campaign. Distribution decisions query which platforms the Brand can post to directly. If this information is spread across two different record types — or partially stored in the Manager record — none of these checks can be trusted to be consistent. One unified Brand record eliminates that risk entirely.

### B.2 How the Escrow Waterfall Must Work

The existing codebase handles payouts as one-time events. The platform's vision, as defined in the main specification, is a milestone-driven waterfall where funds are released automatically in tranches as verified performance thresholds are crossed. Bridging these two realities requires a specific engine built with care, because it handles real money moving between multiple parties simultaneously.

The engine runs a check on every active campaign once every 24 hours. During that check, it contacts the social media platform where the content was posted and retrieves the current verified view count for each post linked to that campaign. The view count is not entered by the Creator, is not reported by the Distributor, and cannot be influenced by any user on the platform. It comes directly from the social platform's own data, through a connection that no user has access to.

When the engine detects that the cumulative view count has crossed the next milestone threshold for a campaign, it does not wait for a human to confirm or approve anything. It executes the release immediately.

The release follows a strict sequence and every step must complete successfully before the next one begins. If anything fails partway through, the entire release is rolled back and the system tries again one hour later. This all-or-nothing approach ensures that the financial record always reflects a complete, consistent transaction.

The first deduction from the released amount is the platform commission. This percentage is set by the Admin and applies to every release without exception.

After the platform commission is taken, if the Distributor has a confirmed post for this campaign and their 48-hour payment hold has cleared, and this is the first milestone event since that confirmation, the Distributor's agreed flat fee is taken from the remaining amount and transferred to their wallet. On all subsequent milestone releases for the same campaign, the Distributor has already been paid and this step is skipped.

After the Distributor is settled, the Creator receives their share. For campaigns using the hybrid payment model, the flat base fee is paid in full on the very first milestone release if it was not already paid at the point of content approval. Performance tranches are credited at each subsequent milestone. For pure performance campaigns, the Creator's share at each release is the percentage of the performance budget pool that corresponds to the milestone crossed.

The Manager's commission is not taken from individual milestone releases. It is calculated as a percentage of the Brand's total campaign spend over a given period and swept on a monthly basis from the platform's ledger. This keeps the milestone release transaction clean — only three parties are touched at each release: the platform, the Distributor (on first release only), and the Creator.

Finally, a permanent ledger record is written for every release: the campaign, the milestone crossed, the view count at the moment of crossing, the exact amount each party received, and the timestamp. The Brand, the Creator, and where applicable the Distributor all receive a notification the moment a milestone is hit and funds move.

### B.3 View Counts Cannot Be Entered Manually

In the current version of the platform, there is a mechanism that allows Creators to manually input or update view counts on their submissions. This must be removed completely before the escrow waterfall described in B.2 is built, because the two cannot safely coexist.

A Creator who can self-report views can trigger escrow releases. The financial consequences of this are serious. It is not enough to add a warning or require a manager to confirm manually entered figures. The ability to enter view counts manually must not exist anywhere on the platform for any user of any role.

View counts flow in one direction only: from the social media platform's own systems, through the platform's server-side sync engine, into the campaign record. No user action of any kind can initiate or alter this data. The sync engine is the only part of the system authorised to write view counts, and it runs on a schedule controlled by AMPLIFYR itself, not triggered by any user request.

### B.4 The Two-Round Revision Limit Must Be Technically Enforced

The main specification defined a two-round revision limit per content submission. Once a Brand has requested two rounds of revisions on a given piece of content, the option to request a third revision does not exist — the Brand must either approve the content or open a formal dispute. The current codebase states this as a policy but does not technically enforce it.

Making this rule real requires two layers working together.

The first layer is the interface the Brand sees. Once a submission has received two revision requests, the button that allows the Brand to request further revisions must not appear on the screen at all. The Brand sees two options and two options only at this point: Approve, or Open Dispute. A short, plain notice should appear explaining that the maximum revision rounds for this submission have been reached and that these are the only paths forward.

The second layer is the server. Independently of what the interface shows, the server must check the revision count before processing any action that would register a new revision request. If the count is already at two, the server refuses the action outright, regardless of how the request was sent. This matters because a determined person can send requests directly to the server without using the interface at all. Both layers must be in place simultaneously, because either one alone is insufficient.

### B.5 Exclusivity Conflicts Must Be Checked Automatically

The main specification defined a rule that a Rostered Creator cannot take a new campaign in the same brand category as an existing exclusive campaign they are currently working on. This check needs to happen automatically every time a Creator applies to a job or a Brand attempts to directly assign a Creator to one — because a rule that depends on a person remembering to check it will eventually be missed.

When any assignment event is triggered, the system looks at every active campaign the Creator is currently working on that carries an exclusivity clause through their rostered Brand relationships. For each of those active campaigns, it reads the brand category. It then compares that list of categories against the category of the new campaign being applied for or assigned.

If there is a match, the assignment does not go through. The message shown to whoever initiated the action — whether the Creator themselves or the Brand attempting to assign them — is specific: it names which Brand's exclusivity clause is being violated, names the conflicting category, and gives an honest estimate of when the conflict will clear based on the expected close date of the active exclusive campaign.

If the Creator's relationship with a given Brand does not include any exclusivity clause — because exclusivity is optional and must be agreed to at the time of rostering — then no check is performed in relation to that Brand and the assignment proceeds freely.

This check happens on the platform's own systems and cannot be bypassed through the interface. It is not an advisory warning that a user can dismiss. It is a hard stop.

### B.6 Manager Impersonation Must Create an Unalterable Record

The current codebase allows a Manager to switch their session and act as another user account with a simple toggle. The main specification described detailed audit requirements for this capability. What the existing platform does not yet implement is the immutable record that makes those requirements real rather than theoretical.

The rule is this: a Manager cannot begin an impersonation session without first providing a written reason. The reason must be substantive — a few words of genuine explanation for why the Manager is acting on behalf of another account. A blank field, a single word, or a placeholder phrase is rejected and the session does not start. The Manager must state a real reason before they are granted access.

The moment a valid reason is provided and the session begins, a record is created in a dedicated section of the platform's logs. This record captures who initiated the session and their role, whose account they are accessing and that account's role, the reason provided, and the exact time the session began. As the session continues, every action the Manager takes while operating as the other account is added to this record: every form submitted, every approval given, every message sent, every file downloaded, all with timestamps.

When the session ends — whether the Manager closes it manually, the automatic four-hour timeout fires, or the session is terminated by the system — the end time is recorded and the log entry is sealed.

The defining property of this log is that it cannot be altered once written. No user on the platform, including the Admin, can go back and edit or remove an impersonation record. A Manager cannot clean up sessions they wish had not been logged. The Admin cannot remove records as a favour. The log only grows — it is never trimmed. This is what gives the audit trail genuine legal weight. A record that those it is meant to hold accountable can modify is not a record in any meaningful sense.

---

## Part C: The Order in Which to Build These Things

The improvements in this document are not independent of each other. Some must be in place before others can work correctly. The order below reflects those dependencies.

The first priority is consolidating the Brand record into a single unified entity, as described in B.1. This is the prerequisite for everything else. Every other enforcement rule in this document queries Brand-level data. Until that data lives in one authoritative place, none of the checks can be built with confidence.

The second priority is removing the ability to enter view counts manually, as described in B.3. This is not a new feature — it is closing an existing financial vulnerability. It should be treated as a patch and addressed before any work begins on the escrow engine.

The third priority is enforcing the two-round revision limit, as described in B.4. This is a contained change and can be completed relatively quickly. Having it in place is necessary before the Dispute Centre can handle its escalation role correctly.

The fourth priority is putting the audit log for impersonation in place, as described in B.6. This should exist before the platform handles any real user data, and certainly before it is reviewed by investors or any legal team. Its absence is the kind of gap that raises serious questions during due diligence.

The fifth priority is the exclusivity conflict check, as described in B.5. This depends on the Brand entity being correctly consolidated first, because the check queries a Brand's roster directly. Once B.1 is done, B.5 can be built against it cleanly.

The sixth and final priority is the automated escrow waterfall engine, as described in B.2. This is the largest piece of work on the list and it must not be started until items one, two, and three are fully in place. The milestone logic, the daily sync, the release transaction, and the ledger write all need to be built and tested together as one system before any real campaign funds flow through them.

The changes to Distributor verification — replacing test posts with permission checking and replacing screenshots with URL confirmation — should be embedded in the design requirements for all Distributor-facing screens before any of those screens are built, so the correct approach is built once rather than corrected later.

---

## Part D: What Each Record in the System Needs to Hold

For the rules in this document and the main specification to be enforceable, the platform's underlying records need to carry certain information that the current data models do not yet include. The following describes what each record type needs, in plain terms.

A Campaign record needs to know which milestone tier the Brand selected when the campaign was created, which payment model is in use, how much of the budget is allocated as a flat base fee versus a performance pool, which milestones have already triggered a release, how the Brand has decided to handle distribution on each target platform, when the campaign window is scheduled to close, and how many revision requests have been made on each content submission.

A Creator record needs to carry their current reputation score, the details of every social media account they have verified on the platform along with that account's performance metrics, and a record of every Brand roster they currently belong to including the exclusivity terms that apply to each rostered relationship.

A Brand record — now the single unified entity that replaces both the old Brand and Branch records — needs to carry the Manager it belongs to, the commission arrangement with that Manager, every Creator on its roster and the terms of each roster relationship, every social media account it has connected for direct posting, every Distributor it works with, the current balance in its wallet, and the identity of any internal team member designated to manage the Brand's day-to-day operations on the platform.

A Distributor record needs to carry their reliability score, a history of any reliability strikes they have received including when each occurred and on which campaign, the details of every social media platform account they have verified through the platform along with exactly which permissions were granted and when the verification was last confirmed, and the current balance in their wallet.

The impersonation audit log is a record type that does not yet exist in the current codebase and needs to be created. Each entry in this log needs to hold a unique session identifier, the identity and role of the person who initiated the session, the identity and role of the account they accessed, the reason they provided before the session began, the time the session started, the time the session ended, and a complete, timestamped list of every action taken during the session.

---

*End of AMPLIFYR Supplementary Logic Note*
*This document supersedes conflicting rules in AMPLIFYR_System_Specification v1.0 where explicitly stated.*
