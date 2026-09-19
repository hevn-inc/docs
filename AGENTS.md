# Documentation project instructions

## About this project

- This is a documentation site built on [Mintlify](https://mintlify.com)
- Pages are MDX files with YAML frontmatter
- Configuration lives in `docs.json`
- Use the Mintlify MCP server, `https://mcp.mintlify.com`, to edit content and settings via MCP
- Use the Mintlify docs MCP server, `https://www.mintlify.com/docs/mcp`, to query information about using Mintlify via MCP

The site has four tabs, and each one has a different reader:

| Tab | Reader | What it answers |
| --- | --- | --- |
| **General** | anyone evaluating HEVN | How the platform is built, and why HEVN cannot touch the money |
| **Whitelabel** | a backend engineer integrating HEVN | How to create accounts for your own customers and move their money from code |
| **CLI** | an operator or an AI agent | How to run HEVN workflows from a terminal |
| **REST API** | the same engineer, mid-integration | The exact contract for every operation |

## Terminology

These are product nouns. Introduce each one once, then keep using it. Never
substitute a synonym for variety — in payments documentation a second name for
one thing reads as a second thing.

### The money-in chain

Three levels, three names. Do not collapse them.

| Term | What it is | Never call it |
| --- | --- | --- |
| **rail** | A bank route a client can request, identified by an opaque id from `GET /dapi/v1/banks` | "channel", "corridor", "payment method" (a method is one property of a rail) |
| **virtual account** | The opened rail — a `bnk_…` record in the client's own legal name | "bank account" (that is the beneficiary's, at the other end), "requisites" |
| **account details** | What the payer needs in order to wire money: IBAN, account and routing number, PIX key | "requisites", "credentials", "banking info" |

`requisites` is the **field name on the wire** and stays that way. In prose,
write "account details" and introduce the field explicitly the first time a page
needs it: *the account details — `requisites` in the response*.

### Accounts

| Term | What it is |
| --- | --- |
| **integrator account** | The reader's own HEVN account, the one that creates and controls others |
| **client** | An account the integrator creates, one per customer. Also the `cl_…` id and the `X-Hevn-Account` header value |
| **your customer** | The business or person behind a client, in the reader's own product |
| **developer key** | The P-256 keypair the integrator's backend holds |

Do not write "end user", "sub-account", "merchant" or "tenant".

### Money movement

| Term | What it is | Never call it |
| --- | --- | --- |
| **payin** | An arrival of money, quoted or not. The `pi_…` resource | "deposit" when the `pi_…` resource is meant; "pay-in" |
| **payout** | Every departure of money, to a bank account or to a wallet. The `po_…` resource | "withdrawal", "transfer", "send" |
| **contact** | A saved destination, `ct_…`. The API object | "recipient", "payee" |
| **beneficiary** | The party at the far end of a fiat payout — a role, not an object | "recipient" |
| **quote** | The priced terms of one payin or payout | "estimate", "rate" |
| **approval** | The bytes HEVN builds for the integrator to sign | "transaction", "payload" on its own |
| **settlement token** | The stablecoin a rail delivers — USDC or EURC | "coin", "asset" |

"Recipient" is not used anywhere. It is ambiguous between the contact, the
beneficiary and the client.

### Other fixed choices

- **non-custodial**, not "self-custody" or "trustless"
- **Base smart wallet**, not "wallet contract" or "account abstraction wallet"
- **verification** for the process, **KYB** only where the API says `kyb`
- **partner bank** / **licensed banking partner** — see Content boundaries

## Style preferences

The target voice is the `whitelabel/` tab: dense, plain, and confident that the
reader is an engineer. Every other tab is brought to it, not the reverse.

### Structure

- The first one or two sentences of a page say what the thing is and what it is
  for. No "Introduction" heading, no "Overview" heading at the top of a page.
- Sentence case for every heading. Never Title Case.
- End a guide page with a single `<Card title="Next: …">` pointing at the next
  page in the reading order.
- Prefer a table to a bullet list whenever the bullets share a shape — but keep
  bullets when each one is a different thing rather than a different value of
  the same thing.
- One `<CodeGroup>` per operation, tabs in the order cURL, Python, Node, Go.

### Sentences

- Active voice, second person. The reader is "you"; HEVN is "HEVN".
- One idea per sentence. If a sentence needs two em dashes, it is two sentences.
- State the rule, then the consequence. Do not build up to it.
- Name the error code and the status together: `422 validation_failed`.
- Bold for UI elements (Click **Settings**); code formatting for file names,
  commands, paths, fields and endpoints.

### Things not to write

- **Feature lists without a task.** No "What you can do", no "Key features".
  Say what the reader is trying to achieve, then how. This bans enumerating API
  capabilities — it does **not** ban bullets. A short list of distinct things the
  reader could build or decide between, each with a bolded lead-in, is the right
  shape for a first screen, and turning one into a paragraph makes it worse.
- **"Best practices" sections.** Put the practice next to the thing it applies to.
- **Emoji in headings or bullets.**
- **Marketing superlatives.** "seamless", "powerful", "simply", "just", "easily".
- **Aphorisms the reader has to decode.** The voice is compressed, not cryptic:
  if a sentence needs a second reading to parse, split it.
- **Hardcoded counts** ("45 operations", "37 paths") — they go stale silently.

## Content boundaries

- **Never name a banking partner.** Always "a licensed banking partner" or "the
  partner bank". This is deliberate company policy.
- **No fee rates, no spreads, no minimums as numbers.** Document the *structure*
  of a fee and how to read it out of a quote; point at sales for the rates.
- **No internal admin features**, no partner-side tooling, no emulator internals
  beyond what the sandbox pages already state.
- **Document what the deployed API does**, not what is planned, unless the page
  says plainly that it is a roadmap item.
