# Creative brief templates (for generating the pre-filled Google Doc)

The brief is a real Google Doc that must match Bing Bang's house format **exactly**: a title, short
section intros, and a **two-column table** under each section (bold field label on the left, value on
the right). Two house templates exist — pick the right one, fill every cell, and delete the sections
this project doesn't need.

## How to create it so the tables are REAL tables

Build the brief as **HTML** and create it with the Drive tool using `contentMimeType: text/html`
(leave conversion on — Drive turns HTML `<table>`s into real Google Doc tables). **Do NOT use
`text/plain`** — plain text produces a wall of text with no tables and does not match the house format.

```
create_file(
  title: "[CODE]-[number] _ Kickoff & Creative Brief",
  parentId: <project subfolder id>,
  contentMimeType: "text/html",
  textContent: <the filled HTML below>
)
```

Capture the returned `viewUrl` for the Overview link (SKILL.md Step 9).

## Rules (do these every time)

1. **ALWAYS ask the operator which template to use.** Propose one based on project type — Video →
   Video Production Brief; everything else → general Creative Brief — but never silently default.
   Confirm before generating (a hard rule from Josh).
2. **Fill every cell.** Use the real values gathered at intake. Only write `TBD` when the value is
   genuinely unknown at acceptance. Never leave a cell blank.
3. **Delete sections this project doesn't need.** Remove the entire `<h2>` + its table for anything
   that doesn't apply — e.g. a video with no interviews drops *Interview Direction*; a general project
   with no website work drops *Optional: Website Work*. Don't leave empty scaffolding behind.
4. **Keep the field rows in the order shown.** The order is the house format.

---

## Template A — Project Kick Off & Video Production Brief

Use for **Video** projects (shoots, brand films, testimonial/interview pieces, reels packages).
Sections: Quick Reference Info, Intake, Discovery, Strategy / Brief, Core Needs, Story Structure,
Video Creative Direction, Interview Direction (delete if no interviews), Production Logistics,
Shoot Day Schedule(s), Assets / Access Needed, Post-Production / Delivery, Distribution / Usage.

```html
<h1>Project Kick Off &amp; Video Production Brief</h1>
<p>A simple, reusable brief for aligning video scope, creative direction, production needs, shoot planning, and delivery requirements.</p>
<p><strong>Client:</strong> [Client Name] ([CODE]) &nbsp; <strong>Project:</strong> [Project Name] &nbsp; <strong>Bonsai #:</strong> [number] &nbsp; <strong>Brief created:</strong> [date] &nbsp; <strong>Project lead:</strong> [name]</p>

<h2>Quick Reference Info</h2>
<table border="1">
  <tr><td><strong>Job Number</strong></td><td>[Bonsai # or job no.]</td></tr>
  <tr><td><strong>Shoot Date</strong></td><td>[date or TBD]</td></tr>
  <tr><td><strong>Shoot Location</strong></td><td>[location or TBD]</td></tr>
  <tr><td><strong>Client Name</strong></td><td>[Client Name]</td></tr>
  <tr><td><strong>Client Contact Info</strong></td><td>[name / email / phone]</td></tr>
</table>

<h2>Intake</h2>
<table border="1">
  <tr><td><strong>Scope</strong></td><td>[scope]</td></tr>
  <tr><td><strong>Owner</strong></td><td>[AM / owner]</td></tr>
  <tr><td><strong>Specialist</strong></td><td>[specialist]</td></tr>
  <tr><td><strong>Budget</strong></td><td>[billing type + fee]</td></tr>
  <tr><td><strong>Timing</strong></td><td>[start] – [target due/launch]</td></tr>
  <tr><td><strong>Deliverables</strong></td><td>[deliverables]</td></tr>
</table>

<h2>Discovery</h2>
<table border="1">
  <tr><td><strong>Files</strong></td><td>[links]</td></tr>
  <tr><td><strong>Stakeholder contacts</strong></td><td>[contacts]</td></tr>
  <tr><td><strong>References</strong></td><td>[links]</td></tr>
  <tr><td><strong>Existing assets</strong></td><td>[assets]</td></tr>
  <tr><td><strong>Required approvals</strong></td><td>[approver(s)]</td></tr>
</table>

<h2>Strategy / Brief</h2>
<table border="1">
  <tr><td><strong>Objective</strong></td><td>[objective]</td></tr>
  <tr><td><strong>Goal of the video</strong></td><td>[goal]</td></tr>
  <tr><td><strong>Target audience</strong></td><td>[audience]</td></tr>
  <tr><td><strong>Success metric</strong></td><td>[metric]</td></tr>
  <tr><td><strong>Deliverables list</strong></td><td>[list]</td></tr>
  <tr><td><strong>Production plan</strong></td><td>[plan]</td></tr>
</table>

<h2>Core Needs</h2>
<p>Identify what matters most so production can focus on the highest-value footage and deliverables.</p>
<table border="1">
  <tr><td><strong>Primary message</strong></td><td>[message]</td></tr>
  <tr><td><strong>Must-have content</strong></td><td>[content]</td></tr>
  <tr><td><strong>Required people / subjects</strong></td><td>[people]</td></tr>
  <tr><td><strong>Required locations</strong></td><td>[locations]</td></tr>
  <tr><td><strong>Available B-roll dates</strong></td><td>[dates]</td></tr>
  <tr><td><strong>Recommendation of priority</strong></td><td>[priority]</td></tr>
</table>

<h2>Story Structure</h2>
<p>Outline the narrative flow before scripting, shooting, or editing.</p>
<table border="1">
  <tr><td><strong>Hook / opening</strong></td><td>[hook]</td></tr>
  <tr><td><strong>Problem / context</strong></td><td>[context]</td></tr>
  <tr><td><strong>Main story beats</strong></td><td>[beats]</td></tr>
  <tr><td><strong>Proof points / examples</strong></td><td>[proof]</td></tr>
  <tr><td><strong>Resolution / payoff</strong></td><td>[payoff]</td></tr>
  <tr><td><strong>Call to action</strong></td><td>[CTA]</td></tr>
</table>

<h2>Video Creative Direction</h2>
<p>Define what the video should communicate and how it should feel.</p>
<table border="1">
  <tr><td><strong>Video type / format</strong></td><td>[type/format]</td></tr>
  <tr><td><strong>Core message / story</strong></td><td>[message]</td></tr>
  <tr><td><strong>Tone / style</strong></td><td>[tone]</td></tr>
  <tr><td><strong>Script or interview needs</strong></td><td>[needs]</td></tr>
  <tr><td><strong>Key shots / moments</strong></td><td>[shots]</td></tr>
  <tr><td><strong>Visual references</strong></td><td>[references]</td></tr>
  <tr><td><strong>Call to action</strong></td><td>[CTA]</td></tr>
</table>

<h2>Interview Direction, if Necessary</h2>
<p>Use this section when the video includes interviews, testimonials, leadership commentary, or subject-matter input. <em>Delete this whole section if there are no interviews.</em></p>
<table border="1">
  <tr><td><strong>Interview needed?</strong></td><td>[yes/no]</td></tr>
  <tr><td><strong>Interview subject(s)</strong></td><td>[subjects]</td></tr>
  <tr><td><strong>Key questions</strong></td><td>[questions]</td></tr>
  <tr><td><strong>Soundbites needed</strong></td><td>[soundbites]</td></tr>
  <tr><td><strong>Topics to avoid</strong></td><td>[topics]</td></tr>
  <tr><td><strong>On-camera direction</strong></td><td>[direction]</td></tr>
  <tr><td><strong>Release / approval needs</strong></td><td>[releases]</td></tr>
</table>

<h2>Production Logistics</h2>
<p>Capture the practical details needed to plan and execute the shoot.</p>
<table border="1">
  <tr><td><strong>Shoot date(s)</strong></td><td>[dates]</td></tr>
  <tr><td><strong>Location(s)</strong></td><td>[locations]</td></tr>
  <tr><td><strong>Talent / interviewees</strong></td><td>[talent]</td></tr>
  <tr><td><strong>Crew / roles needed</strong></td><td>[crew]</td></tr>
  <tr><td><strong>Props / wardrobe / products</strong></td><td>[props]</td></tr>
  <tr><td><strong>Permits / releases</strong></td><td>[permits]</td></tr>
  <tr><td><strong>Travel / access notes</strong></td><td>[notes]</td></tr>
</table>

<h2>Shoot Day Schedule(s)</h2>
<p>Build a simple schedule for each shoot day so the team knows what happens when.</p>
<table border="1">
  <tr><td><strong>Shoot day 1 date / time</strong></td><td>[date/time]</td></tr>
  <tr><td><strong>Location / setup</strong></td><td>[setup]</td></tr>
  <tr><td><strong>Scene / segment order</strong></td><td>[order]</td></tr>
  <tr><td><strong>Interview time(s)</strong></td><td>[times]</td></tr>
  <tr><td><strong>B-roll time(s)</strong></td><td>[times]</td></tr>
  <tr><td><strong>Breaks / resets</strong></td><td>[breaks]</td></tr>
  <tr><td><strong>Wrap time</strong></td><td>[wrap]</td></tr>
</table>

<h2>Assets / Access Needed</h2>
<p>List everything needed before production or editing begins.</p>
<table border="1">
  <tr><td><strong>Brand guidelines</strong></td><td>[guidelines]</td></tr>
  <tr><td><strong>Logos / graphic assets</strong></td><td>[logos]</td></tr>
  <tr><td><strong>Existing footage / b-roll</strong></td><td>[footage]</td></tr>
  <tr><td><strong>Photos / product assets</strong></td><td>[photos]</td></tr>
  <tr><td><strong>Music / licensing needs</strong></td><td>[music]</td></tr>
  <tr><td><strong>Location access</strong></td><td>[access]</td></tr>
  <tr><td><strong>Account / platform access</strong></td><td>[access]</td></tr>
</table>

<h2>Post-Production / Delivery</h2>
<p>Define what needs to happen after the shoot and what final files are expected.</p>
<table border="1">
  <tr><td><strong>Edit length(s)</strong></td><td>[lengths]</td></tr>
  <tr><td><strong>Aspect ratio(s)</strong></td><td>[ratios]</td></tr>
  <tr><td><strong>Captions / subtitles</strong></td><td>[captions]</td></tr>
  <tr><td><strong>Graphics / lower thirds</strong></td><td>[graphics]</td></tr>
  <tr><td><strong>Music / sound design</strong></td><td>[sound]</td></tr>
  <tr><td><strong>Review rounds</strong></td><td>[#]</td></tr>
  <tr><td><strong>Final file formats</strong></td><td>[formats]</td></tr>
  <tr><td><strong>Delivery location</strong></td><td>[location]</td></tr>
</table>

<h2>Distribution / Usage</h2>
<p>Clarify where the video will live and how success will be measured.</p>
<table border="1">
  <tr><td><strong>Channel(s)</strong></td><td>[channels]</td></tr>
  <tr><td><strong>Placement / usage</strong></td><td>[placement]</td></tr>
  <tr><td><strong>Thumbnail needs</strong></td><td>[thumbnail]</td></tr>
  <tr><td><strong>Title / description copy</strong></td><td>[copy]</td></tr>
  <tr><td><strong>Posting or launch date</strong></td><td>[date]</td></tr>
  <tr><td><strong>Paid or organic</strong></td><td>[paid/organic]</td></tr>
  <tr><td><strong>Reporting metrics</strong></td><td>[metrics]</td></tr>
</table>
```

---

## Template B — Project Kick-off & Creative Brief (general)

Use for **Social Media, Campaign, Website, Branding, Email**, and anything non-video. Core sections:
Intake, Discovery, Strategy / Brief. Then the two optional add-on sections — **keep only the ones the
project needs and delete the rest**.

```html
<h1>Project Kick-off &amp; Creative Brief</h1>
<p>A simple, reusable brief for aligning project scope, inputs, and production direction.</p>
<p><strong>Client:</strong> [Client Name] ([CODE]) &nbsp; <strong>Project:</strong> [Project Name] &nbsp; <strong>Bonsai #:</strong> [number] &nbsp; <strong>Brief created:</strong> [date] &nbsp; <strong>Project lead:</strong> [name]</p>

<h2>Intake</h2>
<table border="1">
  <tr><td><strong>Scope</strong></td><td>[scope]</td></tr>
  <tr><td><strong>Owner</strong></td><td>[AM / owner]</td></tr>
  <tr><td><strong>Specialist</strong></td><td>[specialist]</td></tr>
  <tr><td><strong>Budget</strong></td><td>[billing type + fee]</td></tr>
  <tr><td><strong>Timing</strong></td><td>[start] – [target due/launch]</td></tr>
  <tr><td><strong>Deliverables</strong></td><td>[deliverables]</td></tr>
</table>

<h2>Discovery</h2>
<table border="1">
  <tr><td><strong>Files</strong></td><td>[links]</td></tr>
  <tr><td><strong>Stakeholder contacts</strong></td><td>[contacts]</td></tr>
  <tr><td><strong>References</strong></td><td>[links]</td></tr>
  <tr><td><strong>Existing assets</strong></td><td>[assets]</td></tr>
  <tr><td><strong>Required approvals</strong></td><td>[approver(s)]</td></tr>
</table>

<h2>Strategy / Brief</h2>
<table border="1">
  <tr><td><strong>Objective</strong></td><td>[objective]</td></tr>
  <tr><td><strong>Target audience</strong></td><td>[audience]</td></tr>
  <tr><td><strong>Success metric</strong></td><td>[metric]</td></tr>
  <tr><td><strong>Deliverables list</strong></td><td>[list]</td></tr>
  <tr><td><strong>Production plan</strong></td><td>[plan]</td></tr>
</table>

<!-- OPTIONAL ADD-ONS: keep only what applies, delete the rest -->

<h2>Optional: Website Work</h2>
<table border="1">
  <tr><td><strong>Current website / domain</strong></td><td>[domain]</td></tr>
  <tr><td><strong>Page(s) or section(s) needed</strong></td><td>[pages]</td></tr>
  <tr><td><strong>Platform / CMS</strong></td><td>[platform]</td></tr>
  <tr><td><strong>Hosting / login access</strong></td><td>[access]</td></tr>
  <tr><td><strong>Sitemap or navigation needs</strong></td><td>[sitemap]</td></tr>
  <tr><td><strong>Content needed</strong></td><td>[content]</td></tr>
  <tr><td><strong>Forms / integrations</strong></td><td>[forms]</td></tr>
  <tr><td><strong>SEO requirements</strong></td><td>[SEO]</td></tr>
  <tr><td><strong>Tracking / analytics</strong></td><td>[tracking]</td></tr>
  <tr><td><strong>Launch requirements</strong></td><td>[launch]</td></tr>
</table>

<h2>Optional: Social Media / Digital Work</h2>
<table border="1">
  <tr><td><strong>Channel(s)</strong></td><td>[channels]</td></tr>
  <tr><td><strong>Campaign or content theme</strong></td><td>[theme]</td></tr>
  <tr><td><strong>Post / ad formats</strong></td><td>[formats]</td></tr>
  <tr><td><strong>Copy needs</strong></td><td>[copy]</td></tr>
  <tr><td><strong>Creative asset needs</strong></td><td>[assets]</td></tr>
  <tr><td><strong>Audience / targeting</strong></td><td>[targeting]</td></tr>
  <tr><td><strong>CTA / destination link</strong></td><td>[CTA]</td></tr>
  <tr><td><strong>Posting or flight dates</strong></td><td>[dates]</td></tr>
  <tr><td><strong>Budget / spend</strong></td><td>[spend]</td></tr>
  <tr><td><strong>Reporting metrics</strong></td><td>[metrics]</td></tr>
</table>
```

---

(House-format source docs: Video Production Brief `1buXVcgoOdoVXjL66sRZk1FzQuNdpJmo4Hq7XdY8Yg4w`;
general Creative Brief `1i710mvGsWPMEkhM1wYp_MXwAEWXGt6iTFJFGDTr8WhM`.)
