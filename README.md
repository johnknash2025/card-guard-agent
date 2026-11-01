# Card Guard Agent (design)

English | [日本語](README.ja.md)

> **Design stage: no code in this repository yet.** This page describes the idea so engineers can comment on it before it is built.

**One line:** a local agent that keeps track of which payment card is registered with which service, so that when a card leaks, expires or is replaced you know exactly what to change, and in what order.

## The problem
Most people cannot answer "which services charge this card?" A card gets registered once and forgotten: with subscriptions, shops, app stores, games, utilities. When the card leaks or is reissued, people find out what was attached to it only when payments start failing, or when a fraudulent charge appears. Rarely used cards keep high limits and no usage alerts, which is exactly what makes them attractive to misuse.

## What it does
1. **Card map.** For each card, the list of services it is registered with. A card is identified only by a label you choose and its last four digits; the full number, expiry date and security code are never stored.
2. **Limits and alerts check.** Flags rarely used cards that still have a high limit, and cards without usage notifications turned on, and suggests what to change in the card issuer's own settings. The agent never changes those settings itself.
3. **Leak or reissue playbook.** When a card leaks or is replaced, it lists every service registered with that card, ordered by risk (recurring payments and stored-card shops first), with a checklist to update or remove the card at each one, and to confirm with the issuer.
4. **Dormant subscriptions.** Points out services that still charge a card but have not been used for months.

## Principles
- **Never stores card numbers, expiry dates, security codes or passwords.** Only a label and the last four digits.
- **Runs locally.** Your card map stays on your own machine; no cloud account is needed.
- **Advises, never acts.** It does not log in to your bank or card issuer, and it does not change limits or cancel anything. You do that, with its checklist.
- **Honest status.** Every check says what was verified and what was not.

## How it fits with other tools
- **[Account Keeper Agent](https://github.com/johnknash2025/account-keeper-agent)** already keeps track of which services you signed up for and can import service names and URLs from a password-manager export without reading passwords. Card Guard adds the card side: which card each of those services charges. They are kept separate on purpose, so that account data and payment data do not live in one place.
- **[leakwatch-jp](https://github.com/johnknash2025/leakwatch-jp)** checks whether your email or passwords appear in known breaches. Card Guard covers the case where it is a card that leaked.

## Open questions (feedback welcome)
| Question | Why it matters |
|---|---|
| How to build the card map without reading statements in full | Statements contain far more than needed; the agent should take only merchant name, last four digits and amount. |
| Card issuers' settings differ widely | Limit and alert advice has to stay generic, or be written per issuer. |
| Recurring payments through intermediaries (app stores, PayPal) | The real service is hidden behind the intermediary; the map needs to show both. |

*By [Happy AI Garage](https://github.com/johnknash2025). Comments and disagreement are welcome: open an issue.*
