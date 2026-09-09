# AdvicePay

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

AdvicePay is a fee-for-service billing, payment processing and engagement-management platform for
financial advisors, RIAs and broker-dealers. Founded by Michael Kitces and Alan Moore, it lets firms
invoice clients for financial planning advice, collect card and ACH payments, run recurring
subscriptions, capture eSignatures, and track engagement workflows, deliverables and compliance
oversight.

- Website: https://advicepay.com/
- API reference: https://docs.advicepay.com/
- Status: https://status.advicepay.com/
- Security and compliance: https://advicepay.com/security/

## The API

AdvicePay publishes a public REST API (v1.0.1) at `https://app.advicepay.com/api/public/v1`, with a
test environment at `https://demo.advicepay.com` that does not touch live data or banking networks.
The reference documents 55 operations across 13 resources: admins, advisors, agreements, clients,
custom attributes, deliverables, engagements, invoices, notifications, offices, subscriptions and
transfers.

Authentication is OAuth 2.0 (authorization code and client credentials) with three client
authentication methods — `client_secret_post`, `client_secret_jwt` (HS256) and `private_key_jwt`
(RS256) — plus SAML 2.0 single sign-on. Access tokens live 5 minutes; refresh tokens are single-use
and rotate on every use.

**API access is an Enterprise-plan capability.** The documentation is fully public and needs no
login, but calling the API requires an Enterprise contract.

## What this profile found

- **No machine-readable contract.** The reference is a server-rendered Slate page that looks
  widdershins-generated, so an OpenAPI almost certainly exists internally — but none is served.
  Probed 14 spec paths across four hosts; all miss. See `x-contract-discovery` in `apis.yml`.
  No spec has been generated from the docs, because an authored contract would be a fabrication.
- **No idempotency, on an API that moves money.** Nothing in the documentation describes an
  idempotency key, a de-duplication window or a safe-retry contract, including on invoice creation,
  subscription creation and refunds.
- **No webhooks.** Change notification is polling-only, via the notifications endpoint with
  `createdAfter` / `createdBefore` filters.
- **Refunds are the only reversal.** Subscriptions can be created through the API but not
  cancelled through it, and no window is published for how long a refund remains possible.
- **Good rate-limit hygiene.** Published ceilings (10 req/sec, 1 req/sec on agreement download),
  `X-RateLimit-*` headers on success and `Retry-After` on 429.
- **A well-run deprecation in flight.** The `canceled` invoice status is being replaced by
  `voided` on 2026-12-02, with dual-accept during the transition and a `useVoidedStatus` opt-in
  parameter for testing ahead of the cutover — announced in prose only, with no RFC 8594 headers.
- **No SDKs.** Nothing on npm, PyPI, RubyGems, NuGet, crates.io or Packagist, and no first-party
  GitHub organization. The docs' seven-language snippets are generated raw-HTTP samples.
- **No `/.well-known/` surface.** All 15 named paths 404 on all five hosts.
- **Real compliance posture.** SOC 2 Type II (KirkpatrickPrice, annual), PCI SAQ A behind Stripe,
  annual third-party penetration tests.

## Artifacts in this repository

| Artifact | File |
|---|---|
| Authentication profile | `authentication/advicepay-authentication.yml` |
| OAuth scopes | `scopes/advicepay-scopes.yml` |
| Error catalog | `errors/advicepay-problem-types.yml` |
| Rate limits | `rate-limits/advicepay-rate-limits.yml` |
| Conventions, idempotency, reversibility | `conventions/advicepay-conventions.yml` |
| Lifecycle and deprecation | `lifecycle/advicepay-lifecycle.yml` |
| Conformance and domain standards | `conformance/advicepay-conformance.yml` |
| Data model | `data-model/advicepay-data-model.yml` |
| Sandbox and test environment | `sandbox/advicepay-sandbox.yml` |
| Plans and pricing | `plans/advicepay-plans-pricing.yml` |
| Changelog | `changelog/advicepay-changelog.yml` |
| Packages | `packages/advicepay-packages.yml` |
| Well-known probe | `well-known/advicepay-well-known.yml` |
| Candidate MCP tool list | `mcp/advicepay-mcp.yml` |
| Domain security | `security/advicepay-domain-security.yml` |
| Vulnerability disclosure | `security/advicepay-vulnerability-disclosure.yml` |
| Trust center | `security/advicepay-trust-center.yml` |
| llms.txt | `llms/advicepay-llms.txt` |

The MCP tool list is a **candidate** derived by API Evangelist from AdvicePay's documented
operations. AdvicePay ships no MCP server; nothing can call it.
