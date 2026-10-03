# NETWORKWALKS-ADEDUROTIMI-BO83-WK2-PM1-PM2-PASSWORD-CRACKING

# 🔐 Password Cracking Report — John the Ripper & Networkwalks Tools

**Type:** Password/Hash Cracking (Authorized Lab Exercise)
**Target:** 3 password-protected PDF files provided by Networkwalks Academy
**Date:** 27 May 2026 (Module 1) & 26 August 2026 (Module 2)
**Author:** *(add your name here)* — Cybersecurity Trainee, Networkwalks Program (Batch B083)
**Environment:** Windows (JTR + Johnny GUI), Web browser (Networkwalks online tools)

---

## ⚠️ Liability Disclaimer

All activities documented in this report were performed against sample PDF files provided specifically for this purpose by Networkwalks Academy, as part of an authorized training program. This report is for educational and portfolio purposes only. Attempting to crack passwords or bypass encryption on files or accounts you do not own or have explicit permission to test is illegal in most jurisdictions. Do not use this content against systems or files without authorization.

---

## 📖 Introduction

This report documents **Week 3** of the Networkwalks Cybersecurity program, covering two linked project modules on password cracking:

- **Module 1 — Password Cracking with JTR:** using **John the Ripper (JTR)** and its GUI, **Johnny**, to recover passwords from two encrypted PDF files.
- **Module 2 — Password Cracking with Networkwalks Tools:** using Networkwalks' own browser-based **Hash Calculator** and **Password Cracker** (dictionary attack) to recover the password of a third encrypted PDF.

The exercise demonstrates the full password-cracking workflow: extracting a crackable hash from a protected file, running that hash through a cracking tool with a wordlist, and using the recovered password to unlock the original file — exactly the technique real attackers (and penetration testers) use against weak passwords.

---

## 🛠️ Tools Used

| Tool | Purpose |
|---|---|
| **pdf2john** (via [OnlineHashCrack](https://www.onlinehashcrack.com/tools-pdf-hash-extractor.php)) | Extract a crackable `$pdf$...` hash from an encrypted PDF |
| **John the Ripper (JTR)** | Password-hash cracking engine |
| **Johnny** | GUI front-end for John the Ripper |
| **Networkwalks Hash Calculator** ([networkwalks.com/hash-calculator](https://networkwalks.com/hash-calculator/)) | Browser-based tool to extract a `$pdf$...` hash directly from a PDF |
| **Networkwalks Password Cracker** ([networkwalks.com/password-cracker](https://networkwalks.com/password-cracker/)) | Browser-based dictionary-attack tool for cracking the extracted hash |

---

## 🕵️ Module 1: Password Cracking with JTR (John the Ripper + Johnny)

### PDF 1 — "My Locked PDF1.pdf"

**Step 1 — Extract the hash**

The encrypted PDF was uploaded to the OnlineHashCrack PDF Hash Extractor (which uses `pdf2john` under the hood). The extracted hash was saved as `hash1.txt`:

```
$pdf$4*4*128*-1060*1*16*55d1a5c14175da449753199e44971d32*32*777fd021a7f3c5ae598c0c6495c7f76e00000000000000000000000000000000*32*ceecdac74b19b5a62688d3b3524e1374c955cbb9cc3c45316494d9446ef81af1
```

📸 `pwcrack-screenshots/fisthashed.PNG`

**Step 2 — Crack the hash with Johnny**

The hash file was opened in Johnny (JTR's GUI) and an attack was started.

**Result:** Cracked in a single pass — **`password1`**

📸 `pwcrack-screenshots/passhash1.PNG`

**Step 3 — Unlock the PDF**

Opening the PDF with `password1` revealed:

> 🎉 **Flag 1:** `nw{networkwalks_flag1_jtr_270521_1}`

📸 `pwcrack-screenshots/capturedflag1.PNG`

---

### PDF 2 — "My Locked PDF2.pdf"

**Step 1 — Extract the hash**

A second hash was extracted (`hash2.txt` / `My-Locked-PDF2_hash.txt`):

```
$pdf$4*4*128*-1028*1*16*0853f2cde0ef15b1c0f93ed229d3b1ad*32*8f13ce5aa39ad974364d36a057da76790021446990b9e4114071a4d9104984c1*32*ceecdac74b19b5a62688d3b3524e1374c955cbb9cc3c45316494d9446ef81af1
```

**Step 2 — Crack the hash with Johnny**

Running the same JTR/Johnny attack against this second hash cracked it to the **same password as PDF 1: `password1`**.

**Observation:** Reusing an already-cracked password dramatically speeds up subsequent cracks — this is exactly how real-world "credential stuffing" and password-reuse attacks work once one password in a set is known.

**Step 3 — Unlock the PDF**

Opening the PDF with `password1` revealed:

> 🎉 **Flag 2:** `nw{networkwalks_persistence_jtr_270521}`
>
> *"Cracking passwords is all about patience and the right wordlist. You are learning the mindset of a real security tester at Networkwalks."*

📸 `pwcrack-screenshots/capturedflag2.PNG`

---

## 🕵️ Module 2: Password Cracking with Networkwalks Tools

### PDF 3 — "My-Locked-PDF3.pdf"

Unlike Modules 1–2 above, this task used Networkwalks' own **browser-based** tools instead of installing JTR locally.

**Step 1 — Extract the hash with the Hash Calculator**

The encrypted PDF was uploaded directly to the Networkwalks Hash Calculator (`networkwalks.com/hash-calculator`), which parses the PDF locally in-browser and extracts a `pdf2john`/hashcat-compatible hash — no file is uploaded to a server.

```
$pdf$4*4*128*-1028*1*16*34eb542eff4e1b0b32d25ce15a9a7281*32*b77872bfc9a24fb2f845066283a8fc1b0021446990b9e4114071a4d9104984c1*32*e7572256e4b552cd57988f5134214b91920d94d7a6bf550ea94a2995c7f2ab02
```

(Revision 4, Version 4, 128-bit key length)

📸 `pwcrack-screenshots/thirdhash.PNG`

**Step 2 — Crack the hash with the Password Cracker**

The hash was pasted into the Networkwalks Password Cracker, which ran a **dictionary attack using its built-in 100-password wordlist**, trying common passwords such as `service`, `canada`, `hockey`, `killer`, `george`, `asdfgh`, `zxcvbn`, `qwertyuiop`, `111222`...

**Result:** Match found at 91/100 attempts — **`1qaz2wsx`**

📸 `pwcrack-screenshots/passwordcracked.PNG`

**Step 3 — Unlock the PDF**

Opening the PDF with `1qaz2wsx` revealed:

> 🎉 **Flag 3:** `nw{networkwalks_flag_260821_1}`

📸 `pwcrack-screenshots/capturedflag3.PNG`

---

## 📊 Summary of Results

| PDF | Hash Prefix | Tool Used | Attack Type | Password Recovered | Flag Captured |
|---|---|---|---|---|---|
| PDF 1 | `...-1060...` | JTR + Johnny | Wordlist | `password1` | `nw{networkwalks_flag1_jtr_270521_1}` |
| PDF 2 | `...-1028...` | JTR + Johnny | Wordlist (password reuse) | `password1` | `nw{networkwalks_persistence_jtr_270521}` |
| PDF 3 | `...-1028...` (different hash) | NW Hash Calculator + Password Cracker | Dictionary (100-word built-in list) | `1qaz2wsx` | `nw{networkwalks_flag_260821_1}` |

---

## 📊 Risk Analysis / Impact

| # | Finding | Evidence | Potential Impact | Risk Level |
|---|---|---|---|---|
| 1 | Weak, dictionary-guessable passwords | `password1`, `1qaz2wsx` — both common patterns | Trivially cracked by any basic wordlist attack, often in seconds to minutes | 🔴 Critical |
| 2 | Password reuse across files | PDF1 and PDF2 shared the identical password | Cracking one password instantly compromises every other file/account protected by the same password | 🔴 Critical |
| 3 | PDF encryption relies entirely on password strength | All 3 hashes were crackable via basic wordlists | PDF "protection" offers little security if the password itself is weak | 🟠 Medium |
| 4 | Hash extraction requires no special access | Any PDF's hash can be extracted with a free online tool or `pdf2john` | Anyone with a copy of an encrypted file can attempt to crack it offline, with unlimited attempts and no lockout | 🟠 Medium |

**Risk level key:** 🔴 Critical · 🟠 Medium · 🟢 Low

> These passwords were deliberately weak, provided for training purposes. The risk ratings reflect what these patterns would mean in a real-world setting, not a flaw in Networkwalks' lab design.

---

## ✅ Recommendations

1. **Use long, unique passphrases** — at least 12–16 characters, combining unrelated words, numbers, and symbols, instead of predictable patterns like `password1` or keyboard-walk patterns like `1qaz2wsx`.
2. **Never reuse passwords** across files, accounts, or services — one cracked password should never unlock more than one thing.
3. **Use a password manager** to generate and store strong, unique passwords rather than relying on memorable (and therefore guessable) ones.
4. **Treat file-level encryption passwords with the same seriousness as account passwords** — a strong PDF/ZIP/Office password resists offline dictionary and brute-force attacks far longer than a weak one.
5. **Understand that offline cracking has no rate limit** — unlike a login form, a cracking tool can try unlimited passwords per second against an extracted hash, so password strength is the only real defense.
6. **Always operate within authorized scope** — all cracking in this report was performed only against files explicitly provided for this training exercise.

---

## 🧾 Conclusion

This project walked through both a traditional, locally-installed cracking workflow (John the Ripper + Johnny) and a fully browser-based one (Networkwalks' own Hash Calculator and Password Cracker), cracking three different password-protected PDFs.

The biggest takeaway is how fast weak passwords fall: a dictionary attack against a 100-word list cracked `1qaz2wsx` in 91 tries, and `password1` was cracked almost instantly by both PDFs that used it. Reusing that same password on PDF 2 also demonstrated, firsthand, why credential reuse is one of the most damaging habits in security — cracking one password effectively cracks every file or account protected by it.

Every activity in this report was carried out only against sample files provided specifically for this authorized training exercise.

---

## 📁 Evidence

Screenshots referenced above are stored in [`/pwcrack-screenshots`](./pwcrack-screenshots):

- `fisthashed.PNG` — hash extracted for PDF 1
- `passhash1.PNG` — Johnny GUI cracking PDF 1's hash → `password1`
- `capturedflag1.PNG` — Flag 1 captured
- `capturedflag2.PNG` — Flag 2 captured (PDF 2, password reuse)
- `thirdhash.PNG` — hash extracted for PDF 3 via Networkwalks Hash Calculator
- `passwordcracked.PNG` — Networkwalks Password Cracker result → `1qaz2wsx`
- `capturedflag3.PNG` — Flag 3 captured

Raw extracted hashes are stored in [`/pwcrack-evidence`](./pwcrack-evidence):
- `hash1.txt` — PDF 1 hash
- `hash2.txt` / `My-Locked-PDF2_hash.txt` — PDF 2 hash (identical to each other)
- `My-Locked-PDF3_hash.txt` — PDF 3 hash

---

**👤 Author:** *(add your name)*
**Program:** Cybersecurity & Ethical Hacking Internship — Networkwalks | Week 3 (Batch B083)
**LinkedIn:** *(add your LinkedIn, optional)*
