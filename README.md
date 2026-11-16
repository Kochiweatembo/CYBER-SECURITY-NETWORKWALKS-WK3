# PASSWORD CRACKING LABS - WEEK 3

Networkwalks Cybersecurity Internship 2026 | Essentials: W3-PM1 (JTR) + W3-PM2 (NW Tools)

| Field | Detail |
|---|---|
| **Pentester Name (Cybersecurity Professional)** | Kochiwe Anthony Tembo |
| **Program/Batch** | Networkwalks Cybersecurity Internship 2026, Batch B084
| **Modules completed** | W3-PM1 (Password Cracking with JTR)<br>W3-PM2 (Password Cracking with Networkwalks Tools) |
| **Target files** | Password-protected training PDFs supplied by Networkwalks for this lab only |
| **Permission** | Yes - cracking these specific lab files is the assigned task |

---

## 1. Liability Disclaimer

I cracked only the password-protected PDF files supplied by Networkwalks for this lab. These are training files provided for the exercise, not files belonging to a third party. I did not run these tools against any file I do not own or was not told to test. Password cracking against systems or files without authorization is illegal in most countries and is not what this lab is for.

## 2. Introduction

This repo covers the two essential Week 3 modules: cracking a locked PDF's password using **John the Ripper (JtR)** through its GUI, Johnny (W3-PM1), and cracking a locked PDF's password using **Networkwalks' own browser-based tools** (W3-PM2).

**Background.** A locked PDF stores its password as a hash, not as plain text. Hashing is one-way: you cannot reverse a hash back into the password directly. Instead, a cracking tool hashes candidate words from a wordlist and compares each result to the stored hash; a match reveals the password. This is called a dictionary attack. It only works well against passwords that are short, common, or in the wordlist used - which is exactly why "password1"-style passwords are considered weak.

## 3. Tools Used

| Tool | Purpose |
|---|---|
| John the Ripper (JtR), via Johnny GUI | Industry-standard offline password cracker; Johnny is its graphical front end |
| Online HashCrack (onlinehashcrack.com) | Browser tool used to extract a crackable hash from the locked PDF, instead of running the `pdf2john` script that normally ships with John the Ripper |
| Networkwalks Hash Calculator | Browser tool that extracts a `$pdf$...` hash from a locked PDF, computed locally in the browser |
| Networkwalks Password Cracker | Browser tool that runs a dictionary attack against a pasted `$pdf$...` hash |

## 4. W3-PM1: Password Cracking with JTR

### Step 1 - Extract the hash

I used **Online HashCrack** to extract a crackable hash from the locked file **"My Locked PDF3.pdf"** (Figure 1).

- Extracted hash: `$pdf$4*4*128*-1028*1*16*34eb542eff4e1b0b32d25ce15a9a7281*32*b77872bfc9a24fb2f84506 6283a8fc1b0021446990b9e4114071a4d9104984c1*32*e7572256e4b552cd57988f5134214b91920d...` (continues beyond what the screenshot shows).
- This step does the same job as the `pdf2john.pl` script that ships with John the Ripper: both turn a password-protected PDF into a hash format the cracker can read. I used the online tool because it needed no extra setup.

⚠️ **Privacy note:** this file is a training sample, so uploading it to a third-party website was not a risk. For a real or sensitive document, I would use the local `pdf2john` script instead, since an online service means the file briefly leaves your control.

### Step 2 - Crack the hash with Johnny (JtR GUI)

I loaded the hash into **Johnny**, the graphical front end for John the Ripper, and ran the default attack (Figure 2).

| User | Password | Hash (as shown in Johnny) | Format |
|---|---|---|---|
| ? | **password1** | `$pdf$4*4*128*-102...` (truncated by the column width; matches the hash from Step 1) | PDF |

- Result: **100% (1/1: 1 cracked, 0 left) [format=PDF]**.

### Why it matters

John the Ripper is the reference tool most other password-cracking utilities are compared against. Learning the extract-hash-then-crack workflow here is the same two-step pattern used in the NW Tools module below and in most real-world credential-cracking work.

## 5. W3-PM2: Password Cracking with Networkwalks Tools

### Step 1 - Extract the hash

I used the **Networkwalks Hash Calculator**, PDF tab, on the file **"My Locked PDF1 (for NW Tools Lab WK3-PM2).pdf"** (65.2 KB), which hashes the file locally in the browser and returns a `$pdf$...` hash (Figure 3).

- Extracted hash: `$pdf$4*4*128*-1060*1*16*55d1a5c14175da449753199e44971d32*32*777fd021a7f3c5ae598c0c6495c7f76e00000000000000000000000000000000*32*ceecdac74b19b5a62688d3b3524e1374c955cbb9cc3c45316494d9446ef81af1`
- Revision: R4, Version: V4, Key length: 128 bit.

### Step 2 - Crack the hash with the Networkwalks Password Cracker

I pasted that hash into the **Networkwalks Password Cracker** and ran its built-in 100-word dictionary attack (Figure 4).

- Hash used: `$pdf$4*4*128*-1060*1*16*55d1a5c14175da449753199e44971d32*32*777fd021a7f3c5ae598c0c6495c7f76e00000000000000000000000000000000*32*ceecdac74b19b5a62688d3b3524e1374c955cbb9cc3c45316494d9446ef81af1`
- Progress at match: **91/100 tried, 9 passwords/second, 91%**.
- Result: **PASSWORD CRACKED SUCCESSFULLY -> password1**.

### Note on this result

The hash matches the value shown in the Networkwalks lab's own written walkthrough for "My Locked PDF1.pdf" - every student downloads the same fixed demo file, so getting the same hash is expected and is not a mistake. Figure 3 now shows the Hash Calculator step run directly against that file, so both steps of this module are independently evidenced.

### Why it matters

This module shows that the same dictionary-attack idea behind John the Ripper can be run entirely in a browser, with no installation. It is slower here (9 passwords/second against a 100-word list) than John the Ripper would be, but the underlying logic - hash the guess, compare, repeat - is identical.

## 6. Conclusion

Both modules cracked the same weak password, **"password1"**, using two different tool chains: a locally installed GUI tool (Johnny / John the Ripper) and a pair of browser-only tools (Networkwalks Hash Calculator and Password Cracker). The lesson from both is the same: a password that is a dictionary word followed by a digit is cracked almost immediately by even a 100-word list, which is why real passwords need to be longer, unpredictable, and not based on common words.

I also learned to be careful about *where* a password hash is extracted. Doing it locally (the `pdf2john` script, or a Networkwalks tool that hashes in-browser) keeps the file under your control; an online extraction service is fine for a training file but should not be a habit for anything sensitive.

## 7. Evidence Collected

**Figure 1 - Online HashCrack: hash extracted from "My Locked PDF3.pdf"**

![1_onlinehashcrack_extract](screenshots/1_onlinehashcrack_extract.png)

**Figure 2 - Johnny (JtR GUI): password cracked ("password1")**

![2_johnny_jtr_crack](screenshots/2_johnny_jtr_crack.png)

**Figure 3 - Networkwalks Hash Calculator: hash extracted from "My Locked PDF1.pdf"**

![3_nw_hash_calculator](screenshots/3_nw_hash_calculator.png)

**Figure 4 - Networkwalks Password Cracker: dictionary attack result ("password1")**

![4_nw_password_cracker](screenshots/4_nw_password_cracker.png)

---

-End-

## 👤 Author

**Kochiwe Anthony Tembo**
Cybersecurity intern, Networkwalks 

LinkedIn: <https://www.linkedin.com/in/kochiwe-tembo-0a52a4400>
GitHub: <https://github.com/Kochiweatembo>

## 📌 Project Information

**Program:** Cybersecurity Internship 2026 at Networkwalks | **Week:** 03 | **Repository:** GitHub
