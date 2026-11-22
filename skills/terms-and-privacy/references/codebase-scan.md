# Codebase scan

The scan finds the facts that the documents must describe. Record each finding with its evidence: the file path and the dependency, import, field, or call. Confirm every finding with the user before it goes into a document.

## 1. Dependencies and services

Read the dependency manifests: `package.json`, `requirements.txt`, `pyproject.toml`, `Gemfile`, `go.mod`, `Cargo.toml`, `composer.json`, `pubspec.yaml`, `Podfile`, `build.gradle`. Also read `.env.example`, deployment configs, infrastructure code (Terraform, Pulumi, CDK), and `<script>` tags in HTML templates.

A dependency is a data flow only if the code calls it with personal data. Search for imports and calls before you list a service.

| Pattern (examples) | Category | What to disclose |
|---|---|---|
| `stripe`, `@stripe/*`, `paddle`, `lemonsqueezy`, `braintree`, `@paypal/*` | Payments | Payment processor. Card data goes to the processor, not to the business, unless the code stores it. |
| `@sentry/*`, `bugsnag`, `rollbar`, `@datadog/*`, `newrelic`, `logrocket` | Error tracking, monitoring | Device and usage data, and any personal data in error payloads. |
| `posthog-*`, `mixpanel*`, `amplitude*`, `@segment/*`, `heap`, `gtag`, `react-ga4`, `@vercel/analytics`, `plausible`, `fathom`, `umami` | Analytics | Usage and device data. Note whether it sets cookies. |
| `hotjar`, `@fullstory/*`, `clarity`, `logrocket`, PostHog session recording | Session replay | Recording of user interactions. Usually needs consent in the EU and UK. |
| Meta Pixel (`fbq`), TikTok Pixel, LinkedIn Insight, Google Ads (`gtag` with `AW-`), `@google/ads*` | Advertising | Data sent to an ad network. In California this can be "sharing", and in some laws a "sale". |
| `firebase*`, `@supabase/*`, `@clerk/*`, `next-auth`, `@auth0/*`, `passport-*`, `@aws-amplify/*`, `better-auth`, `lucia` | Authentication, database | Account data. List each OAuth provider used (Google, GitHub, Apple). |
| `@aws-sdk/*`, `@google-cloud/*`, `@azure/*`, `vercel`, `netlify`, `fly`, `render`, `railway`, `cloudflare` | Hosting, storage, CDN | Hosting provider and region. The region decides international transfer disclosures. |
| `resend`, `nodemailer`, `@sendgrid/*`, `postmark`, `mailgun`, `@aws-sdk/client-ses`, `mailchimp*`, `@loops/*`, `customerio` | Email | Email address. Separate transactional email from marketing email. |
| `twilio`, `@vonage/*`, `messagebird` | SMS, voice | Phone number and message content. |
| `intercom*`, `crisp*`, `@zendesk/*`, `helpscout`, `tawk` | Support, chat | Support messages and contact data. |
| `openai`, `@anthropic-ai/*`, `@google/genai`, `@google/generative-ai`, `replicate`, `cohere*`, `@mistralai/*`, `groq-sdk`, `@huggingface/*`, `ai` (Vercel AI SDK) | AI providers | Content sent to the provider. Check the provider settings and contract for training use and retention. |
| `@googlemaps/*`, `mapbox*`, `ipinfo`, `maxmind`, `geoip*` | Location | Location data. Note whether it is precise (GPS) or approximate (IP-based). |
| `recaptcha`, `hcaptcha`, `turnstile` | Bot protection | Device and interaction data sent to the provider. |
| `uploadthing`, `cloudinary`, `@uploadcare/*`, S3 presigned uploads | File uploads | User content storage. |

For each service, find the provider's current privacy policy URL from the provider's own site. Do not guess the URL.

## 2. Personal data fields

Read the data model: database schemas and migrations (Prisma, Drizzle, SQL, Django or Rails models, Mongoose), API request validators (Zod, Pydantic), and form components.

- List each field that is personal data: name, email, phone, address, date of birth, IP address, device ID, location, payment details, government ID, photos, voice, free-text content.
- Flag sensitive fields: health, biometric, precise location, government ID, financial account, children's data, racial or ethnic origin, religion, sexual orientation, union membership.
- Find what the server logs: request logs with IP addresses, user agents, and user IDs.

## 3. Cookies and local storage

- Search for `document.cookie`, `cookies()`, `setCookie`, `res.cookie`, `localStorage`, `sessionStorage`, and cookie libraries.
- Classify each cookie: strictly necessary (session, authentication, CSRF, load balancing, consent record), functional (preferences), analytics, or advertising.
- Check for a consent tool (OneTrust, Cookiebot, Usercentrics, Klaro, a custom banner) and whether non-essential scripts wait for consent.
- Check whether the site responds to Global Privacy Control (`Sec-GPC` header or `navigator.globalPrivacyControl`).

## 4. Product features that change the terms

- Accounts, teams, or workspaces (who owns the account: the person or the organization?).
- Subscriptions, free trials, automatic renewal, cancellation flow, refunds.
- User-generated content that other users or the public can see.
- Public API or developer keys.
- AI-generated output shown to users.
- Account deletion flow, and what it deletes (database rows, files, backups, third-party copies).

## 5. Report the findings

Give the user a table: finding, category, evidence (path), and the document section it affects. List the questions that the scan could not answer. List any contradiction between the code and an existing legal document.
