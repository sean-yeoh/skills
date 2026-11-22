---
name: terms-and-privacy
description: "Draft or update Terms of Service (Terms and Conditions) and a Privacy Policy for a website, app, SaaS product, or API. Use when the user asks to write, generate, update, or review terms, T&C, ToS, a privacy policy, a privacy notice, a cookie policy, or /legal page content. Scans the codebase for real data flows and third-party services, asks only for missing facts, applies the rules of each jurisdiction that applies (EU/UK GDPR, US state laws, COPPA, Australia, and others), and marks every assumption and placeholder. Produces drafts for lawyer review, not legal advice."
---

# Terms and Privacy

## Scope

This skill drafts and updates these documents:

- Terms of Service (also called Terms and Conditions or Terms of Use).
- Privacy Policy (also called Privacy Notice).
- On request: a cookie policy, short notices at the point of collection (for example, under a sign-up form), and an "Acceptable Use Policy" split out from the terms.

Do not use this skill to draft data processing agreements, enterprise master service agreements, employee privacy notices, HIPAA notices of privacy practices, or contracts that a counterparty negotiates. Tell the user that these need a different template or a lawyer.

The output is a draft. It is not legal advice. Never state or imply that a document is "compliant", "fully GDPR compliant", or "lawyer approved".

## Rules

1. **The documents must describe what the product actually does.** A privacy policy that does not match the real data practices is worse than no policy: regulators enforce the promises in it. Get the facts from the codebase and the user. Do not write a practice into a document because it is common.
2. **Never invent identity or contact facts.** Do not guess the legal entity name, trading name, registered or postal address, company number, phone number, contact emails, DPO, EU or UK representative, or governing law. Ask for them, or use a bold placeholder such as **[LEGAL_ENTITY_NAME]**.
3. **Mark every assumption.** When you fill in a fact that the user or the codebase did not give, write `[ASSUMPTION: ...]` inline at that location.
4. **Verify time-sensitive law.** Privacy and consumer law changes often. The reference files record facts as of their "Verified" date. Before you rely on a threshold, deadline, effective date, or new requirement, check it against a primary source (legislation, regulator guidance) when web access is available. If you cannot check it, say so in the draft notes.
5. **Your memory is not a legal source.** Cite only articles, sections, and laws that you have confirmed. Do not invent article numbers, case names, or regulator guidance.
6. **Keep the two documents consistent.** Use the same entity name, product name, minimum age, contact details, governing law, defined terms, and "Last updated" date in both. The terms link to the privacy policy. The privacy policy does not repeat the terms.
7. **Do not weaken rights that the law gives.** Do not draft clauses that exclude non-excludable consumer rights (for example, Australian Consumer Law guarantees, EU and UK statutory rights, or liability for death or personal injury caused by negligence). Use "to the maximum extent permitted by law" and add the required carve-outs.

## Workflow

### 1. Find what already exists

1. Search the repository for existing legal documents: `terms`, `tos`, `privacy`, `legal`, `cookie` in file names, routes, and content (MDX, Markdown, HTML, JSX, CMS fixtures).
2. If documents exist, this is an update. Read them in full. Keep their structure and defined terms unless the user asks for a rewrite. Find the "Last updated" date and any version history.
3. Find how the project renders legal pages (route, component, content directory, styling). The output must fit that pattern.

### 2. Scan the codebase for facts

If a codebase is available, read `references/codebase-scan.md` and follow it. The scan finds: the personal data the product collects, the third-party services that receive it, cookies and tracking, authentication methods, payments and subscriptions, user-generated content, AI features, email sending, and hosting regions.

Record each finding with its evidence (file path and the dependency, import, or field). Present the findings to the user for confirmation. A dependency in `package.json` that the code never calls is not a data flow: check usage before you list it.

### 3. Ask for the missing facts

Ask for everything that the scan did not answer, in **one message**. Group the questions. Show what you already found so the user only confirms or corrects it. Skip questions that the scan answered. Read `references/intake.md` for the full question list.

The facts that you must always have (or mark as placeholders) are:

- Legal entity name, and the trading or registered business name if it is different.
- Country and state of establishment, and registered address.
- Postal address for mail and legal notices, if it is different from the registered address.
- The countries and regions where the product has users, or that it targets.
- Contact emails: legal notices, privacy requests, and (if there is user content) abuse reports. A contact phone number, if the business wants to publish one or a law requires one.
- Whether users are consumers, businesses, or both.
- Minimum user age, and whether children could use the product.
- Whether the business sells personal data, shares it for targeted advertising, or uses it to train AI models.

If the user wants a draft now, write it with placeholders and assumptions. Do not block on answers.

### 4. Decide which laws apply

Use the entity location, user locations, business size, and data practices to decide which jurisdiction references apply. Load each one that applies:

| Condition | Load |
|---|---|
| Entity in the EU, EEA, or UK, or the product offers services to, or monitors, people there | `references/eu-uk.md` |
| Entity in the US, or US users | `references/us.md` |
| Entity in Australia, or Australian users | `references/australia.md` |
| Users in Canada, Brazil, India, Japan, Singapore, South Korea, China, or other countries | `references/other-jurisdictions.md` |

For a global product, apply the strictest applicable rule as the baseline. Add jurisdiction sections (for example, "Additional information for California residents") only where a law requires specific disclosures. Record in the draft notes which laws you applied and which you did not, with the reason for each.

Stop and recommend a lawyer before you publish if any of these are true. Still write the draft if the user asks, but list the risk first in the draft notes:

- The product is directed to children, or is likely to be used by children (education, games, social).
- The product processes health, biometric, genetic, precise location, or financial account data as a core feature.
- The business sells personal data, or is a data broker.
- The product makes automated decisions with legal or similarly significant effects (credit, hiring, housing, insurance, education).
- The product is in a regulated sector (finance, health, insurance, telecommunications, gambling).

### 5. Draft the documents

Read `references/privacy-policy.md` and `references/terms.md` for the sections and the rules for each section. Include a section only when it applies, but do not omit a required disclosure. If a required disclosure has no answer, use a placeholder.

Start each draft with a **Draft notes** block that the user must delete before publishing:

```markdown
> **Draft notes - delete this block before publishing**
>
> This draft was prepared with AI assistance as a starting point. It is not legal advice and does not create a lawyer-client relationship. Have a qualified lawyer in each relevant jurisdiction review it before you publish or rely on it.
>
> - Laws applied: ...
> - Laws not applied, and why: ...
> - Facts not verified against a primary source: ...
> - Risks that need a lawyer: ...
```

End each draft with a **Publishing checklist** inside the same kind of block: every placeholder, every `[ASSUMPTION: ...]`, and every product change the document depends on (for example, "Add a 'Do Not Sell or Share' link to the footer", "Block analytics cookies until consent", "Add a cancellation flow").

### 6. Check the drafts

Before you give the drafts to the user, check each item:

1. Every data category, purpose, and recipient in the privacy policy matches a finding from the scan or a fact from the user.
2. Every third-party service found in the scan appears in the privacy policy, or you recorded why it does not (for example, it receives no personal data).
3. No statement contradicts the code. Examples: "We do not use cookies" while an analytics script sets cookies; "We do not sell or share your data" while an advertising pixel sends data to an ad network (this can be "sharing" under the CCPA).
4. Each jurisdiction requirement from step 4 has a matching section or is in the draft notes.
5. The terms and the privacy policy are consistent (rule 6).
6. No clause tries to exclude a non-excludable right (rule 7).
7. No compliance claims, no invented facts, and no invented citations.

### 7. Write the output

- Default format: Markdown, with `##` for sections and `###` for subsections. Use lists and tables where they make the text easier to scan.
- If the project already renders legal pages, write the files in its format and location (for example, MDX in `content/legal/`, or a page component). Markdown stays the source of truth if the user wants both.
- Write in plain language for the product's users. Use precise legal terms only where they change the meaning (for example, "controller", "sale", "sharing").
- Use the date the user gives for "Last updated". If they give none, use today's date and mark it as an assumption.
- For an update, give the user a short summary of the material changes. Material changes to a privacy policy or terms usually need notice to existing users before they take effect. Say so, and state the notice method that the current documents promise.

After you write the files, report: the files written, the laws applied, the open placeholders and assumptions, the product changes needed, and the risks that need a lawyer. Offer section-by-section refinement (plainer, more protective, shorter, or more product-specific).
