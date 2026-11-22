<h1 align="center">Veiled</h1>

<p align="center">
  A private money account on Solana, for you and for the AI agents you let spend on your behalf.
</p>

<p align="center">
  <a href="https://useveiled.com">Website</a> ·
  <a href="https://docs.useveiled.com">Docs</a> ·
  <a href="https://x.com/useveiled">X</a>
</p>

---

Hi, we're Veiled.

Most money is either private or verifiable, and rarely both. Your bank keeps your history away from the public, but it sees every line of it. A public blockchain cuts out the middleman, then shows your balance and every payment to anyone with a block explorer.

We think you shouldn't have to pick one. Payments on Veiled settle on Solana, so anyone can check that a transfer happened. The amount, though, is encrypted. Only the sender and the recipient can read it.

You keep your own keys. Balances are held in USDG or USDC, and you can send to a name like `lena.veiled` instead of pasting an address.

### Agents with a budget

More and more software spends money on its own: paying for API calls, compute, data, other agents. We give each agent its own account with rules that the chain enforces, not a server that could be talked out of them.

- a cap on each payment
- a cap on spend over a rolling 24 hours
- an optional list of who it's allowed to pay
- a threshold above which you approve the payment yourself

If an agent misbehaves, pause it. If you're done with it, revoke it and whatever is left goes back to you. Agents also speak [x402](https://docs.useveiled.com/autonomous-agents/paying-per-request), so they can pay for an HTTP request when a service answers with `402 Payment Required`.

### How the privacy works

We didn't write our own cryptography. Solana's Token-2022 program has a confidential transfer extension that stores balances and amounts as ciphertext, and the chain's own zero knowledge proof program checks every transfer without learning its value. Our program sits on top and adds the parts an encrypted balance needs to act like a real account: names, agent accounts, spending policies, an approval queue, payment requests and disclosure receipts.

To be upfront about the limits: wallet addresses on both sides of a payment are still public. What we hide is how much.

### What's here

| Repository | What it is |
| --- | --- |
| [contracts](https://github.com/useveiled/contracts) | `veiled`, our Solana program, written in Rust with Anchor |
| [webapp](https://github.com/useveiled/webapp) | The web app and the site at useveiled.com |
| [sdk](https://github.com/useveiled/sdk) | The Official TypeScript SDK for Veiled |
| [documentation](https://github.com/useveiled/documentation) | The source behind docs.useveiled.com |

If you want to try it, the SDK is the quickest way in:

```bash
npm install @useveiled/sdk
```

```ts
import { Veiled } from "@useveiled/sdk";

const veiled = new Veiled({ apiKey: process.env.VEILED_API_KEY! });

await veiled.transfers.create({
  to: "@harbor-coffee",
  amount: "42.75",
  asset: "USDG",
  memo: "Roastery order 1182",
});
```
