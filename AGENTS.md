# Documentation project instructions

## About this project

- Use the Mintlify MCP server, `https://mcp.mintlify.com`, to edit content and settings via MCP
- Use the Mintlify docs MCP server, `https://www.mintlify.com/docs/mcp`, to query information about using Mintlify via MCP

Each tab has a different reader:

| Tab | Reader | What it answers |
| --- | --- | --- |
| General | anyone evaluating HEVN | How the platform is built and who can sign for the wallet |
| Remote Transfers | an engineer automating their own account | How to let a server or script send USDC from that account with a capped key. For advanced use |
| Whitelabel | a backend engineer integrating HEVN | How to create accounts for your customers and move their money from code |
| REST API | the same engineer, mid-integration | The contract of each operation |

## Terminology

These are product nouns. Introduce each one once, then keep using it. Do not
substitute a synonym, because a reader takes a second name for a second object.

### The money-in chain

Each level has one name. Do not merge them.

| Term | What it is | Never call it |
| --- | --- | --- |
| product | A region a client can request a virtual account in, for example `eu`, `us`, `uk` or `swift`. The key of `POST /dapi/v1/banks` and `/banks/{product}` | "region code", "market", "rail" |
| rail | The bank route HEVN picks for a product. Returned as `rail`. Treat the value as opaque. It is not a request key | "channel", "corridor", "payment method" (a method is one property of a rail) |
| virtual account | The `bnk_…` record. On a named rail the partner bank opens it in the client's legal name | "bank account" (that is the beneficiary's, at the other end), "requisites" |
| account details | What the payer needs in order to wire money: IBAN, account and routing number, PIX key | "requisites", "credentials", "banking info" |

`requisites` is the field name in the API and does not change. In prose write
"account details". The first time a page needs the field, write "the account
details, returned as `requisites`".

### Accounts

| Term | What it is |
| --- | --- |
| integrator account | The reader's HEVN account. It creates and controls clients |
| client | An account the integrator creates, one per customer. Also the `cl_…` id and the `X-Hevn-Account` header value |
| your customer | The business or person behind a client, in the reader's product |
| developer key | The P-256 keypair the integrator's backend holds |

Do not write "end user", "sub-account", "merchant" or "tenant".

### Money movement

| Term | What it is | Never call it |
| --- | --- | --- |
| payin | An arrival of money, quoted or not. The `pi_…` resource | "deposit" for the `pi_…` resource, "pay-in" |
| payout | A departure of money to a bank account or to a wallet. The `po_…` resource | "withdrawal", "transfer", "send" |
| contact | A saved destination, `ct_…`. The API object | "recipient", "payee" |
| beneficiary | The party that receives a fiat payout. A role with no API object | "recipient" |
| quote | The priced terms of one payin or payout | "estimate", "rate" |
| approval | The bytes HEVN builds for the integrator to sign | "transaction", "payload" on its own |
| settlement token | The stablecoin a rail delivers: USDC or EURC | "coin", "asset" |
| account | One of `USDC`, `EURC`, `FDIC_USD`, `FDIC_EUR`. The value of `sourceAccount`, `destinationAccount` and each balance row | "wallet". Do not call `FDIC_USD` or `FDIC_EUR` a token |
| swap | A move between two accounts the same client owns. The `swp_…` resource | "conversion", "exchange", "transfer" |

"Recipient" is not used anywhere. It is ambiguous between the contact, the
beneficiary and the client.

### Other fixed choices

- "non-custodial", not "self-custody" or "trustless"
- "Base smart wallet", not "wallet contract" or "account abstraction wallet"
- "verification" for the process, "KYB" where the API says `kyb`
- "partner bank" or "licensed banking partner" (see Fintech vocabulary)

## Page shape

- Length: a concept page is 300 to 700 words, a guide 600 to 1,100, code excluded.
- Opening: two sentences. The first defines the object or names the outcome. The second says what the reader does first, with the endpoint. Do not open with a greeting, a benefit claim or an "Overview" heading.
- Paragraphs: 1 to 3 sentences of 20 words at most. In a guide, prose is at most 250 words and 35% of the page.
- Headings: 1 to 4 words, sentence case, a noun phrase or an imperative ("Statuses", "Request a virtual account"). No colons, counts or questions.
- Guide step: one imperative line with the endpoint, one `<CodeGroup>`, a truncated response, and a link to the operation's REST API page for the fields.
- `<CodeGroup>` tabs go in the order cURL, Python, Node, Go.
- Tables hold statuses, limits, time windows, error codes and rails. A status table has "Meaning" and "Next" columns.
- Callouts: `<Note>` and `<Warning>`, two per page at most. `<Warning>` marks a way to lose money or pay twice. A callout does not repeat the body.
- Ending: the last table, or one `<Card title="Next: …">` with no body text. No recap.
- Name the status and the error code together: `422 validation_failed`.
- Bold is for UI labels (Click **Settings**), three spans per page at most. Code formatting is for files, commands, paths, fields and endpoints.
- Frontmatter `description`: one sentence of 20 words at most.

## Topic owners

Define a term or a rule on one page. On other pages write one clause and a link. The owners:

| Topic | Owner |
| --- | --- |
| Product, rail, virtual account, account details | `whitelabel/virtual-accounts`, tables in `whitelabel/reference/rails` |
| Accounts (`USDC`, `EURC`, `FDIC_USD`, `FDIC_EUR`) | `whitelabel/overview` (How money moves) |
| Developer key, IP allowlist, login, tokens, `X-Hevn-Account` | `whitelabel/authentication` |
| Who signs what | `whitelabel/overview` (Accounts and control) |
| Prepare, sign, confirm | `whitelabel/signing`, code in `whitelabel/reference/client` |
| Money format, ids, idempotency, pagination, polling | `whitelabel/conventions` |
| Error envelope and codes | `whitelabel/reference/errors` |
| Numeric limits | `whitelabel/conventions` (Rate limits, Lifetimes, Setting limits) |
| Fees and rates | `whitelabel/reference/rails` (Fees) |
| Sandbox differences and limits | `whitelabel/sandbox` |
| Non-custodial wallet model | `general/what-is-hevn` (Account structure), `general/security-model` (Smart wallet) |
| Self-serve payins, fiat payouts, cross-chain, JWT and signature authorization | `general/how-money-moves` |
| Spend permissions | `general/security-model` (Shared access) |
| KYB document fields, enums and slots | Backend schema descriptions (REST API tab). Flow, missing paths and complex ownership: `whitelabel/clients` |
| Fields of a request or response | Backend schema descriptions (REST API tab) |
| Statuses | The page of the resource |

## What to leave out

- Design rationale and "Why this is safe" sections. One clause of reasoning per concept at most.
- Explanations of third-party technology and "References" lists. One sentence and one link replace them.
- Card grids and diagrams that repeat the body or another page.
- Sentences that restate the description, the heading or the previous sentence.
- Scene-setting and story-style step titles. Example company names may appear as JSON values.
- API history, roadmap items and internals: service layout, cloud vendors, admin tooling, emulator internals.
- Feature lists with no task, "Best practices" sections, emoji.
- Hardcoded counts ("four statuses", "45 operations"). They go stale.

Do not cut a status, error code, limit or condition. Move it into a table, or into the backend schema description when it is a rule about one field.

## Banned patterns

| Pattern | Bad | Write |
| --- | --- | --- |
| Counted slogan | "Two calls, two signatures, no browser." | "Log in with `POST /auth/challenge` and `POST /auth/token`." |
| Aphoristic closer | "A rotation you have never run is not a control." | "Test rotation in the sandbox." |
| "Not X, it is Y" | "The redirect is a hint, never the record." | "Do not act on the redirect. Read `GET /dapi/v1/payouts/{payoutId}`." |
| Negative triplet | "It reserves nothing, signs nothing and returns no `swapId`" | "Preview has no side effects." |
| Absolute promise | "A payout can never be sent twice." | "Each operation has a unique nonce, so a retried payout is rejected." |
| Personified object | "the record tells you what it still waits for" | "The call returns `201`. `blockers[]` lists what is pending." |
| Meta-commentary | "Two consequences worth knowing before your first `422`:" | Delete. |
| Self-justification | "The write is idempotent by construction." | "`PUT` is safe to repeat." |
| Metaphor | "The email is the crown jewel" | "Anyone who controls the email can unlock the wallet." |
| Slogan heading | "Two tokens, two jobs" | "Tokens" |

Limits per page, outside code and outside the "Bad" column above:

- Em dashes: none. Use a period, a comma or parentheses. Write "n/a" in an empty table cell.
- Semicolons in prose: none. A line ends in a colon only above a list, table or code block.
- "never", "always", "only", "every", "nothing": keep one only where it states an API contract.
- A route "returns". Objects do not "answer", "carry", "travel", "live", "own" or "decide".

Check a page before you hand it off. Each hit needs an API-contract reason.

```bash
grep -nE '—|–|[a-z]; |\*\*[^*]+\*\*' page.mdx
grep -niE 'seamless|powerful|simply|easily|just |robust|leverage|journey|under the hood|exactly|strictly|entirely|fully|directly|actually' page.mdx
grep -niE 'worth (knowing|noting|keeping)|keep in mind|note that|this page|what follows|the whole |that is (the|what|why)|the one thing|by (design|construction)|deliberate|honest' page.mdx
grep -niE ', not (a|an|the) |, never |rather than|no [a-z]+, no [a-z]+|^#* ?(One|Two|Three|Four|Five) ' page.mdx
grep -niE 'crown jewel|honeypot|blast radius|rogue|hatch|in disguise|kills|dies|answers|travels|northwind\b' page.mdx
grep -niE 'financial platform|neobank|licen[cs]|regulat|insured|safeguard|custod|omnibus|pooled|guarantee|instant|secure|safe |provider|offramp|some partners|statement|deposit' page.mdx
```

## Fintech vocabulary

The positioning statement appears once, on `general/what-is-hevn`:

> HEVN is payments software: an API and app over a Base smart wallet that only its holder's key can sign for. HEVN is not a bank. Virtual accounts, fiat payins and fiat payouts are provided by partner banks, which hold fiat while it is in transit.

The `FDIC_*` sentence appears once, on `whitelabel/overview`:

> `FDIC_USD` and `FDIC_EUR` are account identifiers for a balance held at a partner bank. The name is an identifier and says nothing about deposit insurance.

| Topic | Write | Do not write |
| --- | --- | --- |
| HEVN | "payments software", "platform", "API" | "bank", "neobank", "banking", "financial platform" |
| What the client gets | "virtual account opened by a partner bank", "account details" | "bank account" for anything HEVN gives, "looks like a bank account" |
| Licence | Nothing about HEVN: no licence, registration or regulated status, and no disclaimer beyond the positioning statement. "partner bank" by default, "licensed banking partner" once per page at most | "licensed", "regulated", "authorised", "supervised", "compliant" about HEVN. "under its own licensing" about a partner |
| Insurance | The `FDIC_*` sentence above | "insured", "protected", "safeguarded", any expansion of "FDIC" |
| Custody | "HEVN holds no key to the wallet", "only the holder's key can sign", "the wallet is non-custodial", "balance held at the partner bank" | "non-custodial platform", "self-custodial", "trustless", "custodial balance", "never holds the money", "no pooled balance", "no omnibus account" |
| Guarantees | What a key can and cannot sign. "safe to repeat" for an idempotent call. "Timing is set by the partner bank." | "cannot freeze", "nothing to lose", "safe", "secure", "guarantee", "instant", "a few hours", "same day" |
| Partners | "the partner bank", singular, as a role. Do not name a financial provider: bank, payment institution, card issuer, KYC vendor or stablecoin issuer | "some partners", "a partner", "provider", "verification provider", "offramp" |
| Words | "payin", "payout", "incoming payment", "transaction report" | "deposit" outside field names, "statement" outside `kind=statement`, "cards" with no "issued by a partner" |
| Compliance | States and what the client must submit: "under review", "rejected" | "HEVN compliance", "our compliance team" |

- Do not make a financial provider identifiable. Examples use `"bankName": "Example Bank"`, `BANKDEFFXXX` and `DE89370400440532013000`, with no real bank name, real BIC or partner-country IBAN. Treat `rail` values as opaque. Do not describe how a partner operates.
- Privy, Coinbase Smart Wallet, Base and NEAR Intents (1Click) stay named. Attribute key-handling claims to Privy.
- Scope "non-custodial" to the wallet. The partner bank holds fiat in transit, pooled accounts and `FDIC_*` balances.
- "In the client's legal name" needs "on a named rail" and "the partner bank opens it".
- First use of "HEVN account" in a tab: "a profile plus a Base smart wallet".
- No fee rate, spread or minimum as a HEVN price. Document the structure of a fee and how to read it from a quote. For a unit example write "`100` is 1%", once.

## API facts and code

- `openapi.json` and the deployed API are the source of truth. Do not invent a field, status or error code. Document what the deployed API does.
- Field-level rules live in the backend schema descriptions (`app/schemas/partner`, `Field(description=...)`) and reach the REST API tab through `openapi.json`. A guide links to the operation's REST API page instead of repeating its field table. Keep flows, cross-resource rules, status lifecycles and what a reader must know before calling.
- Do not change endpoints, methods, headers, field names, enum values, error codes with their HTTP status, id prefixes or contract limits to fit the prose.
- Code samples keep their behaviour, request bodies and tab order (cURL, Python, Node, Go). Allowed edits: a variable rename (use `client` for a variable that holds a client), neutral example values, comments.
- Keep frontmatter keys, `title`, Mintlify components and their props, file paths and link targets. Remove an MDX `import` line only with its last use.
- A heading that another page links to keeps its text. When you rename one, fix the inbound links in the same change.
- When you delete a page, add a redirect in `docs.json`.
