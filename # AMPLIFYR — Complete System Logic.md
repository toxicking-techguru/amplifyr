# AMPLIFYR — Complete System Logic
### Full Step-by-Step Operational Specification
**Hardened from all prior sessions | All roles | All phases | All constraints**

---

## How to Read This Document

Every step in this document is a system instruction. Each one describes what happens, who initiates it, what the system does in response, and what conditions must be true before the next step can proceed. Steps marked with a constraint note carry a rule that the system must enforce automatically — not as a warning, but as a hard gate that no user action can bypass. This document covers every role: Admin, Manager, Brand, Creator, and Distributor.

---

## Phase 1: Public Entry & Discovery

**Step 1 — User arrives at the landing page.**
The platform presents a high-end public interface with a hero section, a clear value proposition, and navigation to public discovery features. No login is required to view this page. The landing page must load performance data in real time: total campaigns live, total creators verified, and total payouts processed to date. These numbers are pulled from the live database and are never manually entered or fabricated.

**Step 2 — User browses the Discovery Wall.**
Before creating any account, a visitor may browse the Discovery Wall — a curated, publicly visible gallery of completed campaign content. Each entry on the Discovery Wall shows the content piece, the niche it belongs to, the verified view count it achieved, and a anonymised indication of what the creator earned (e.g., "Earned $240 — 12,000 verified views"). The Discovery Wall does not reveal Creator identities, Brand names, or exact financial figures. It exists to build trust and demonstrate real platform activity.

**Step 3 — User examines the Trend Pulse.**
The Trend Pulse page displays real-time market benchmarks drawn from live campaign data on the platform. This includes the current average cost per verified view across all active campaigns, the top-performing niches of the past seven days, the platforms with the highest engagement rates, and the average time from campaign creation to content approval. All figures on the Trend Pulse are calculated automatically from real campaign data. No figure on this page is static or hardcoded.

**Step 4 — User reads creator and brand success stories.**
The landing page surfaces a rotating set of anonymised success stories drawn from completed campaigns. Each story shows the campaign niche, the content format used, the milestone tier selected, the final verified view count, and the total payout made. These stories update automatically as new campaigns close successfully on the platform.

**Step 5 — User clicks the primary call to action.**
The landing page presents a single primary call to action that routes the visitor into the authentication environment. The label on this button communicates access and entry — it is the threshold between the public-facing discovery experience and the secure, role-specific application. Clicking it does not require the user to have decided on a role yet. Role selection happens after the authentication step.

**Step 6 — User is held at the entry gate until authenticated.**
The system does not allow access to any role-specific dashboard, campaign data, Creator profiles, or financial information until the user has completed authentication. A visitor who navigates directly to a protected URL without an active authenticated session is redirected to the authentication screen without exception.

---

## Phase 2: Authentication & Role Selection

**Step 7 — User reaches the authentication screen.**
The authentication screen presents two options: sign in with an existing account, or create a new account. Both paths require a valid email address. The platform supports email-and-password authentication and single-sign-on via Google. No other authentication methods are supported at this stage without explicit Admin configuration.

**Step 8 — New user selects a role.**
During account creation, the user must select one of the following roles before their account is created: Creator, Brand, Manager, or Distributor. The Admin role is not selectable during self-registration. Admin accounts are created exclusively by the platform's founding administrator and cannot be created through the standard sign-up flow. Role selection is permanent and cannot be changed by the user after account creation. If a user needs a different role, they must contact the Admin, who may create a new account for them.

**Step 9 — The system validates the role selection.**
Before creating the account, the system checks that the selected role is one the platform currently accepts for self-registration. If the system is in a restricted mode where, for example, Distributor self-registration is turned off by the Admin, a user selecting that role is shown a message explaining the restriction and is not permitted to proceed.

**Step 10 — The system creates the account and initialises the profile.**
On successful registration, the system creates a user record with the selected role, a unique user identifier, the registration timestamp, and a default account status of Pending Verification. The account is not fully active until the verification step that follows is completed. A verification email is sent to the address provided.

**Step 11 — User verifies their email address.**
The user must click the verification link sent to their email before gaining access to any feature of the platform beyond the authentication screen. An unverified account can log in but sees only a verification prompt. The system does not grant access to dashboards, campaigns, or any operational feature until email verification is confirmed.

**Step 12 — Returning user signs in.**
A returning user enters their credentials on the authentication screen. The system validates the credentials and, on success, reads the user's role from their account record. The session is initialised and the user is routed to the dashboard that corresponds to their role. A returning user does not go through role selection again.

**Step 13 — The system routes the user to their role-specific dashboard.**
After authentication, the routing is absolute. A Creator is sent to the Creator Dashboard. A Brand is sent to the Brand Dashboard. A Manager is sent to the Manager Dashboard. A Distributor is sent to the Distributor Dashboard. An Admin is sent to the Admin Console. There is no shared dashboard. Each dashboard surfaces only the information and actions relevant to that role. No dashboard shows data or controls that belong to another role, except in cases of a properly initiated and audited impersonation session.

**Step 14 — The system checks for onboarding completion.**
On every login, before presenting the full dashboard, the system checks whether the user has completed their role-specific onboarding profile. If required fields are missing — for example, a Creator who has not yet verified a social media account, or a Brand that has not completed its company context profile — the system surfaces a prominent onboarding prompt. The user may dismiss the prompt and access the dashboard, but certain features remain locked until onboarding is complete. A Creator cannot apply to campaigns until at least one social account is verified. A Brand cannot create campaigns until its company context profile has been saved at least once.

---

## Phase 3: Brand Onboarding & Profile Setup

**Step 15 — Brand completes the company context profile.**
The Brand fills in a structured questionnaire that the AI uses to generate campaign briefs. This questionnaire asks for the company name, the industry the company operates in, a description of the product or service being promoted, the target audience in terms of age range, geographic focus, and interest categories, the brand voice the company wants Creators to adopt, the competitors the brand wants to differentiate itself from, and any topics or associations the brand wants to avoid. This profile can be updated at any time, but changes made after a campaign brief has been generated for an active campaign do not retroactively affect that brief.

**Step 16 — Brand saves the context profile.**
On saving, the system stores the company context and marks the Brand's onboarding profile as complete. The system confirms to the Brand that their context has been saved and that the AI will use it as the foundation for all future campaign brief generation. The Brand is now eligible to create campaigns.

**Step 17 — Brand connects a payment method.**
Before a Brand can deposit funds into escrow, they must connect a valid payment method. The platform supports bank transfers, card payments, and mobile money depending on the operating region. The payment method is verified by the platform's payment processor before it is saved to the Brand's account. A Brand with no verified payment method cannot create a campaign or proceed past the campaign budget step.

**Step 18 — Brand is assigned to a Manager.**
Every Brand on the platform is associated with a Manager. This association happens in one of two ways. If the Brand registered through an invitation link provided by a Manager, the association is created automatically at the point of registration. If the Brand registered independently, the Admin assigns them to the appropriate Manager based on their industry, region, or other qualifying criteria. A Brand cannot operate on the platform — cannot create campaigns, cannot access the marketplace — until a Manager association exists. The Brand is notified of their Manager association and shown the Manager's name and contact information within the platform.

**Step 19 — Manager reviews and confirms the Brand.**
When a new Brand is assigned, the Manager receives a notification. The Manager reviews the Brand's company context profile and either confirms the Brand as active or requests additional information before confirming. A Brand that has not been confirmed by their Manager remains in a Pending state and cannot create campaigns. The Manager has 48 hours to confirm or respond. If 48 hours pass without action, the system escalates the pending confirmation to the Admin.

**Step 20 — Brand explores the dashboard.**
On their first confirmed login, the Brand's dashboard shows an overview of their account status, a prompt to create their first campaign, a summary of any active campaigns and their current status, a link to the Discovery Wall filtered to their industry niche, and their earnings and spend summary (empty at this stage).

---

## Phase 4: Campaign Creation

**Step 21 — Brand initiates a new campaign.**
The Brand clicks the option to create a new campaign. The system opens a multi-step campaign creation flow. The Brand cannot skip steps or submit the campaign until all required fields in every step are complete. The system saves progress automatically so the Brand can return to a partially completed campaign without losing their work.

**Step 22 — Brand names the campaign and sets its objective.**
The Brand gives the campaign a name for internal reference and selects the primary campaign objective from a predefined list: brand awareness, product launch, audience growth, engagement, or direct conversion. The objective informs the AI brief generator and also determines which milestone tiers are most appropriate to recommend in a later step.

**Step 23 — Brand activates the AI brief generator.**
The Brand clicks the option to generate a campaign brief using the platform's AI. The system uses the Brand's saved company context profile alongside the campaign name and objective to generate a structured brief. The brief produced by the AI includes a recommended content format and duration, a list of key messages the Creator should communicate, three suggested hook scripts the Creator can use as a starting point, a list of mandatory inclusions such as specific hashtags, product names, or calls to action, a list of mandatory exclusions such as competitor names or sensitive topics, tone and pacing guidance, and platform-specific formatting notes for each social media platform the Brand intends to target. The generation process takes no more than thirty seconds. The result is displayed to the Brand for review before it is saved.

**Step 24 — Brand reviews and edits the AI-generated brief.**
The Brand reads the generated brief and may edit any section. If the Brand is not satisfied with the result, they may click to regenerate with additional context they provide in a free-text field. The brief is not saved or attached to the campaign until the Brand explicitly clicks to confirm it. The confirmed brief becomes the authoritative reference document for the campaign and is shown in full to Creators once they are assigned.

**Step 25 — Brand selects target platforms.**
The Brand selects which social media platforms the content should be published to. The available options include TikTok, Instagram, YouTube, X, LinkedIn, and Facebook. The Brand may select multiple platforms. For each selected platform, the Brand indicates whether they want the content formatted for the short-form vertical format, the long-form horizontal format, or both. This selection determines the technical specifications the Creator must follow when producing the content.

**Step 26 — Brand defines Creator requirements.**
The Brand sets the minimum requirements a Creator must meet to be eligible for this campaign. This includes the minimum follower count on the target platform, the minimum average view count per post over the past 30 days, the niche or content category the Creator must specialise in, and the minimum reputation score the Creator must hold on the platform. These requirements filter the pool of eligible Creators when the campaign is published to the marketplace or when the Brand searches for Creators to assign directly.

**Step 27 — Brand selects a payment model.**
The Brand selects one of three payment models for this campaign. The flat fee model means the Creator is paid a fixed amount on content approval regardless of how the content performs after publication. The performance model means the Creator's entire payment comes from a budget pool released in tranches as verified view milestones are reached. The hybrid model combines a flat base fee paid on approval with a separate performance bonus pool released at milestones. The system recommends the hybrid model as the default and shows the Brand an explanation of how each model affects Creator incentives and Brand financial risk.

**Step 28 — Brand selects a milestone tier (performance and hybrid models only).**
If the Brand has selected the performance or hybrid model, they must choose a milestone tier. The available tiers are Starter, Growth, Scale, and Viral, each corresponding to a set of verified view thresholds at which portions of the performance budget are released. The system displays the specific thresholds for each tier alongside the campaign objective the Brand selected earlier, so the Brand can choose a tier that is realistic given their target audience size and the content format selected.

**Step 29 — Brand sets the campaign budget.**
The Brand enters the total campaign budget. For flat fee campaigns, this is a single amount paid to the Creator on approval. For performance campaigns, this is the total pool distributed across milestones. For hybrid campaigns, the Brand sets both the flat base fee and the performance pool amount separately. The system shows the Brand a breakdown of how the budget will be distributed: the platform commission deducted at each release, the Creator's take, the Distributor fee if applicable, and the Manager's monthly commission as a separate line. The Brand must confirm they understand the fee structure before proceeding.

**Step 30 — Brand defines the campaign window.**
The Brand sets the start date and the end date for the campaign. The campaign window is the total time from Creator assignment to campaign close, during which view counts are tracked and milestone releases can occur. The maximum campaign window is 180 days. The system will not accept a window longer than 180 days. If the Brand needs a longer window, they must contact the Admin with a written reason.

**Step 31 — Brand decides on distribution method per platform.**
For each platform selected in Step 25, the Brand must specify how the approved content will be published. If the Brand has a social media account on that platform connected to the AMPLIFYR platform, they may select direct posting, which means the platform handles publication through the Brand's own connected account. If the Brand does not have a connected account for a given platform, they must select the option to assign a Distributor for that platform. The system identifies any platform for which no posting method has been selected and prevents the campaign from being submitted until every selected platform has a defined distribution method.

**Step 32 — Brand deposits the campaign budget into escrow.**
Before the campaign can be published or assigned to a Creator, the Brand must deposit the full campaign budget into the platform's escrow system. The system charges the Brand's connected payment method for the total campaign budget. The funds are held in a secure escrow wallet controlled by the platform. Neither the Brand nor the Creator can access these funds directly. The system confirms the deposit to the Brand and updates the campaign status from Draft to Funded. An unfunded campaign cannot proceed.

**Step 33 — Brand submits the campaign.**
With all steps complete and the escrow funded, the Brand submits the campaign. The system runs a final validation check: every required field is filled, the budget is in escrow, a distribution method is defined for every selected platform, and the campaign window dates are valid. If any check fails, the system shows the Brand precisely which items need attention. If all checks pass, the campaign is created with a status of Open and becomes available for Creator assignment.

**Step 34 — Brand decides between direct assignment and marketplace open call.**
After submission, the Brand chooses how to find a Creator. In the direct assignment path, the Brand searches the platform's Creator directory filtered by the requirements set in Step 26 and selects a specific Creator to invite. In the marketplace open call path, the campaign is published to the Discovery Hub where eligible Creators can see it and apply. Both paths are available simultaneously — the Brand may invite a specific Creator while also accepting applications from others, and make their final choice from the combined pool.

---

## Phase 5: Creator Onboarding & Verification

**Step 35 — Creator completes their profile.**
After registration and email verification, the Creator fills in their profile. This includes their display name, a short bio describing their content style and specialisations, their primary content niche selected from the platform's standardised niche list, the social media platforms they are most active on, and the content formats they are best at producing. The profile is public-facing and visible to Brands and Managers when they search for Creators.

**Step 36 — Creator links at least one social media account.**
A Creator must verify at least one social media account before they can apply to any campaign. To verify an account, the Creator connects it through the platform's authorisation flow for the relevant platform. On connection, the platform reads the account's follower count, the average view count per post over the past 30 days, the account's engagement rate, and the account creation date. All of these metrics are read directly from the platform's API. The Creator cannot enter or edit these figures. If the API connection fails, the Creator is shown an error and prompted to try again. If the platform's API does not support the data being requested, the verification cannot be completed and the account cannot be listed as verified.

**Step 37 — The system validates the verified account.**
The system checks that the connected account meets the platform's minimum standards for Creator participation: the account must be at least 90 days old, must have at least one piece of original content posted in the past 30 days, and must not be flagged as inauthentic or restricted by the social media platform's own systems. Accounts that fail any of these checks are marked as unqualified and the Creator is shown an explanation of which check failed and why.

**Step 38 — Creator submits a content portfolio.**
The Creator uploads a minimum of three examples of their past content. These can be links to existing posts on their verified social accounts. The portfolio is reviewed by the system's content quality layer, which checks for original content, acceptable production standards, and brand safety. Brand safety checks flag content that contains explicit material, extremist content, or content that would be incompatible with the brands likely to operate in the Creator's selected niche. A Creator whose portfolio contains brand-unsafe content is notified and given the opportunity to replace the flagged samples before their profile is published.

**Step 39 — Creator's reputation score is initialised.**
On first approval, the Creator receives a starting reputation score of 50 out of 100. This score is visible on their marketplace profile and factors into whether they meet the minimum reputation requirement set by Brands on individual campaigns. The score changes over time based on performance (see Phase 14).

**Step 40 — Creator's profile is published to the marketplace.**
Once the social account is verified, the portfolio has passed quality review, and the reputation score has been initialised, the Creator's profile is published and they become searchable in the Creator directory. They are now eligible to apply for campaigns on the marketplace and to receive direct assignment invitations from Brands.

**Step 41 — Creator optionally applies to join a Brand's roster.**
A Creator who wants a long-term relationship with a specific Brand may apply to be rostered. To do this, they navigate to the Brand's public profile on the marketplace and submit a roster application alongside a short note explaining their interest. The Brand reviews the application alongside the Creator's full profile and verified metrics. The Brand may accept the application, which creates a formal rostered relationship, or decline it.

**Step 42 — Brand and Creator agree on roster terms.**
If the Brand accepts the roster application, the system presents both parties with the terms of the rostered relationship: whether exclusivity applies, and if so, which brand categories the Creator agrees to avoid taking from other Brands during active campaigns on this Brand's roster. Both parties must confirm the terms before the rostered relationship is recorded. If either party does not confirm within 48 hours, the roster application expires and the Creator must reapply if still interested.

---

## Phase 6: Creator Job Discovery & Application

**Step 43 — Creator browses the Discovery Hub.**
The Creator's dashboard includes access to the Discovery Hub — the marketplace of open campaigns. The Creator can filter campaigns by niche, by target platform, by payment model, by minimum payout, and by content format required. The system automatically hides campaigns that the Creator is ineligible for based on the requirements set by the Brand in Step 26. A Creator who does not meet a campaign's minimum follower count or minimum reputation score does not see that campaign in their feed unless they explicitly search for it, in which case the campaign appears but the Apply button is replaced with an eligibility notice.

**Step 44 — Creator reads a campaign listing.**
Each campaign listing in the Discovery Hub shows a summary version of the campaign brief — not the full brief, which is only visible after assignment. The summary includes the content niche, the target platforms, the content format required, the payment model, the total payout possible, and the Brand's minimum Creator requirements. The Creator can also see how many other Creators have already applied and whether the Brand has been active on the platform recently.

**Step 45 — System checks Creator eligibility before allowing application.**
When the Creator clicks to apply to a campaign, the system runs an eligibility check before showing the application form. This check verifies that the Creator meets the follower count requirement, meets the reputation score requirement, has a verified account on each of the target platforms, and does not have an active exclusivity conflict with a Brand they are rostered with that covers the applying campaign's brand category. If any check fails, the system shows the Creator which specific requirement they do not meet and why the application cannot proceed. If all checks pass, the application form is presented.

**Step 46 — Creator submits an application.**
The application form asks the Creator to write a short cover note explaining why they are a good fit for this campaign, to select which of their verified accounts they plan to use for content delivery on each required platform, and to confirm they have read the campaign summary and understand the content niche and format requirements. The Creator submits the application. The Brand receives a notification that a new application has arrived.

**Step 47 — Creator receives a direct assignment invitation.**
In the direct assignment path, the Creator receives a notification that a Brand has invited them to work on a specific campaign. The notification includes the campaign name, the platform, the payment model, the payout, and a summary of the brief. The Creator has 24 hours to accept or decline the invitation. If the Creator accepts, the assignment is created. If the Creator declines or does not respond within 24 hours, the invitation expires and the Brand is notified so they can invite someone else.

**Step 48 — Creator opens a pre-assignment chat with the Brand.**
Before an assignment is formally created, both parties may use the platform's built-in messaging system to ask and answer questions about the campaign. The Creator may ask for clarification on brief requirements. The Brand may ask the Creator about their content style or past experience. This conversation is recorded and becomes part of the job record once the assignment is created. The chat cannot be used to negotiate payments — payment terms are set during campaign creation and cannot be changed through the chat.

**Step 49 — Brand selects a Creator from the applicant pool.**
The Brand reviews all applications received for a campaign. Each application shows the Creator's profile, their verified metrics on the relevant platform, their reputation score, their portfolio, and their cover note. The Brand selects one Creator. All other applicants receive a notification that the position has been filled. The selected Creator receives an assignment confirmation.

---

## Phase 7: Manager Workflow

**Step 50 — Manager reviews their Manager Dashboard on login.**
The Manager's dashboard shows a summary of all Brands under their management: how many are active, how many campaigns are currently live, the total escrow value locked across all active campaigns, recent dispute activity, and the Manager's commission earned this month to date. The Manager can drill into any Brand to see its specific campaign list and operational status.

**Step 51 — Manager creates a new Brand account.**
When a new client is brought onto the platform, the Manager creates a Brand account on their behalf. The Manager enters the company name, the primary contact's email address, and the industry. The system creates the Brand account, sends a welcome email to the contact address with login instructions, and automatically associates the new Brand with the Manager who created it. The Manager may then complete the Brand's company context profile on their behalf through an impersonation session, following the full audit logging rules defined in the system constraints.

**Step 52 — Manager initiates an impersonation session.**
To act on behalf of a Brand or Creator account, the Manager navigates to the account in their dashboard and clicks the option to manage on behalf of this account. The system requires the Manager to provide a written reason before the session begins. The reason must be at least a full sentence in length. The system rejects one-word or placeholder entries. Once a valid reason is submitted, the session begins and every action taken is logged under the Manager's identity alongside the account being acted on behalf of.

**Step 53 — Manager ends the impersonation session.**
The Manager explicitly closes the impersonation session when their task is complete. The session also ends automatically if there is no activity for four hours. On session end, the audit log record is sealed. The Manager returns to their own dashboard. A summary notification is sent to the account that was accessed, informing them that a Manager session occurred, what the stated reason was, and when it happened.

**Step 54 — Manager sets the Brand's commission arrangement.**
The Manager defines what the Brand pays in terms of platform usage, which feeds into the Manager's monthly commission calculation. This is configured in the Brand's settings within the Manager's dashboard and is not visible to the Brand. The commission structure agreed between the Manager and the platform Admin governs how much of this flows to the Manager and how much stays with the platform.

**Step 55 — Manager monitors active campaigns across all Brands.**
The Manager can see all active campaigns across every Brand they manage from a single view. They can filter by campaign status — open, active, in review, disputed, closed — and by Brand. When a campaign has been in a particular status for longer than the expected window — for example, a campaign where the review window has passed without a Brand action — the system highlights it for the Manager's attention.

**Step 56 — Manager handles dispute escalations.**
When a dispute at the Brand level has not been resolved within 72 hours, it is escalated to the Manager. The Manager receives a notification and must review the case within a further 72 hours. The Manager can access the full dispute record: the campaign brief, the submitted content, the complete message history between the Brand and the Creator, the revision history, and the reason the dispute was raised. The Manager issues a resolution decision from the options available in the Dispute Centre (see Phase 12).

**Step 57 — Manager views their commission ledger.**
The Manager Dashboard includes a dedicated earnings section showing the commission earned from each Brand, calculated as a percentage of total campaign spend across their managed Brands. This is updated in real time as campaigns close and is swept to the Manager's wallet on the first day of each calendar month. The Manager can request an early withdrawal if their wallet balance exceeds a minimum threshold set by the Admin.

---

## Phase 8: Distributor Workflow

**Step 58 — Distributor completes onboarding.**
After registration, the Distributor completes their profile by listing the social media platforms they can post to and the type of account access they have — for example, whether they manage business pages, personal accounts, or influencer networks. The profile is not active until at least one platform account has been verified.

**Step 59 — Distributor verifies platform access.**
For each social media platform the Distributor claims to post on, they connect the relevant account through the platform's authorisation flow. During this flow, the system requests posting-level permission — not just read access. On completion, the system checks whether posting permission was actually granted. If posting permission is confirmed, the platform is listed as verified on the Distributor's profile. If only read permission was granted, the platform is listed as unverifiable and the Distributor is prompted to reconnect with the correct permissions.

**Step 60 — Distributor appears in the Distributor Directory.**
Once at least one platform account is verified, the Distributor's profile is published to the Distributor Directory, which is searchable by Brands and Managers. The directory shows the Distributor's verified platforms, their reliability score, and the number of campaigns they have successfully completed.

**Step 61 — Distributor receives a posting assignment.**
When a Brand's campaign reaches the distribution stage for a platform on which the Brand has no direct social account, a Distributor must be assigned. The Brand or Manager selects a Distributor from the directory. The Distributor receives an assignment notification that includes the campaign name, the target platform, the content files to be posted, the caption and hashtag requirements from the brief, the posting deadline, and the agreed fee. The Distributor must accept the assignment before the content files are made available for download.

**Step 62 — Distributor downloads content from the Campaign Workspace.**
After accepting the assignment, the Distributor can download the content files from the Campaign Workspace. The files are delivered in a read-only format. The Distributor cannot modify, re-encode, re-compress, or alter the files in any way before posting. If the Distributor identifies a technical issue — for example, a file format that the target platform does not accept — they must flag this through the platform's notification system within four hours of downloading. They must not post a modified version without explicit authorisation from the Brand communicated through the platform's messaging system.

**Step 63 — Distributor posts the content and submits proof.**
Once the content is posted on the target platform, the Distributor copies the live post URL and submits it through the platform's proof-of-post form. The system immediately begins its automated verification process: confirming the URL is live and public, reading the post's caption and hashtags against the brief's mandatory inclusions, and comparing the posted media's fingerprint against the delivered file's fingerprint. The Distributor is notified of the verification result within fifteen minutes. If verification passes, the proof-of-post is confirmed and the 48-hour payment hold begins. If verification fails, the Distributor is shown exactly which check failed and must correct and repost before payment can proceed.

**Step 64 — Distributor's 24-hour posting deadline is enforced.**
From the moment the content files are made available for download, the Distributor has 24 hours to complete the post and submit a valid proof-of-post. At the 20-hour mark, the system sends an automated reminder if no proof has been submitted. At the 24-hour mark, if no proof has been submitted, the Brand is notified and the assignment enters a late status. At the 36-hour mark, if there is still no submission, the Brand may reassign the posting task to another Distributor. A failure to post without an accepted excuse logged in the system within the 36-hour window results in a reliability strike on the Distributor's account.

---

## Phase 9: Content Creation & Submission

**Step 65 — Creator accesses the Campaign Workspace.**
On assignment confirmation, the Creator's Campaign Workspace for this job is activated. The workspace contains the full campaign brief, the posting deadline, the submission portal, the pre-assignment message history, and a draft area where the Creator can optionally share work-in-progress previews with the Brand before formal submission.

**Step 66 — Creator studies the full brief.**
The Creator reads the complete AI-generated brief as confirmed by the Brand. This includes the content format and duration requirements, all key messages, the suggested hook scripts, the mandatory inclusions and exclusions, the tone guidance, and the platform formatting specifications. The Creator must produce content that complies with all mandatory inclusions and does not contain any mandatory exclusions. Non-compliance with these elements is grounds for a revision request or dispute.

**Step 67 — Creator uses the brief as the production foundation.**
The Creator produces their content piece using the brief as the authoritative guide. The hook scripts in the brief are suggestions, not scripts that must be delivered verbatim. The Creator brings their own voice and style to the execution while adhering to the message, format, and compliance requirements. The Creator is not permitted to use AI-generated media as the sole deliverable unless the campaign brief explicitly states that AI-assisted content is acceptable. Human-created content is the standard.

**Step 68 — Creator optionally submits a draft preview.**
Before making a formal submission, the Creator may upload a draft preview to the workspace. A draft preview is not a formal submission — it does not start the review clock and cannot be approved or rejected. It is a voluntary sharing mechanism that allows the Brand to give informal guidance before the Creator finalises the content. Brands are not obligated to respond to draft previews, and no draft preview can be used as evidence in a dispute.

**Step 69 — Creator makes a formal submission.**
When the content is ready, the Creator uploads the final files to the submission portal and clicks the button to mark this as a Final Submission. This action is irreversible. On clicking, the system timestamps the submission, notifies the Brand and the Manager, and opens the review window. The Creator cannot make changes to the submitted files after this point without the Brand opening a revision request.

**Step 70 — System checks submission completeness.**
Before accepting the final submission, the system verifies that at least one file has been uploaded, that the file type matches the expected format for the campaign's target platforms, and that the file size falls within the platform's defined limits per platform. If any check fails, the system shows the Creator which file has an issue and does not proceed with the submission until it is corrected. The submission timestamp is recorded only after all files pass these checks.

---

## Phase 10: Review, Revision & Approval

**Step 71 — Brand is notified of the submission and review window opens.**
On formal submission, the Brand receives an in-platform notification and an email notification. The review window opens: 48 hours on weekdays, 72 hours when the submission falls on a Friday, Saturday, or Sunday or on a public holiday configured for the Brand's operating region. The review window countdown is visible to both the Brand and the Creator in the workspace.

**Step 72 — Brand reviews the submitted content.**
The Brand watches or reads the submitted content and compares it against the campaign brief. The Brand checks for all mandatory inclusions, confirms the content format and duration are correct, evaluates the tone against the brief's guidance, and assesses the overall quality of the execution.

**Step 73 — Brand approves the content (first review).**
If the Brand is satisfied, they click Approve. Approval triggers the flat fee release if the payment model is flat fee or hybrid. For performance or hybrid campaigns, the content moves to the distribution stage and the milestone tracking engine begins monitoring the post once it is live. The Creator is notified of the approval and of any payment that has been released.

**Step 74 — Brand requests a first revision.**
If the Brand finds issues, they click Request Revision. The system requires the Brand to specify the issue before the request can be submitted. The specification must reference at least one element of the campaign brief — a specific mandatory inclusion that is missing, a content duration that is incorrect, a tone that does not match the guidance. A revision request with no brief reference is rejected by the system and the Brand must add a specific reason before proceeding. The Creator receives the revision request with the Brand's specific notes and has 48 hours to resubmit.

**Step 75 — Creator resubmits after the first revision.**
The Creator addresses the specific issues raised in the revision request, updates the content, and uploads a new set of files to the submission portal. The Creator marks this as a revised submission. The review window opens again from the start for the second review.

**Step 76 — Brand approves or requests a second revision.**
The Brand reviews the revised submission. If approved, the process follows Step 73. If the Brand still has concerns, they may submit a second revision request following the same process as Step 74. The revision count is now at two.

**Step 77 — Revision options are locked after round two.**
After the Brand submits a second revision request, the system records the revision count as two and permanently disables the ability to submit a third revision request on this submission milestone. The Brand will not see the revision request option again for this submission. The Creator resubmits after addressing the second round of notes.

**Step 78 — Brand is presented with the final decision.**
After the Creator resubmits following the second revision, the Brand's workspace shows two options only: Approve or Open Dispute. No other options are available. A visible notice explains that the maximum revision rounds have been reached for this submission and that these are the only paths forward. The review window countdown applies to this final decision as it does to all previous review rounds.

**Step 79 — Brand approves after the second revision.**
If the Brand clicks Approve, the process follows Step 73. The campaign moves to distribution and milestone tracking.

**Step 80 — Brand does not respond within the final review window.**
If the Brand takes no action within the review window after the second revision, the system auto-approves the submission at the moment the window expires. The Creator is notified of the auto-approval and any payment due on approval is released. The campaign moves to distribution and milestone tracking. The auto-approval is recorded in the campaign log.

**Step 81 — Brand opens a dispute after the second revision.**
If the Brand clicks Open Dispute, the Dispute Centre is engaged. The Campaign Workspace enters a frozen state. No new submissions, revision requests, or messages can be sent until the dispute is resolved. Both parties are notified that the dispute process has begun.

---

## Phase 11: Distribution & Publication

**Step 82 — Approved content is routed to distribution.**
On approval, the system checks the distribution method defined for each target platform. For platforms on which the Brand has selected direct posting, the content is delivered to the Brand's connected social account and posted automatically. The post URL is captured by the system and logged to the campaign record. For platforms on which a Distributor has been assigned, the content files and posting instructions are made available in the Distributor's workspace and the Distributor's 24-hour posting window begins.

**Step 83 — Brand's direct post is published.**
When the platform posts content directly through a Brand's connected social account, the system records the exact post URL, the posting timestamp, and the platform it was posted to. No human action is required on the Brand's side for this step. The Brand receives a confirmation notification that the content has been posted on their behalf. The post is now live and the milestone tracking engine begins monitoring it.

**Step 84 — Distributor posts and submits proof.**
This follows the process defined in Steps 62 through 64. On confirmed proof-of-post, the Distributor's assignment is marked complete and the 48-hour payment hold begins.

**Step 85 — All platforms are confirmed as live.**
Once every target platform shows a confirmed live post URL — whether from direct posting or from a Distributor's verified proof-of-post — the campaign status updates to Live. The milestone tracking engine is now actively monitoring all posts associated with this campaign. The Brand, Creator, and Manager all receive a notification that the campaign is live.

---

## Phase 12: Performance Tracking & Escrow Release

**Step 86 — Milestone tracking engine runs daily.**
Every 24 hours, the system's milestone tracking engine checks the verified view count on every live post associated with every active campaign. It does this by connecting directly to each social platform's API using the platform's own credentials — not any credential provided by the Creator or Distributor. The view counts retrieved are the raw figures provided by the social platform's own measurement system.

**Step 87 — Verified view count is calculated.**
From the raw view count returned by the API, the system applies the verification filter: it subtracts views from accounts flagged as inauthentic by the social platform, it subtracts repeat views from the same account within a 24-hour window, it subtracts views shorter than three seconds for video content, and it subtracts views from geographic regions the Brand excluded during campaign setup. The resulting number is the verified view count for that post for that day. It is added to the cumulative verified view total for the campaign.

**Step 88 — System checks whether a milestone has been crossed.**
After calculating the daily verified view count, the system compares the cumulative total against the next unclaimed milestone threshold for the campaign's selected tier. If the cumulative total equals or exceeds the threshold, a milestone release event is triggered immediately. If the cumulative total does not yet reach the threshold, the engine records the day's count and waits for the next daily run.

**Step 89 — Milestone release executes the payment waterfall.**
On a confirmed milestone crossing, the system executes the release as a single all-or-nothing transaction. The platform commission is deducted first. The Distributor fee is deducted next if this is the first milestone event and the Distributor's payment hold has cleared. The Creator's payment for this milestone tranche is calculated and credited to their wallet. The transaction is recorded in the permanent ledger. All parties receive a notification. If any step in the transaction fails, the entire release is rolled back and the system retries after one hour.

**Step 90 — Subsequent milestones release in the same way.**
Each time the cumulative verified view count crosses the next threshold in the selected tier, the same waterfall executes for the corresponding tranche of the performance budget. The Distributor fee is only paid on the first milestone event. All subsequent events flow directly to the platform commission and the Creator.

**Step 91 — Campaign window closes naturally.**
On the campaign close date set by the Brand during campaign creation, the system runs a final milestone check. If any portion of the performance budget remains unclaimed because the content did not reach the final milestone, the system calculates the return amount. Ten percent of the unspent performance budget is credited to the Creator as a completion acknowledgement for their effort on content that underperformed. The remaining 90 percent of the unspent amount is returned to the Brand's payment method. Both parties are notified and the campaign status is updated to Closed.

**Step 92 — Brand manually closes the campaign.**
If the Brand is satisfied that all deliverables have been met before the campaign window expires, they may manually close the campaign from their dashboard. On manual close, all remaining milestone tracking continues for 30 days to capture any late-breaking performance on content already published. After 30 days from the close date, the campaign is fully settled, remaining unspent budget is returned to the Brand under the same completion terms as Step 91, and the campaign is archived.

---

## Phase 13: Dispute Resolution

**Step 93 — Dispute is raised.**
A dispute may be raised by the Brand after the second revision round has been exhausted, or by either party at any point if they believe the other has materially breached the campaign terms. The party raising the dispute clicks the Open Dispute button in the Campaign Workspace and fills in the dispute form, specifying the nature of the issue and referencing the specific brief element or platform rule that has been violated. The dispute form cannot be submitted without this specification.

**Step 94 — Campaign Workspace is frozen.**
On dispute submission, the Campaign Workspace immediately enters a frozen state. No new content uploads, revision requests, or messages can be sent by either party. All existing files and messages remain fully visible to both parties and to the dispute resolver. The escrow remains locked. Neither party can access the campaign funds while a dispute is open.

**Step 95 — Dispute is assigned for resolution.**
The dispute is assigned to the Manager overseeing the Brand. The Manager receives a notification and has 72 hours from the time of assignment to review and issue a resolution decision. The Manager's review period begins at the moment the notification is sent, not at the moment the Manager first views the case.

**Step 96 — Manager reviews the dispute evidence.**
The Manager accesses the full dispute record through their dashboard. They review the complete campaign brief, every file submitted by the Creator, every revision request and the Creator's responses, the complete message history between the Brand and the Creator, and the reason stated by the party who raised the dispute. The Manager may not contact either party directly through the platform chat for purposes of dispute resolution — the decision must be based entirely on the documented evidence.

**Step 97 — Manager issues a resolution decision.**
The Manager selects one of four resolution outcomes. The first is approval with no changes, meaning the content meets the brief and the Creator is paid in full. The second is approval with one final required revision, meaning the content is close but has one specific correctable issue, and a third and final revision round is opened. The third is partial settlement, meaning both parties have partial merit, and the Manager proposes a percentage split of the flat fee — for example, 60 percent to the Creator and 40 percent returned to the Brand — which both parties must accept within 24 hours or the dispute escalates. The fourth is full rejection, meaning the content materially fails to meet the brief and the Brand receives a full refund minus the platform fee, with the Creator receiving no payment and a significant negative impact on their reputation score.

**Step 98 — Both parties are notified of the resolution.**
On the Manager issuing a decision, both the Brand and the Creator are notified immediately with the full reasoning for the decision. If the decision is a partial settlement, both parties have 24 hours to accept or reject it.

**Step 99 — Unresolved disputes escalate to the Admin.**
If the Manager does not issue a decision within 72 hours, or if either party rejects a partial settlement offer, or if the Manager's decision is contested through a formal escalation request, the dispute moves to the Admin level. The Admin has 96 hours to issue a final binding decision. No further escalation within the platform is possible after an Admin ruling. Legal remedies outside the platform remain available to either party per the platform's Terms of Service.

**Step 100 — Admin issues the final ruling.**
The Admin reviews all evidence including the Manager's notes and any escalation reasoning from the contesting party. The Admin may issue any of the same four resolution outcomes available to the Manager, or may create a custom settlement if the situation warrants it. The Admin's ruling is final and immediately executes the corresponding payment actions from the escrow.

---

## Phase 14: Payments & Withdrawals

**Step 101 — Creator views their earnings dashboard.**
The Creator's Earnings Dashboard shows the full history of their income on the platform: every campaign they have completed, every milestone payout received, every pending amount, and the current balance in their platform wallet. The dashboard also shows the Creator's cumulative verified performance data across all campaigns.

**Step 102 — Creator requests a withdrawal.**
When the Creator's wallet balance meets or exceeds the platform's minimum withdrawal threshold — set by the Admin, for example the equivalent of twenty US dollars — the Creator may request a withdrawal to their linked payment account. The Creator selects the amount they wish to withdraw and the payment account to receive it. Supported payment methods include bank transfer, mobile money, and other payment providers configured for the Creator's region by the Admin.

**Step 103 — Withdrawal is processed.**
On withdrawal request, the system verifies that the Creator's identity has been confirmed (a one-time identity verification step required before first withdrawal), that the wallet balance is sufficient, and that the requested amount does not include any funds that are still within a hold period. If all checks pass, the withdrawal is queued for processing. Processing times depend on the payment method and region. The Creator is notified when the withdrawal is initiated and again when it is confirmed as complete.

**Step 104 — Distributor receives payment after hold period.**
When a Distributor's proof-of-post passes all automated verification checks, a 48-hour hold begins. After 48 hours, if no content modification dispute has been raised by the Brand, the Distributor's fee is released from escrow and credited to the Distributor's wallet. The Distributor may then withdraw to their connected payment method following the same process as the Creator.

**Step 105 — Manager commission is swept monthly.**
On the first day of each calendar month, the system calculates the total campaign spend across all Brands managed by each Manager over the prior month, applies the Manager's commission percentage, and transfers the result to the Manager's wallet. The Manager receives a detailed commission statement showing the calculation per Brand. The Manager may withdraw from their wallet at any time once the balance exceeds the minimum threshold.

---

## Phase 15: Reputation & Quality Systems

**Step 106 — Creator reputation score is updated after each campaign closes.**
When a campaign closes and all deliverables are settled, the system recalculates the Creator's reputation score. The score is a weighted composite of four components. On-time delivery rate carries 30 percent of the weight and measures the percentage of submissions the Creator has made before their deadline across all campaigns. Revision request rate carries 20 percent and measures how often the Creator's submissions require revision relative to the total submissions made — a lower rate indicates consistently brief-compliant work. Brand satisfaction rating carries 30 percent and is derived from a 1-to-5 rating the Brand submits after each campaign across three dimensions: brief adherence, production quality, and communication. The verified performance index carries 20 percent and measures the Creator's historical average of actual verified views achieved relative to the milestone tier selected by the Brand — a Creator who consistently exceeds milestones scores higher on this dimension.

**Step 107 — Brand submits a satisfaction rating after campaign close.**
When a campaign is closed and settled, the system prompts the Brand to submit a satisfaction rating for the Creator. The rating form presents three questions, each on a 1-to-5 scale, covering how well the Creator adhered to the brief, how polished the production quality was, and how responsive the Creator was to communication during the campaign. The Brand must submit this rating within seven days of campaign close. If no rating is submitted within seven days, the campaign does not count toward or against the Brand satisfaction component of the Creator's reputation score.

**Step 108 — Reputation score consequences are applied automatically.**
A Creator whose reputation score falls below 40 is automatically removed from the public marketplace listing. Their profile is no longer visible to Brands browsing the Discovery Hub. They may still receive direct assignment invitations from Brands who already know them or Brands with whom they are rostered. To be relisted on the marketplace, the Creator must complete at least one campaign through a direct assignment with an approval outcome. A Creator whose score falls below 20 is flagged for Admin review. During the review period, the Creator cannot receive any new assignments. The Admin reviews the account and decides whether to reinstate it, place it on a supervised plan, or close it.

**Step 109 — Distributor reliability score is updated after each assignment.**
After each posting assignment is completed and verified, the Distributor's reliability score is updated. The score reflects their on-time posting rate, their post accuracy rate measured by how often their submissions pass all three automated verification checks on the first attempt, and their strike history. A Distributor who accumulates three reliability strikes within any 90-day period has their account suspended and their listing removed from the Distributor Directory. Reinstatement requires a review with the Admin and a 30-day waiting period.

---

## Phase 16: Analytics

**Step 110 — Brand views live campaign analytics.**
From their dashboard, the Brand can access a live analytics view for any active campaign. This view shows the current verified view count across all live posts, the engagement rate on each post, the cost per verified view calculated from the total spend to date divided by the total verified views, the milestone progress showing which milestones have been paid out and which remain, and a comparison of the current performance trajectory against the selected milestone tier's expected thresholds.

**Step 111 — Creator views their performance history.**
The Creator's dashboard includes a performance history section that shows the verified view counts achieved on every past campaign, the milestone tranches earned, the total income by campaign, and the Creator's average verified view count compared to the platform average for their niche. This comparison gives the Creator context for understanding how their performance ranks within the platform.

**Step 112 — Manager views aggregate analytics across all managed Brands.**
The Manager's analytics view shows the total number of active campaigns across all their Brands, the total escrow value locked, total payouts processed in the current month, the dispute rate across all campaigns, the average time from campaign creation to final approval, and the top-performing Creators across their managed Brands ranked by verified view count and Brand satisfaction rating.

**Step 113 — Admin views platform-wide metrics.**
The Admin's analytics console shows platform-wide health metrics updated in real time: total registered users by role, total campaigns created, total campaigns active, total campaigns closed, total escrow value locked, total payouts processed since platform launch, platform commission earned this month and year to date, average campaign completion time, dispute resolution rate, Creator churn rate, and Brand retention rate. These figures are used by the Admin for strategic decision-making and reporting.

---

## Phase 17: Admin Operations

**Step 114 — Admin manages Manager accounts.**
The Admin can create Manager accounts, configure each Manager's commission rate, suspend Manager accounts in cases of policy violation, and close Manager accounts permanently. When a Manager account is suspended, all Brands under that Manager are notified and assigned to a temporary holding state until the Admin reassigns them to a functioning Manager. Active campaigns are not interrupted by a Manager suspension — they continue to run under Admin oversight during the reassignment period.

**Step 115 — Admin configures platform-wide settings.**
The Admin sets the platform commission rate that applies to every escrow release. This rate cannot be overridden by any Manager or Brand. The Admin also configures the milestone tier table — the verified view thresholds for each tier and the corresponding payout percentages at each threshold. Changes to the milestone table apply only to campaigns created after the change is made. Active campaigns continue under the tier that was in place when they were created.

**Step 116 — Admin reviews flagged accounts.**
Accounts flagged by the system — Creators with a reputation score below 20, Distributors with three or more strikes in 90 days, Brands with multiple unresolved disputes — appear in the Admin's review queue. The Admin reviews each flagged account and decides on an action: clear the flag and reinstate full access, place the account on a supervised plan with restrictions, or close the account permanently.

**Step 117 — Admin issues final dispute rulings.**
The Admin is the final escalation point for any unresolved dispute. When a dispute reaches the Admin level, it appears in the Admin's dispute queue. The Admin reviews the full evidence record and issues a binding ruling within 96 hours. The ruling cannot be appealed within the platform.

**Step 118 — Admin monitors the audit log.**
The Admin has read-only access to the complete impersonation audit log covering all Manager impersonation sessions across the platform. The Admin can filter by Manager, by target account, by date range, and by action type. The Admin cannot edit or delete any audit record. If the Admin identifies a session that appears to involve an abuse of impersonation privileges, they may initiate an account review of the Manager in question.

---

## Phase 18: System-Wide Constraints (Always Active)

**Step 119 — Escrow is mandatory before any campaign proceeds.**
No campaign may move from the Funded status to the Open status without a fully confirmed escrow deposit matching the total campaign budget. This is enforced at the system level. No Manager, Admin, or Brand action can bypass this requirement. A campaign that loses its escrow funding after being set to Open — for example, due to a payment reversal — is immediately paused and all parties are notified.

**Step 120 — View counts are read-only for all users.**
No user of any role — Creator, Brand, Manager, Distributor, or Admin — may write, edit, or influence the view count figures stored against any post or campaign. View counts are written exclusively by the platform's server-side sync engine and are read-only in every user-facing interface. Any API endpoint that accepts a view count update from a client-authenticated request is a critical security vulnerability and must not exist.

**Step 121 — Revision requests are capped at two per submission.**
After two revision requests have been recorded on any single content submission, the ability to request a third revision is permanently disabled for that submission. This is enforced both in the user interface and at the server level. A server-side check rejects any request to record a third revision regardless of how it is sent.

**Step 122 — Direct Brand-Creator communication is restricted to active jobs.**
A Brand cannot send a message to a Creator outside of an active or pending job thread. A Creator cannot send a message to a Brand outside of an active or pending job thread. The platform's messaging system enforces this by allowing thread creation only when a job association exists between the two parties. This rule protects against off-platform recruitment and payment avoidance.

**Step 123 — Campaign windows cannot exceed 180 days.**
The system rejects any campaign creation attempt that sets a window longer than 180 days. Extensions beyond this limit are not available through the standard campaign settings. A Brand that requires a longer window must contact the Admin and provide a written justification. The Admin may grant an extension on a per-campaign basis, and this action is logged.

**Step 124 — Impersonation sessions are always audited.**
Every impersonation session by every Manager and Admin is logged with a written reason before the session begins, a timestamped record of every action taken during the session, and a sealed record when the session ends. These records cannot be edited or deleted by any user. The four-hour inactivity timeout applies to every session without exception.

**Step 125 — Creator social accounts must remain active throughout a campaign.**
If a Creator's verified social account is suspended, deactivated, or flagged as inauthentic by the social media platform during an active campaign, the Creator has 72 hours to notify the platform and provide a replacement verified account. If no replacement is provided within 72 hours, the campaign enters dispute status automatically. The Creator cannot continue working on the campaign until an active verified account is in place.

**Step 126 — Distributor modifications to content are detectable and consequential.**
Every file delivered to a Distributor from the Campaign Workspace has a unique fingerprint recorded by the system. When the Distributor submits a proof-of-post URL, the system reads the media from the live post and generates a fingerprint of that media. If the two fingerprints do not match, the content was modified before posting. This results in an automatic reliability strike, immediate escalation to the Brand, and the case entering the Dispute Centre. The Distributor does not receive payment for a confirmed modified post.

**Step 127 — Brands cannot block a payment if milestones are independently verified.**
If the platform's milestone tracking engine confirms that a verified view milestone has been crossed, the corresponding escrow release is automatic. A Brand cannot manually block, delay, or prevent a milestone-triggered release. The Brand's approval is required only for the flat fee component of a hybrid campaign, not for performance tranches that are verified by the platform's own tracking system.

**Step 128 — Exclusivity conflicts block assignment automatically.**
When any Creator assignment event occurs — whether through marketplace application or direct Brand assignment — the system checks for active exclusivity conflicts before allowing the assignment to proceed. If a conflict is found, the assignment is blocked and a clear explanation is shown to both parties. This check is not a warning the user can dismiss. It is a hard gate.

**Step 129 — Cancelled campaigns follow defined financial rules.**
A Brand that cancels before Creator assignment receives a full escrow refund minus a small listing fee covering the AI brief generation cost. A Brand that cancels after assignment but before any draft is submitted pays a kill fee of 50 percent of the flat fee to the Creator and receives the remainder of their budget back. A Brand that cancels after at least one draft has been submitted owes the Creator the full flat fee. The performance budget is returned to the Brand in full in all cancellation scenarios. These rules are enforced automatically by the system and cannot be negotiated through the platform — any contractual deviation must happen outside the platform.

**Step 130 — All financial transactions are recorded in the permanent ledger.**
Every movement of funds through the escrow system — deposits, releases, returns, fees, commissions, and withdrawals — is written to a permanent financial ledger in real time. The ledger is the authoritative record for all payment disputes. No figure in the ledger can be edited after it is written. The ledger is accessible to the Admin in full, to Managers for their managed Brands, to Brands for their own campaigns, and to Creators for their own earnings. No party can see another party's financial data except where their role explicitly grants cross-party visibility.

---

*End of AMPLIFYR Complete System Logic*
*This document incorporates and supersedes all prior session specifications and supplements.*
*Version: Complete | All Phases | All Roles | All Constraints*