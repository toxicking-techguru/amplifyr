# AMPLIFYR — Full System Specification
### Continuation Logic, Role Constraints & Operational Rules
**Version 1.0 | Internal Working Document**

---

## Preface

This document is the authoritative continuation of the AMPLIFYR platform summary. It defines every role, every workflow state, every payment trigger, and every system constraint required to make the platform fully functional and logically airtight. Where the original summary left gaps — creator onboarding, distributor accountability, multi-branch membership, revision limits, impersonation audit trails, and milestone standardisation — this document closes them with precise rules that the development team can implement directly.

The system must handle two concurrent realities: a creator may be rostered exclusively to one branch, or they may operate as a free-agent applying to jobs across branches. Likewise, a brand may work with multiple branches simultaneously, and a manager may oversee several branches with varying levels of daily involvement. All of these cases are supported, and the rules governing each are defined in the sections below.

---

## Part 1: Platform Structure & Role Hierarchy

### 1.1 The Role Stack

AMPLIFYR operates on a strict six-role hierarchy. Every user account is assigned exactly one primary role at registration. A user cannot hold two primary roles simultaneously, but certain roles carry delegated permissions that allow them to act on behalf of other accounts under controlled, audited conditions.

The role stack from highest authority to lowest is:

**Admin (Executive)** → **Manager** → **Branch** → **Brand / Creator / Distributor**

Brand, Creator, and Distributor sit at the same tier but have entirely separate functional surfaces. They do not have authority over each other.

---

### 1.2 Admin (Executive)

The Admin is the single highest-authority account on the platform. There is exactly one Admin account per AMPLIFYR installation, though the Admin may create sub-admin seats with restricted scopes (e.g., a Finance Sub-Admin who can only see escrow and payout ledgers, or a Support Sub-Admin who can only access the Dispute Centre).

**Core permissions:**
- Create, suspend, or permanently delete Manager accounts.
- Set the platform-wide commission rate (the percentage deducted from every escrow release before any other party is paid). This rate is locked and cannot be overridden by any Manager or Branch.
- View platform-wide analytics: total campaigns, total escrow value locked, total payouts made, platform revenue, and dispute resolution rates.
- Act as the final escalation point for any dispute that a Manager has failed to resolve within the defined window (72 hours).
- Configure the standard milestone tier table (see Part 4) and determine which milestones are available on which subscription plan.
- Enable or disable the marketplace feature globally, or restrict it to specific branches.

**Constraints on Admin:**
- The Admin cannot directly impersonate a Creator or Brand without logging a reason. Every Admin impersonation session is recorded in a permanent, tamper-proof audit log.
- The Admin cannot manually release escrow funds outside of the normal verification pipeline without triggering a two-factor confirmation and a reason entry that is stored in the ledger.

---

### 1.3 Manager

A Manager is an operational leader who is granted authority over one or more Branches by the Admin. A Manager may be an employee of the AMPLIFYR business, a licensed partner, or a franchisee — the system does not differentiate, but the contracts outside the platform must establish which.

**Core permissions:**
- Create Branch accounts and configure them. A Manager may create as many Branch accounts as their subscription tier allows.
- Onboard Brands and Creators at the platform level and assign them to a specific Branch, or make them available across multiple Branches (see Multi-Branch rules in Part 2).
- Impersonate any Brand or Creator account within their Branches for operational management — for example, completing a brand context questionnaire on a Brand's behalf, or reviewing a Creator's submitted content. All impersonation sessions are audited (see Part 1.6).
- Set the Branch commission rate: the percentage of campaign value that each Branch retains after Creator and Distributor payments. This rate is set per Branch and must fall within the bounds permitted by the Admin.
- Resolve disputes escalated from the Branch level before they reach Admin.
- View Manager-level analytics: aggregate performance across all their Branches, top-performing creators, campaign completion rates, and revenue per branch.

**Constraints on Manager:**
- A Manager cannot lower the platform commission rate. They work with the net amount after the platform fee has been deducted.
- A Manager cannot access the accounts or data of Branches they do not own or have been explicitly granted access to by the Admin.
- A Manager earns a passive override commission on all revenue generated by their Branches. This is calculated as a percentage of Branch earnings (not of total campaign value), set at the Admin level, and paid monthly via a sweep. Managers do not directly touch escrow funds.
- A Manager cannot force-release escrow funds. Only the automated milestone verification system or the Admin can release escrow.

---

### 1.4 Branch

A Branch is an operational unit. Think of it as a content agency within the AMPLIFYR ecosystem. Each Branch is owned by exactly one Manager but may have a designated Branch Administrator — a human operator who manages the Branch's day-to-day without having Manager-level platform authority.

**Core permissions:**
- Maintain a roster of Creators. A Branch can invite Creators to join its roster directly, or accept applications from Creators who discover the Branch through the marketplace.
- Accept or reject Brand assignments from the Manager, or receive Brand sign-ups directly if the Branch has a public-facing onboarding link.
- Run campaigns: assign Creators from the roster to specific jobs, review content submissions, and approve or dispute deliverables.
- Maintain a set of linked social media accounts. These accounts are used for direct posting, bypassing the need for a Distributor on platforms where the Branch has a presence.
- Hire or contract Distributors. A Branch may have an internal pool of Distributors (employed or contracted) or may use the platform's independent Distributor marketplace.
- Set per-platform posting rates for Distributors, within bounds defined by the Manager.
- View Branch-level analytics: all active campaigns, Creator performance within the Branch, distributor delivery rates, and revenue.

**Constraints on Branch:**
- A Branch cannot modify its own commission rate. This is set by the Manager.
- A Branch cannot directly send funds to a Creator or Distributor. All payments flow through the escrow system. The Branch's management cut is the remainder of the campaign budget after platform fee, Creator payment, and Distributor payment have been deducted.
- A Branch cannot accept Creators or Brands that the Manager has flagged or suspended.
- If a Branch has no linked social media account for a specific platform required by a campaign, it must assign a Distributor for that platform before the campaign can move to the publishing stage. The system will block campaign progression if no posting method is defined for any required platform.

---

### 1.5 Brand

A Brand is a paying client — a company or individual who wants content created and distributed. A Brand exists within one or more Branches (see Part 2 for multi-branch rules), and its campaigns are managed within those Branches.

**Core permissions:**
- Submit a Company Context Questionnaire, which the AI uses to generate campaign briefs.
- Create campaigns, define budgets, and deposit funds into escrow.
- Choose a payment model per campaign: flat fee, performance-based, or hybrid.
- Review and approve or request revisions on submitted content.
- Communicate with Creators through the in-platform chat on active jobs.
- See real-time analytics on live campaigns: verified views, engagement rate, estimated reach, and cost-per-result.
- Raise a dispute through the Dispute Centre if content does not meet the brief.
- Withdraw from a campaign before a Creator has been assigned without penalty. After assignment, withdrawal triggers the cancellation policy (see Part 5).

**Constraints on Brand:**
- A Brand cannot contact a Creator outside of an active or pending job. Direct outreach to Creators is not permitted. This protects Creators from being recruited off-platform.
- A Brand cannot release or withhold escrow funds manually. Funds are released by the automated milestone system or by a confirmed approval action. A Brand can confirm delivery, which triggers a release, but cannot block a release if milestones have been independently verified.
- A Brand requesting more than two rounds of revisions on the same content submission must engage the Dispute Centre. The system will not show a "Request Revision" button after the second round.
- A Brand may belong to multiple Branches but may not run the same campaign across multiple Branches simultaneously. Each campaign is tied to exactly one Branch.

---

### 1.6 Creator

A Creator is the primary content producer on the platform. A Creator can operate in two modes, and the mode is set per Branch relationship, not globally on the account.

**Rostered Mode:** The Creator has been invited to or accepted by a specific Branch's roster. In Rostered Mode, the Creator is preferentially offered jobs from that Branch. The Branch may set exclusivity terms — meaning the Creator agrees not to take competing brand categories from other Branches during active campaigns. Exclusivity is a Branch-level setting and must be agreed to by the Creator at onboarding. If exclusivity is set, the system will prevent that Creator from being assigned to conflicting brand categories on other Branches for the campaign duration. Exclusivity does not prevent the Creator from applying to non-competing categories on other Branches.

**Free-Agent Mode:** The Creator applies to jobs posted publicly on the marketplace. There is no ongoing relationship with a Branch. The Creator is evaluated per application on portfolio quality, verified social statistics, and reputation score.

A Creator's account may simultaneously be Rostered with one Branch and Free-Agent on the general marketplace, as long as exclusivity terms on the Rostered Branch are not violated by the free-agent applications.

**Onboarding a Creator:**

Step 1 — Registration: Creator signs up with an email, selects the Creator role, and completes a profile: name, bio, content specialisations (niches: Tech, Beauty, Gaming, Lifestyle, Finance, Food, etc.), and primary platform (TikTok, Instagram, YouTube, LinkedIn, X, etc.).

Step 2 — Social Account Verification: Creator links at least one social media account. The platform performs an API sync to verify: account ownership, follower count, average views per post (last 90 days), engagement rate, and account age. Unverified accounts cannot be used to apply for jobs. The verification sync runs every 30 days automatically. A Creator who loses a previously verified account (e.g., account banned or deleted) is flagged and cannot accept new jobs until a replacement account is verified.

Step 3 — Portfolio Submission: Creator submits a minimum of three past content samples. Samples are reviewed either by the Branch (for Roster applications) or by the platform's content quality system (for marketplace applicants). The quality review checks for: production quality, adherence to basic formatting norms, original content (no plagiarism or reposted content), and brand safety (no extremist, explicit, or prohibited content).

Step 4 — Approval and Reputation Seed: On first approval, the Creator receives a seed reputation score of 50 out of 100. The reputation score increases or decreases based on subsequent performance (see Part 6).

**Core Creator permissions:**
- Browse and apply to marketplace jobs.
- Accept direct assignments from Branches they are rostered with.
- Use the Campaign Workspace to view briefs, download reference materials, communicate with the Brand (through the managed channel), and submit content.
- Track earnings, pending payouts, and verified performance data through the Earnings Dashboard.
- Withdraw earnings to a linked bank account or mobile money account, subject to a minimum withdrawal threshold (set by Admin, e.g., $20 equivalent).

**Constraints on Creator:**
- A Creator cannot communicate directly with a Brand outside of a live or pending job thread.
- A Creator cannot submit content on behalf of another creator or use AI-generated media as the sole deliverable unless the campaign brief explicitly permits AI-assisted content.
- A Creator who misses a submission deadline without an approved extension forfeits the flat fee portion of their payment for that milestone. The performance bonus remains earnable if the content is eventually approved and performs.
- A Creator cannot withdraw from a job after content submission has begun (defined as: at least one draft uploaded to the Campaign Workspace) without losing their flat fee deposit for that milestone round.

---

### 1.7 Distributor

A Distributor is responsible for publishing approved content to target social media platforms. The Distributor role is optional at the Branch level — it is only activated when the Branch does not have a linked social account for a platform required by a campaign.

**Two Distributor types:**

Branch-Employed Distributor: Works exclusively for one Branch. Is set up as a sub-account under the Branch. Is paid a fixed monthly retainer set by the Branch, plus a per-post bonus. Does not appear on the marketplace.

Independent Distributor: Registers on the platform, links their social media posting capabilities (i.e., they demonstrate access to specific platform accounts — business manager access, page admin rights, etc.), and appears in the platform's Distributor Directory, searchable by Branch. Is paid per post, per platform, with rates set at the Branch level from within the platform's defined rate bands.

**Onboarding an Independent Distributor:**

Step 1 — Registration: Distributor signs up, selects the Distributor role, and completes a profile listing which social media platforms they can post to, and the type of account access they have (personal account management, business page management, influencer network management, etc.).

Step 2 — Platform Access Verification: For each claimed platform, the Distributor must connect the relevant account via OAuth or provide read-access credentials. The platform verifies that the Distributor genuinely has posting rights on the accounts listed. This is re-verified every 60 days.

Step 3 — Test Post (Optional but Recommended): The platform may require a test post — a non-branded piece of content posted and then immediately deleted — to confirm that the Distributor's access is functional and timely. Branches can filter the Distributor Directory to show only those who have completed a test post.

Step 4 — Rate Agreement: Independent Distributors do not set their own rates. Rates are set by the Branch at the time of campaign assignment. The Distributor must accept the offered rate to be assigned.

**Core Distributor permissions:**
- View incoming posting assignments through their Distributor Dashboard.
- Download approved content files (video, image, caption, hashtags, links) from the Campaign Workspace.
- Submit proof-of-post: a live URL to the published post, plus a timestamp screenshot. The platform verifies the URL is live and matches the content sent.
- Track earnings and submit withdrawal requests.

**Constraints on Distributor:**
- A Distributor has a 24-hour window from content delivery to complete posting and submit proof-of-post. If no proof is submitted within 24 hours, the system sends an automated reminder. If no proof within 36 hours, the Branch is notified and the assignment may be reassigned to another Distributor.
- A Distributor who fails to post without a valid reason (accepted by the Branch within the 36-hour window) receives a reliability strike. Three strikes within 90 days results in account suspension and removal from the Distributor Directory.
- A Distributor cannot modify content. They post exactly what is delivered from the Campaign Workspace. If they identify a technical issue (e.g., file format incompatible with the platform), they must raise a flag through the system within 4 hours of receiving the content. Modifying content without authorisation is grounds for immediate account termination.
- Payment to the Distributor is released 48 hours after proof-of-post is accepted, to allow time for the Brand or Branch to raise a content modification dispute.

---

### 1.8 Impersonation & Audit Trail

Managers may impersonate Brand and Creator accounts within their Branches. Admins may impersonate any account. The following rules govern all impersonation sessions:

Before an impersonation session begins, the impersonating user must log a reason from a predefined list (e.g., "Onboarding assistance," "Technical support," "Content review delegation," "Billing correction") or provide a free-text reason if none applies.

During the session, every action taken — form submissions, approvals, messages sent, files uploaded or downloaded — is tagged with both the impersonator's ID and the account being impersonated. These tags are visible in the account's activity log.

A Brand must consent to Manager impersonation of their account at onboarding. This consent is toggled in their Account Settings and can be revoked at any time. Revoking consent does not retroactively erase audit logs.

Impersonation sessions automatically expire after four hours of inactivity. The impersonator must re-authenticate and log a new reason to resume.

The audit log for all impersonation sessions is write-once and cannot be edited by any user including the Admin. It can only be accessed, not modified.

---

## Part 2: Multi-Branch & Multi-Role Membership Rules

### 2.1 Can a Creator belong to multiple Branches?

Yes. A Creator may be Rostered with multiple Branches simultaneously, subject to exclusivity terms on each individual Branch. The system enforces the following logic:

When a Creator is assigned to a job on Branch A with a brand category exclusivity clause (e.g., "Sports & Fitness"), the system checks whether any active job on Branch B or any other Branch involves a brand in the same category. If a conflict is found, the Creator cannot be assigned to the new job without first completing or withdrawing from the conflicting job.

If Branch A does not set exclusivity, the Creator may work on the same brand category across multiple Branches simultaneously with no restriction.

A Creator may also be simultaneously Rostered on one Branch and operating as a Free-Agent on the marketplace, as long as the free-agent applications do not violate the rostered Branch's exclusivity terms.

### 2.2 Can a Brand work with multiple Branches simultaneously?

Yes. A Brand account may be associated with multiple Branches. Each campaign, however, is always tied to exactly one Branch. A Brand may run Campaign A through Branch X and Campaign B through Branch Y at the same time. The system does not restrict this.

What the system does prevent: running the same campaign (same brief, same budget, same deliverables) simultaneously across two Branches. If a Brand wants broader reach, they must create separate campaigns with separate budgets.

A Manager may assign a Brand to multiple Branches under their management. A Brand that belongs to two Branches managed by different Managers requires Admin approval, as this creates a cross-manager revenue split question that must be resolved contractually before the second Branch assignment is made.

### 2.3 How active is a Manager day-to-day?

A Manager may be fully active (reviewing content daily, responding to disputes personally) or fully passive (delegating all operations to Branch Administrators they appoint). The system supports both.

The Manager's commission is earned passively — it is calculated and swept monthly regardless of how active the Manager is. However, a Manager who is unresponsive to escalated disputes beyond the 72-hour window forfeits their dispute resolution authority for those cases, and they are escalated directly to Admin. Three forfeited escalations in a 90-day period triggers an Admin review of the Manager account.

A Manager who wishes to be operationally active can be granted "Branch Administrator" access on any of their own Branches, letting them directly manage day-to-day operations without it being logged as an impersonation.

---

## Part 3: The Campaign Lifecycle in Full

### 3.1 Campaign Creation

A campaign begins when a Brand (or a Manager acting on behalf of a Brand) initiates a new campaign from the Brand Dashboard.

**Step 1 — Company Context Questionnaire:**
The Brand fills in or updates their Company Context profile. This includes: company name, industry, target audience (demographics, geographics, interests), brand voice (formal/casual, energetic/calm, luxurious/accessible), competitors they want to differentiate from, content formats they want (short-form video, long-form video, static post, story, carousel), and platforms they want to target.

The AI uses this context to generate a Campaign Brief. The Brief specifies: content format and duration, key messages and call to action, mandatory inclusions (logo, specific hashtags, product features to mention), mandatory exclusions (competitor names, restricted topics), platform-specific adaptations (vertical video for TikTok and Reels, landscape for YouTube), tone and pacing guidance, and three example hook scripts for inspiration.

The Brand reviews the AI-generated brief and may accept it, edit it, or regenerate it with additional context. A Manager may edit the brief on the Brand's behalf (logged as a delegated action if done via impersonation).

**Step 2 — Budget and Payment Model:**
The Brand selects a payment model:

Flat Fee Model: A fixed amount is agreed for the deliverable. The Creator is paid the flat fee on content approval. Performance data is tracked but does not affect the Creator's payment.

Performance Model: The Brand defines a budget pool and selects a milestone tier (see Part 4). The Creator earns tranches of the budget as each milestone is verified. If performance falls short, the unspent budget is returned to the Brand at campaign close minus a completion fee (10% of the unspent amount, to compensate the Creator for effort on underperforming content).

Hybrid Model (recommended): A flat base fee is paid on content approval, covering Creator effort regardless of performance. A performance bonus pool is held in escrow and released in tranches as milestones are hit. This is the default recommended model as it aligns incentives without leaving Creators entirely at the mercy of algorithmic performance.

The Brand deposits the full campaign budget into escrow before the campaign is posted or assigned. No campaign may proceed to the Creator assignment stage without a confirmed escrow deposit. The system will not allow a campaign to be published to the marketplace or assigned to a Creator until the escrow is fully funded.

**Step 3 — Platform Selection and Distribution Decision:**
The Brand selects which social media platforms the content should be published to. For each selected platform, the Branch's system checks whether a linked Branch account exists. If yes, the Branch is marked as "posting directly" for that platform. If no, the Branch must assign a Distributor for that platform before the campaign can proceed past content approval. This decision is locked before Creator assignment begins, so the Creator knows exactly where their content will be published and can format accordingly.

**Step 4 — Creator Assignment:**
Two paths:

Direct Assignment: The Branch selects a Creator from their roster. The system checks eligibility (account in good standing, no conflicting exclusivity violation, verified social account active). The Creator receives an assignment notification and has 24 hours to accept or decline. A declined assignment returns the job to the Branch for reassignment. Two consecutive declines by the same Creator within 30 days on the same Branch are noted in their Branch relationship log.

Marketplace Open Call: The job is published to the marketplace with the brief (summary version — not the full AI brief, which the Creator only sees after assignment), the payment model and rate, the platforms, and the content niche. Creators apply with a cover note and their portfolio link. The Brand or Branch reviews applications and selects a Creator. Unselected applicants receive a rejection notification. The selected Creator receives the full brief and the Campaign Workspace is activated.

### 3.2 The Campaign Workspace

Once a Creator is assigned, their Campaign Workspace is activated. The Workspace contains:

- The full approved Campaign Brief, including all mandatory inclusions and exclusions.
- A deadline calendar showing the submission deadline, review window, revision deadlines (if applicable), and the expected posting date.
- A file submission portal where the Creator uploads their content files. Accepted file types are defined per platform (e.g., .mp4 for video, .jpg/.png for image, .pdf for carousel decks). File size limits are enforced per platform's specifications.
- A messaging channel between the Creator and the Brand (or a Branch representative). Messages are threaded and permanent. Neither party can delete messages. This channel is the official record for any brief clarifications.
- A draft review area where the Creator may optionally share work-in-progress previews with the Branch before formal submission. Draft previews do not count as formal submissions and do not start the revision clock.

### 3.3 Submission, Review, and Revision

**Formal Submission:** The Creator marks a submission as "Final Submission" in the portal. This timestamps the submission and notifies the Brand and Branch. The review window opens: 48 hours for the Brand to review (72 hours on weekends and public holidays configured by the Branch's operating region).

**Review Outcome — Three options:**

Approved: The Brand or Branch clicks Approve. The escrow release pipeline is triggered for the flat fee portion (if applicable). The content moves to the distribution stage.

Revision Requested (Round 1): The Brand specifies what needs to change, referencing the brief. The Creator has 48 hours to resubmit. The revision specification must reference at least one element of the Campaign Brief to be valid — the system will not accept a revision request with no content reason.

Revision Requested (Round 2): Same process. This is the final revision round. After Round 2, the Brand's "Request Revision" button is disabled.

**After Round 2 — Three options only:**

The Brand clicks Approve. Content proceeds.

The Brand clicks Dispute. The Dispute Centre is engaged (see Part 5).

The Brand does not respond within the review window after Round 2. The system auto-approves the content after the window closes and triggers the payment release. This prevents Brands from indefinitely blocking payment through inaction.

---

## Part 4: Milestone Verification & Escrow Release

### 4.1 Standard Milestone Tiers

The Admin configures a platform-wide milestone table. Brands choose from this table when creating a performance or hybrid campaign. Custom milestones are only available on the Enterprise plan.

Standard milestone tiers (illustrative — Admin can adjust the view thresholds):

| Tier | Milestone 1 | Milestone 2 | Milestone 3 | Milestone 4 | Final |
|------|-------------|-------------|-------------|-------------|-------|
| Starter | 1,000 views | 5,000 views | — | — | Campaign close |
| Growth | 5,000 views | 15,000 views | 30,000 views | — | Campaign close |
| Scale | 10,000 views | 30,000 views | 75,000 views | 150,000 views | Campaign close |
| Viral | 50,000 views | 150,000 views | 500,000 views | 1,000,000 views | Campaign close |

Each milestone releases a corresponding percentage of the performance budget. Example for Growth tier: Milestone 1 releases 20%, Milestone 2 releases 35%, Milestone 3 releases 30%, and Campaign Close releases the final 15% (provided minimum Milestone 1 was reached). If Milestone 1 is not reached within the campaign window, the Creator receives a partial payment calculated as: (actual views / Milestone 1 threshold) × the flat base fee floor defined in the Hybrid model. The remainder returns to the Brand minus the 10% completion fee.

### 4.2 Verified Views — What Counts

Not all view counts are equal. The platform defines verified views as views from accounts that meet all of the following criteria, verified through the platform's API sync with the social media platform:

- The view came from a unique account (not a repeat view from the same account within 24 hours).
- The viewing account is not itself flagged as a bot or inauthentic account by the social media platform.
- The viewing duration exceeded 3 seconds for video content (this is the standard threshold used by most platforms for a view to count).
- The view originated from a region the Brand has not excluded in their campaign settings.

The platform syncs view data every 24 hours. Milestone checks run automatically at each sync. When a milestone is crossed, the escrow release is triggered automatically — no human action is required. Both the Brand and the Creator receive a notification when a milestone is hit and when the corresponding payment has been released.

### 4.3 Escrow Release Waterfall

When a milestone is triggered, the released amount flows through the following waterfall in this exact order:

1. Platform commission is deducted first (percentage set by Admin, typically 5–8% of the released amount).
2. Distributor fee is deducted (flat fee per platform per post, already agreed at campaign setup, released on the first milestone event after proof-of-post is confirmed).
3. Creator payment: the remaining amount after platform commission is the Creator's share. For hybrid campaigns, this is further split into the flat base (released on approval) and the performance tranche (released at each milestone).
4. Branch management cut: the Branch's cut is not a separate deduction — it is the margin built into the campaign value above the Creator and Distributor costs. When the Manager and Branch set up a campaign, they price it so that after Creator and Distributor payments and the platform fee, the remainder is the Branch's revenue. This remainder is credited to the Branch's internal wallet at the same time the Creator is paid.
5. Manager override commission: calculated monthly, swept from the Branch wallet automatically.

### 4.4 Campaign Close

A campaign closes under one of four conditions:

Natural Expiry: The campaign window ends (duration set at campaign creation, maximum 180 days). Final milestone check runs. Remaining unspent performance budget is returned to the Brand minus the completion fee on the unspent portion.

Brand Closes: The Brand manually closes the campaign after all deliverables are approved. Remaining performance budget (for ongoing posts still accumulating views) continues to be tracked and released for up to 30 days after close date before the campaign is fully settled.

Admin Close: Admin closes the campaign due to policy violation. Escrow handling follows the dispute resolution outcome.

Creator Abandonment: If a Creator has not submitted any content within 72 hours of the submission deadline without requesting an extension, the campaign is automatically moved to "Creator Default" status. The flat fee is forfeited by the Creator. The Branch may reassign to a new Creator. The escrow remains locked and is applied to the new Creator's payment.

---

## Part 5: Dispute Resolution

### 5.1 Dispute Centre

A dispute may be raised by either the Brand or the Creator after a Round 2 revision request has been rejected, or at any point during the campaign where a party believes the other has materially breached the campaign terms.

When a dispute is raised, the Campaign Workspace enters a frozen state: no new submissions, no new revision requests, and no new messages can be sent (existing messages remain visible). Escrow remains locked.

The dispute is assigned to the Branch Administrator (or Manager, if the Branch has no Administrator). The assignee has 72 hours to review the submitted content, the full message history, the Campaign Brief, and the revision request records, and to issue a resolution decision.

**Resolution outcomes:**

Content Approved with Modification: The dispute resolver determines the content meets the brief with minor issues. Content is approved. Escrow is released. The Creator's reputation score takes no negative hit.

Content Rejected — Revision Required: The dispute resolver determines the content genuinely does not meet the brief in a material way. The Creator is required to revise. A third and final revision round opens. The Brand cannot request further revisions after this round.

Content Rejected — Full Cancellation: The dispute resolver finds the content fundamentally undeliverable or the Creator in default. The Brand's escrow is returned in full minus the platform fee. The Creator's reputation score takes a significant negative impact.

Partial Acceptance: The dispute resolver finds partial merit on both sides. An agreed payment is negotiated (e.g., 60% of the flat fee is released to the Creator; 40% is returned to the Brand). Both parties must agree to the partial acceptance offer within 24 hours. If either refuses, the dispute escalates to the Manager level.

### 5.2 Escalation Path

Branch level (72 hours) → Manager level (additional 72 hours) → Admin level (final decision, 96 hours).

At Admin level, the decision is final and binding on both parties. Funds are released or returned according to the Admin's ruling with no further appeal within the platform (legal remedies remain available outside the platform per the Terms of Service).

### 5.3 Cancellation Policy

Brand cancels before Creator assignment: Full escrow refund minus the platform listing fee (a small fixed fee charged at campaign creation to cover AI brief generation costs).

Brand cancels after Creator is assigned but before any draft is submitted: 50% of the flat fee is paid to the Creator as a kill fee. Remaining budget is returned to the Brand minus the platform fee.

Brand cancels after at least one draft has been submitted: Full flat fee is paid to the Creator. Performance budget is returned to the Brand.

Creator withdraws after assignment but before any draft is submitted: No penalty. Job returns to Branch for reassignment. Creator receives no payment.

Creator withdraws after at least one draft has been submitted: Creator forfeits the flat fee. If the submitted draft is approved by the next Creator to be assigned, the original Creator receives no share.

---

## Part 6: Reputation & Quality Systems

### 6.1 Creator Reputation Score

Every Creator has a reputation score from 0 to 100. The score is publicly visible on their marketplace profile. It is calculated as a weighted average of the following components, recalculated after every completed campaign:

On-Time Delivery Rate (30% weight): Percentage of submissions made before the deadline across the Creator's history. Missed deadlines with an approved extension are counted as on-time.

Revision Request Rate (20% weight): Lower is better. Calculated as: number of revision requests received divided by number of campaigns completed. A Creator whose content consistently requires two rounds of revisions will score lower on this dimension than one whose content is approved on first submission.

Brand Satisfaction Rating (30% weight): After each campaign closes, the Brand rates the Creator on a 1–5 scale across three dimensions: Brief Adherence, Production Quality, and Communication Responsiveness. The average of these ratings feeds into the reputation score.

Verified Performance Index (20% weight): The Creator's historical average of actual views achieved relative to the milestone tier selected by the Brand. A Creator who consistently exceeds milestones scores higher than one who consistently falls short.

Reputation score consequences: A score below 40 results in the Creator being hidden from the marketplace until they complete one additional campaign through a direct Branch assignment (giving them a chance to recover without being rejected on sight). A score below 20 results in account review by Admin.

### 6.2 Distributor Reliability Score

Distributors have a simpler reliability score, visible to Branches when searching the Distributor Directory.

The score is calculated from: on-time proof-of-post rate (did they post within 24 hours?), post accuracy rate (was the content posted exactly as delivered, verified by a Branch spot-check), and reliability strikes (strikes reduce the score sharply and remain on record for 90 days).

---

## Part 7: Analytics

### 7.1 What Each Role Sees

**Brand Analytics Dashboard:** Live view count per post per platform, engagement rate (likes + comments + shares / views), cost per verified view (total spend / total verified views), campaign ROI estimate, and comparison against previous campaigns.

**Creator Earnings Dashboard:** Earnings per campaign, pending payouts, milestone progress on active campaigns, performance history, and a comparison of the Creator's average engagement rate versus the platform average in their niche.

**Branch Analytics Dashboard:** All active campaigns and their statuses, revenue generated this month versus last month, Creator performance ranking within the Branch, Distributor reliability scores, and average time from campaign creation to content approval.

**Manager Analytics Dashboard:** Aggregate of all Branch dashboards, revenue per Branch, Branch efficiency score (campaigns completed on time / campaigns created), dispute rate per Branch, and Manager commission ledger.

**Admin Analytics Dashboard:** Platform-wide metrics, total escrow value locked, total payouts made this month, platform commission earned, dispute resolution rate and average resolution time, Creator churn rate, Brand retention rate, and top-performing Branches and Creators.

---

## Part 8: Platform Constraints Summary

The following constraints are enforced at the system level and cannot be overridden by any user action regardless of role.

No campaign may proceed to Creator assignment without a fully funded escrow. This is a hard gate enforced in the platform's backend, not merely a UI warning.

No payment may leave the escrow system to a Creator or Distributor without a verified trigger (content approval confirmation, milestone verification, or Admin ruling in a dispute). Manual overrides require Admin authentication and are logged permanently.

No Brand may communicate directly with a Creator outside of an active or pending job thread within the platform.

No Distributor may modify content prior to posting. File delivery from the Campaign Workspace to the Distributor is a read-only download. Any modification attempt is detectable through file hash comparison at proof-of-post submission.

All impersonation sessions are logged with immutable audit records.

Revision requests are capped at two per submission milestone. The third disagreement must enter the Dispute Centre.

Creator exclusivity conflicts are enforced automatically. The system will block assignment if the conflict rules are triggered.

A campaign window cannot exceed 180 days. Extensions beyond this require Admin approval and a written reason.

A Creator's verified social account must remain active throughout the campaign. If an account is deactivated or suspended by the social media platform during an active campaign, the Creator has 72 hours to provide an alternative verified account or the campaign enters dispute status.

All financial transactions — deposits, releases, returns, fees, and commissions — are recorded in the platform ledger in real time. The ledger is the authoritative source of truth for all payment disputes.

---

## Appendix: Terminology Reference

| Term | Definition |
|------|-----------|
| Escrow | Funds held in a neutral platform wallet, not accessible by the Brand or Creator until release conditions are met |
| Milestone | A verified performance threshold that triggers a partial escrow release |
| Flat Fee | A fixed payment for a deliverable, independent of post-publication performance |
| Hybrid Model | A payment structure combining a flat base fee (paid on approval) and a performance bonus pool (released at milestones) |
| Proof-of-Post | A live URL and timestamp screenshot submitted by the Distributor to confirm content has been published |
| Verified View | A view meeting all platform quality criteria: unique account, human origin, minimum duration, non-excluded region |
| Reputation Score | A 0–100 score on a Creator's profile reflecting historical delivery quality, client satisfaction, and performance |
| Reliability Strike | A negative record on a Distributor's account for failure to post on time or accurately |
| Campaign Window | The total duration of a campaign from Creator assignment to campaign close, maximum 180 days |
| Rostered Creator | A Creator who has an ongoing relationship with a specific Branch's roster |
| Free-Agent Creator | A Creator who applies to jobs individually through the marketplace without a Branch roster relationship |
| Exclusivity | A Branch-level setting that restricts a Rostered Creator from taking competing brand categories during an active campaign |
| Branch Wallet | An internal platform wallet holding the Branch's accumulated management revenue, from which Manager commissions are swept |
| Audit Log | An immutable record of every impersonation session, manual escrow action, and Admin override |

---

*End of AMPLIFYR System Specification v1.0*
