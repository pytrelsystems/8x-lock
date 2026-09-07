# 8X-LOCK Manifesto

Version: 0.2  
Authority: Pytrel Systems / operator-owned agent  
Repo: pytrelsystems/8x-lock  
Status: specification + working keypad. Not a privacy product on hosted chat.

---

## 1. Purpose

8X is a wire language for an operator and an operator-owned agent.

It is not a culture project. It is not a Grok personality mode. It is not "privacy because the words look strange."

The job is:

1. Put English into a bounded, versioned envelope.
2. Optionally lock that envelope with real cryptography.
3. Move only the envelope across channels the operator does not own.
4. Decrypt and think only on a process the operator owns.

If a hosted model must see the plaintext to answer, that channel is not the private language. It is a workshop.

---

## 2. Doctrine

- Obfuscation is not encryption.
- Encryption is not end-to-end privacy if the far side is a vendor runtime.
- The language is the envelope. The agent is the reader.
- Project-specific practice must not become fake canon. 8X1 leaked the moment the first public page shipped. Treat 8X1 as public.
- Secrets never ride in the same log as plaintext.
- LLM suggests. Governor decides. Executor acts. Ledger records. 8X sits in front of that loop as a gate, not inside the model weights.

---

## 3. What exists now

### 8X1 — public scramble

- Grammar: English word order, tense, punctuation.
- Lexicon: algorithmic. No handmade dictionary. Every Latin-letter word encodes.
- Map: keyed alphabet from `pytrel`, then unused letters.
- Transform: map each letter, reverse letter order in the token, case travels with the letter.
- Numbers and punctuation pass through.
- Prefix: `8X1 `.

8X1 is reversible by anyone with this repo or the keypad. Use it for friction and habit, not secrecy.

### 8X2 — payload encryption

- Inner body: an 8X1 string.
- Key: operator passphrase.
- KDF: PBKDF2-SHA-256, 100000 iterations, random 16-byte salt.
- Cipher: AES-GCM-256, random 12-byte IV.
- Pack: `salt || iv || ciphertext`, URL-safe base64.
- Prefix: `8X2 `.

8X2 is real cryptography on the blob. It is not magic. Strength equals passphrase discipline plus where decrypt happens.

### Keypad

Single-page local app. English ↔ envelope. Optional passphrase. Copy / paste / share. Installable from Safari as a Home Screen app.

The keypad is a human tool. It is not the agent.

---

## 4. What 8X is not

- Not a constructed language with unique roots for every English lemma.
- Not quantum anything.
- Not private because the GitHub repo is private.
- Not private because two people "know the dialect."
- Not a substitute for OS keychain, device lock, or agent-side audit logs.
- Not authorized as a way to hide unlawful activity. Same law applies to ciphertext as to English.

---

## 5. Correct runtime

```
phone keypad          owned agent process
   |                         |
English in              decrypt 8X2
   |                         |
8X2 out  ---- channel ----> think
   |                         |
8X2 in  <---- channel ----  encrypt 8X2
   |                         |
English on device       no plaintext log
```

Allowed runtimes: local Dragon, local desktop, operator VPS with a process you control.

Disallowed as the private reader: Grok, ChatGPT, Claude, any hosted chat that must ingest plaintext to reply.

Workshop use of a hosted model to *design* 8X is fine. Workshop use as the *live private peer* is a category error.

---

## 6. Evolution path

Each layer ships only when the previous layer is proven.

### L0 — Envelope (done)

Version prefix. Deterministic 8X1. Working keypad.

### L1 — Lock (done on keypad, not on agent)

8X2 AES-GCM. Passphrase in the keypad only. Refuse to put the passphrase in hosted chat.

### L2 — Agent gate (next)

Owned agent exposes two functions only:

- `unlock(envelope, secret) -> plaintext | FAIL`
- `lock(plaintext, secret) -> envelope`

Fail closed. No plaintext in default logs. Secret from env or OS keychain, never from the model prompt.

### L3 — Session keys

Passphrase unlocks a local keystore. Per-thread AES key. Rotate on demand. Old envelopes still open with archived keys. Envelope header carries `kid`.

### L4 — Identity

Agent and operator each have a keypair. Envelope can add a signature over the ciphertext so the reader knows which peer locked it. Encryption and authorship stay separate fields.

### L5 — Transport

Move envelopes over a channel you pick (local socket, your VPS, your mail). 8X does not become a new network stack. It stays an envelope.

### L6 — Post-quantum option

If the threat model includes long-term stored ciphertext against a future cryptographically relevant quantum computer, add a hybrid KEM (e.g. X25519 + ML-KEM) for session setup. Do not rename 8X2 and pretend it is quantum. Do not mix circuit toys into the gate.

### Deliberately not on the path

- Training a model to "speak 8X natively." That bakes a public codec into weights.
- Security by repo visibility.
- Expanding 8X1 into a 200k-word handmade lexicon.

---

## 7. Versioning

| Prefix | Meaning | Secrecy |
|---|---|---|
| `8X1` | scramble only | none |
| `8X2` | 8X1 + AES-GCM + PBKDF2 | passphrase + decrypt location |
| `8X3` | reserved: 8X2 + key id + signature | L3/L4 |

Readers must fail closed on unknown prefixes.

---

## 8. Operating rules

1. Default envelope for agent traffic: `8X2` or later.
2. Never paste a passphrase into a hosted model.
3. Never log plaintext at info level on the agent.
4. If decrypt fails, stop. Do not "try to read it as 8X1."
5. Keypad updates require replacing the Home Screen copy. Old icons keep old code.
6. Public history of this repo means 8X1 is public forever.
7. Private repo is hygiene. It is not the secret.

---

## 9. Acceptance for "we speak 8X"

The language is in force with the agent when all of these are true:

- Agent process is operator-owned.
- Secret is not in any hosted chat log.
- Agent rejects non-envelope input in locked mode.
- Roundtrip test: English → 8X2 → agent → 8X2 → English, inspected, repeatable.
- Failure test: wrong passphrase yields no plaintext.

Until then, 8X is a keypad and a spec.

---

## 10. Continuation

Next authorized tranche is L2: Dragon or other owned runtime grows `unlock` / `lock`, keychain-backed secret, no plaintext ledger by default.

This document does not authorize that work. `GO` on the agent repo does.
