# Documentation project instructions

## About this project

- Use the Mintlify MCP server, `https://mcp.mintlify.com`, to edit content and settings via MCP
- Use the Mintlify docs MCP server, `https://www.mintlify.com/docs/mcp`, to query information about using Mintlify via MCP

Each tab has a different reader:

| Tab | Reader | What it answers |
| --- | --- | --- |
| General | anyone evaluating HEVN | How the platform is built, who can sign for the wallet, and which developer section to read. Remote transfers is a page here, for an engineer automating their own account with a capped key |
| For developers | a backend engineer starting an integration | How to get API access, log in, sign a payment, verify a client, and what is common to both integration kinds. The first page is the fork between them |
| HEVN Connect | the same engineer, on the recommended integration | How to link customers' HEVN accounts through the sign-in window and get their payouts approved there |
| External wallets | an engineer at an integrator with its own wallet infrastructure | How to create clients through the API and deliver their money to wallets they hold, with no HEVN window |
| Sub Accounts | an engineer running many balances in their own account | How to open sub accounts of the integrator account and move money through them with the developer key |
| REST API | the same engineers, mid-integration | The contract of each operation |

The directories are `general/`, `developers/`, `connect/`, `external-wallets/`, `sub-accounts/` and `api-reference/`. A tab page holds what differs for that tab and links to `developers/` for what is common. Do not copy a common page into a tab.

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
| integrator account | The reader's HEVN account. It links or creates clients and acts for them. Its own `cl_…` id is the `userId` that `POST /auth/token` returns |
| client | A customer's account the integrator acts for, one per customer. Linked through HEVN Connect or created with `POST /clients`. Also the `cl_…` id and the `X-Hevn-Account` header value |
| your customer | The business or person behind a client, in the reader's product |
| developer key | The P-256 keypair the integrator's backend holds |
| HEVN Connect | The integration kind in which the customer links a HEVN account in the sign-in window and approves payouts there. The window itself is the "sign-in window", and its feature name is "Sign in with HEVN" |
| external account | The API object for a wallet address a client holds and signs for: an `acc_…` id with `kind: "external"`. In prose about the product, "external wallet" names the wallet and the integration kind ("External wallets" is the tab title). Write "external account" when you mean the object or its id |
| sub account | A further wallet of the integrator's own account on Base that HEVN opens and the developer key signs for: an `acc_…` id with `kind: "sub"`. Two words in prose |
| main wallet | The Base smart wallet of an account, as opposed to its sub accounts. Use it where a page has to tell the two apart |

Do not write "end user", "sub-account", "subaccount", "merchant" or "tenant". Do not call an external account an "external wallet account" or a "linked wallet".

"Account" has three uses, and each needs its qualifier on first use in a page:

- A HEVN account, integrator account or client: a profile, named by a `cl_…` id.
- An account key: one of `USDC`, `EURC`, `RD`, `USDC_ARB`, `FDIC_USD`, `FDIC_EUR`. A balance of an account, named by its key in `sourceAccount`, `destinationAccount` and each balance row.
- An `acc_…` id: an external account or a sub account. It goes in the same `sourceAccount` and `destinationAccount` fields as an account key, and in `accountId` on virtual accounts, balances, payins and transactions. A balance on the main wallet has an account key and no `acc_…` id.

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
| account | An account key (`USDC`, `EURC`, `RD`, `USDC_ARB`, `FDIC_USD`, `FDIC_EUR`) or an `acc_…` id. The value of `sourceAccount`, `destinationAccount` and each balance row. See Accounts above | "wallet" for an account key. Do not call `FDIC_USD` or `FDIC_EUR` a token |
| swap | A move between two accounts of one integrator account. The `swp_…` resource | "conversion", "exchange", "transfer" |
| move | A swap with a sub account on either side: one token, one for one, no fee. Same `swp_…` resource and routes | "internal transfer", "sweep" |

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
| The two integration kinds and how to choose | `developers/overview` (the fork). `general/build-on-hevn` is the short version for the General tab |
| Product, rail, virtual account, account details | `developers/virtual-accounts`, tables in `developers/reference/rails` |
| Account keys (`USDC`, `EURC`, `RD`, `USDC_ARB`, `FDIC_USD`, `FDIC_EUR`) | `developers/overview` (Account keys) |
| `acc_…` ids, external account and sub account compared | `developers/accounts` |
| Developer key, IP allowlist, login, tokens, `X-Hevn-Account` | `developers/authentication` |
| Who signs what, the `authorization` values | `developers/overview` (Accounts and control), values in `developers/signing` (Authorization values). `developers/payouts` (Authorization) routes each paying account to its page |
| Prepare, sign, confirm | `developers/signing`, code in `developers/reference/client` |
| What a `debit` contains, the checks before signing | `developers/signing` (Debit contents) |
| Money format, ids, idempotency, pagination, polling | `developers/conventions` |
| Error envelope and codes | `developers/reference/errors` |
| Numeric limits | `developers/conventions` (Rate limits, Lifetimes, Setting limits) |
| Fees and rates | `developers/reference/rails` (Fees) |
| Sandbox differences and limits | `developers/sandbox` |
| Sign-in window: sessions, link, sign, verify, callback | `connect/sign-in-with-hevn` |
| Payouts the holder approves (`authorization: "client"`) | `connect/money-movement` |
| Creating a client through the API, access to external wallets | `external-wallets/overview` |
| External account resource: address rules, balance, archive | `external-wallets/external-accounts` |
| Payouts funded by a wallet transfer (`authorization: "wallet"`), `refundAddress` | `external-wallets/money-movement` |
| Sub account rules: who can open one, token, label, limits | `sub-accounts/overview` |
| Payouts, moves, payins and virtual accounts of a sub account | `sub-accounts/money-movement` |
| Non-custodial wallet model | `general/what-is-hevn` (Account structure), `general/security-model` (Smart wallet) |
| Self-serve payins, fiat payouts, cross-chain, JWT and signature authorization | `general/how-money-moves` |
| Spend permissions | `general/security-model` (Shared access) |
| KYB document fields, enums and slots | Backend schema descriptions (REST API tab). Flow, missing paths and complex ownership: `developers/clients` |
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

The `FDIC_*` sentence appears once, on `developers/overview`:

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
- Privy, Coinbase Smart Wallet and Base stay named. Attribute key-handling claims to Privy. Do not name the cross-chain swap route: write "routed" or "the route".
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
- When you delete or move a page, add a redirect in `docs.json` and fix the inbound links in the same change.
