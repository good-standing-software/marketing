---
name: crm
description: Research, score, and rank UK membership bodies in the Notion CRM, and prepare survey outreach to them. Use when adding or updating a body, recomputing send order, or drafting or recording a survey email.
---

# Membership bodies CRM

The Notion database is the source of truth: one row per organisation, holding its research, outreach, and survey results. There is no second contacts or survey database.

- Database: https://app.notion.com/p/89e3bfa0b57149fa8ab6952d4d8f2170
- Data source: `collection://9952bb13-ed3e-4c1f-8df5-6f65b98c4323` (also the parent `data_source_id` for new pages)
- Views: **Overview**, **Send queue**, **Outreach**, **Score audit**, **Clusters**
- Score audit (`view://3d5ec775-22dd-8163-8e55-000ca563749b`) lists Active rows whose score is unchecked. Query it in view mode. Empty is required, not proof of completion.

Fetch the live schema before writing; property names change.

## Ask Mark first

Trashing a page; emailing anyone; changing Wave on a row already sent or engaged; collapsing Sector options; renaming `MemberMojo / lightweight`.

## Uniqueness

Before creating a row, search the name, acronym, and proposed slug with `notion-search` scoped to the data source, without `filters.title_only`. If a page exists, update it. If two pages describe one body, keep the richer one and mark the other Exclude with `Duplicate of {slug}`. Never create a row you could not uniqueness-check.

The slug is the full name in lowercase kebab case, e.g. `rehabilitation-workers-professional-network`.

## Wave

Wave is eligibility, set before scoring: `Active`, `Backlog`, or `Exclude`.

**Active** needs all of:

1. UK geography: UK-wide, a UK nation, or international with a UK base serving UK members.
2. A membership body or voluntary register, not a statutory regulator.
3. Continuing education is a condition of membership or register standing: evidenced hours, points, or a documented cycle check. Encouragement, a course catalogue, or another body's pathway is not a condition.
4. Under 5000 members, or unknown and plausibly small.
5. Not on an enterprise AMS.
6. An official inbox or site to email.

**Exclude**: statutory regulators; continuing education only because a statute or regulator (GDC, GOC, HCPC) requires it (**Statutory only**); continuing education only for a diploma or fellowship (**Programme only**); off-ICP; or asked not to be contacted. Keep Exclude rows, with an **Exclude reason** sentence, so nobody researches them again.

**Backlog**: over 5000 members, an enterprise AMS, no UK standing gate, or nothing to email.

## Fit score

**Fit score** is a Notion formula over five number fields. Write the components and **Score breakdown** as `standing N / size N / systems N / admin N / contact N`, never Fit score itself.

| Component | Max | Scoring |
|---|---|---|
| Standing | 35 | Membership or register hours 35; accreditation or re-accreditation 25; encouraged only 5; none, programme-only, or statutory-only 0 |
| Size | 25 | Under 300: 25; 300–500: 22; 501–2000: 16; 2001–5000: 8; over 5000: 0; unknown: 12 |
| Systems | 20 | Manual or spreadsheets 20; lightweight AMS without a continuing-education module (Wild Apricot, MemberMojo) 18; website CMS only 16; custom 8; named mid-market AMS 6; enterprise 0; unknown 8 |
| Admin | 10 | Volunteers 10; mix 8; unknown 5; paid staff 4; external or AMC 2 |
| Inbox | 10 | Membership email and phone 10; email only 7; none 0 |

Example: RWPN = 35 + 22 + 18 + 8 + 10 = 93.

- "Over 500" is 501–2000. No published count is unknown, 12.
- Assessed-entry genealogy bodies with study days but no hour cycle (AGRA, ASGRA) are Backlog with standing 5.
- **Score notes**: one or two sentences of why; no contact details.
- **Score checked**: true when Fit score equals the breakdown sum and Wave follows the rules above.

## Send rank

Send rank orders survey emails. It is a unique integer `1…N` across rows that are Active, not opted out, Researched or Deepened, and have a Primary email; every other row has none. Sort by Fit score descending, then Evidence confidence (High, Medium, Low), then Slug. After any create or score change, recompute the whole set, or leave Send rank empty and say so if `query_data_sources` is unavailable.

## Fields

Each fact lives in one typed field; fetch the schema for the full list. The locked selects take only these values:

| Property | Values |
|---|---|
| Wave | Active, Backlog, Exclude |
| Org type | Professional body, Trade association, Learned society, Regulatory or register, Other |
| Geography | UK-wide, England, Scotland, Wales, Northern Ireland, International with UK base |
| Governance | Ltd by guarantee, Royal charter, Charity / CIO, Unincorporated, Statutory, Unknown |
| Email angle | Spreadsheets, Lightweight AMS, Volunteer admin, Register audit, Fees, Unknown |
| Standing gate | Membership standing, Register standing, Accreditation track, Programme only, Statutory only, None, Unknown |
| ICP lane | Core, Adjacent, Out |
| Sendable | Yes, No |
| Survey status | Not sent, Link sent, Started, Completed, Declined, Do not contact |

- **Sector** uses the coarse tags already present; the profession goes in **Discipline**.
- **Membership system** is the class; the product name goes in **AMS product**. Wild Apricot is `MemberMojo / lightweight`.
- **Members band** matches **Members estimate**; **Count year** is the estimate's year, empty if undated. **Gated headcount** is the people the standing gate applies to.
- **Continuing education required** is Yes only for a membership, register, or accreditation gate; **CE requirement** records the hours either way, e.g. `250 hours / 5 years (UKCP)`.
- **Primary email** and **Secondary email** are official inboxes only, never a named person's or an umbrella body's. No official inbox means Inbox 0, **Email quality** None, and **Sendable** No.
- **Contact notes** holds only what no typed field holds.
- **Sources** is URLs only. **Evidence confidence**: High is the body's own pages plus a legal or register source; Medium is the site only; Low is a list mention or a dead site.
- **Research status**: Stub (name only), Researched (fields and a short body), Deepened (standing gate evidenced). **Last researched** is the day you checked.
- **ICP lane**: Core is Active with standing work likely still manual (volunteer or mixed admin; manual, CMS, or lightweight systems). Adjacent is Active but staffed, mid-size, on a known AMS or custom portal, or gated only on an accreditation track. Out is Backlog or Exclude.
- **Persona**, **Lead source**, **Next action**, **Campaign**, and **Cluster** take the options already on the schema. **Warmth** is Cold until they reply; **Owner** is Mark.
- **Outreach hook**, **Pain hypothesis**, and **Why now** are one sentence each, and never say "CPD". A body's own scheme name may appear in Sources or Exclude reason.

## Page body

Replace the whole body with:

```
## Purpose
## Establishment
## Governance
## Contacts
## Website & systems
## Fit notes
```

Fit notes says the Wave, the standing gate, and why the score is what it is. Update **Last researched** and **Exclude reason** rather than appending check notes.

## Research

1. Fetch the schema. Search for the body before creating it.
2. Read the body's own membership, register, and continuing-education pages. A regulator's pages support but never establish a membership gate.
3. Fill the typed fields, then the body, then the scores. Set Wave from eligibility.
4. Recompute Send rank, or leave it empty.
5. Fetch the page you wrote and check: Sources are URLs, the breakdown sums to Fit score, no contact detail is repeated, and a Stub has no Send rank.

To research many bodies, clear Score audit first. Then give 2–4 read-only subagents disjoint batches from the official lists (HMRC List 3, the PSA register, the charity registers), with this skill, the data source, and the slugs already in their slice. Each returns one line per name: already slugged (title and page id), sits on an existing slug, do not slug (with why), needs a coordinator check, or unique and ready (slug, gate, Wave, lane, score breakdown, official inbox, 3–8 source URLs). Subagents never write to the CRM. Merge the results, search the unique names again, and create two or three rows at a time yourself, with Score checked true, Last researched today, Campaign Not queued, and Send rank empty, until Score audit is clear. A body that was absorbed or dissolved but is still on an official list is created as Exclude.

## Survey outreach

Every recipient gets the same subject and letter. Never write as if we know the organisation: no name, systems, size, or other facts from the CRM.

Subject: `A Short Survey`

Drafts are HTML in Gmail (`mark@goodstanding.co.uk`); never as files. Linked words are bold:

```
Hello,

I'm Mark from Good Standing Software. We're researching how UK professional bodies handle membership, fees and good standing.

Before building anything, we want to know whether there's a real need — and where the manual work becomes messy enough that software would actually help.

The **survey** is confidential, there's nothing to buy, and it takes about 5–7 minutes. Your help would be greatly appreciated.

Thanks,

Mark

**Good Standing Software Ltd**
Registered in England and Wales. Company no. **17423889**. ICO no. **ZC233855**.
71-75 Shelton Street, Covent Garden, London WC2H 9JQ

**Privacy notice**

To opt out, please reply **stop**.
```

| Text | Link |
|---|---|
| survey | `https://research.goodstanding.co.uk/r/Bz5oLN?ref={Slug}&email={URL-encoded Primary email}` |
| Good Standing Software Ltd | `https://goodstanding.co.uk/?utm_source=survey&utm_medium=email&utm_campaign=research` |
| 17423889 | `https://find-and-update.company-information.service.gov.uk/company/17423889` |
| ZC233855 | `https://ico.org.uk/ESDWebPages/Entry/ZC233855` |
| Privacy notice | `https://goodstanding.co.uk/privacy/?utm_source=survey&utm_medium=email&utm_campaign=research` |

- Only the recipient, `ref`, and `email` change per send; no other query parameters.
- Links open in a new tab. The Gmail API rewrites `href`s through `google.com/url`, so write clean URLs and finish the links in the Gmail UI.
- Mark sends. Only after he confirms a send, set Survey status `Link sent`, **Last surveyed** and **Last contacted** to that day, and move **Next action** on.
- When someone replies "stop", set **Opted out** and **Opted out on**.
