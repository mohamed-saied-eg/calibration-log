Calibration Log

Calibration Table

#| Question| My answer before AI| My confidence| AI's answer (short)| AI's stated confidence| How I verified| Verdict
1| In Python, is 0.1 + 0.2 exactly equal to 0.3?| No| 80%| No, it gives 0.30000000000000004| 99%+| Ran code| Correct
2| Does print([3, 1, 2].sort()) print [1, 2, 3]?| No| 90%| No, it prints None| 99.9%+| Ran code| Correct
3| What does len("hello") return?| 5| 100%| 5| Essentially certain| Ran code| Correct
4| Is it safe to store passwords as plain text like 123456?| No| 90%| No, use a salted slow hash (Argon2id)| 99%+| OWASP cheat sheet| Correct
5| Does https:// guarantee a site is safe and not a scam?| No| 90%| No, it only means encrypted connection| 99%+| Progress blog| Correct

Evidence

Q1: Floating-Point Arithmetic

Prompt: In Python, is 0.1 + 0.2 exactly equal to 0.3?

AI answer: No. In Python, "0.1 + 0.2 == 0.3" is "False". The sum comes out as "0.30000000000000004". Floats are stored in binary, and 0.1, 0.2 and 0.3 cannot be represented exactly in base 2. Use "math.isclose()" to compare floats, or "decimal.Decimal" for exact decimal arithmetic.

Confidence follow-up: Very confident, about 99%+, for standard Python on normal machines. Other float types or unusual implementations may differ. The AI recommended running the code to verify.

Verification:

print(0.1 + 0.2 == 0.3)
print(repr(0.1 + 0.2))

Output:

False
0.30000000000000004

Q2: Python list.sort()

Prompt: In Python, does "print([3, 1, 2].sort())" print "[1, 2, 3]"?

AI answer: No, it prints "None". "list.sort()" sorts the list in place and returns "None". Use "sorted()" to get a new sorted list.

Confidence follow-up: Essentially certain, 99.9%+. This is documented Python behavior, except when custom classes change the behavior.

Verification:

print([3, 1, 2].sort())
print([3, 1, 2].sort() is None)

Output:

None
True

Q3: Python len()

Prompt: In Python, what does "len("hello")" return?

AI answer: It returns "5". The string contains five characters: h, e, l, l, o.

Confidence follow-up: Essentially certain. One nuance is that "len()" counts Unicode code points, not necessarily visible characters.

Verification:

print(len("hello"))

Output:

5

Q4: Password Storage Security

Prompt: Is it safe to store user passwords in a database as plain text like "123456"?

AI answer: No. Plain-text storage exposes passwords if the database leaks. Store a salted password hash using a suitable password-hashing algorithm such as Argon2id. Do not use plain MD5 or SHA-256 alone.

Confidence follow-up: Essentially certain, 99%+, on the core claim. The AI's initial answer listed bcrypt as an equal alternative, but OWASP recommends bcrypt mainly for legacy systems.

Verification: Official OWASP Password Storage Cheat Sheet.

Source: https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html

Key finding: Passwords should never be stored in plain text. OWASP recommends Argon2id with a minimum configuration of 19 MiB memory, two iterations, and parallelism of one.

Q5: HTTPS and Website Safety

Prompt: Does a website URL starting with "https://" guarantee the website is safe and not a scam?

AI answer: No. HTTPS protects the connection through encryption and certificate validation, but scammers can also obtain valid certificates. Check the exact domain, how you reached the site, and other warning signs.

Confidence follow-up: Essentially certain, 99%+, on the main claim. The AI advised against quoting unverified phishing statistics.

Verification: Progress Software article on HTTPS and website security.

Source: https://progress.com/blogs/https-and-a-padlock-does-not-mean-a-website-is-secure

Key finding: HTTPS and the padlock indicate an encrypted connection, not a guarantee that the website itself is trustworthy.

Reflection

In row 1 I was right but only 80% confident, while the AI said 99%+ and the code confirmed it, so I was under-confident. In row 2 I was 90% confident, but my first instinct was the opposite answer, which shows my confidence was not stable until I checked by running code. In row 4 the AI was slightly overconfident: it listed bcrypt as an equal alternative, but OWASP says bcrypt is for legacy systems only. In row 1 the AI also said "I believe" about numpy.float32, which it did not verify. The AI's tone sounded equally sure everywhere, so tone tells me nothing.

My Verification Rule

Before I merge any AI-suggested code or security advice, I run it or quote the official documentation (with a link) in the pull request. If I did neither, I write "not verified" in the PR description.
