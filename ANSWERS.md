# Midterm 1: answers

GitHub username: arailymtoishybekova-debug
Session code: EMBER-92
Fingerprint: b819539e

> Fill in the three lines above from `my_data/DATA.md`. Do not rename headings or table columns: a script reads the numbers.
> Write only in the empty cells and lines. Percentages as 0.78 or 78 %, money as plain numbers.

## AI-use declaration

I used one AI assistant, Claude (Anthropic), in the Claude app. I gave it the exam files, my GitHub username and the session code. Claude generated my data with `make_my_data.py` (the fingerprint above is the one it printed) and wrote a Python script that calculated the recall values, MRR, Wilson intervals, annual costs, the accuracy tables for Part 2 and the numbers for the five Part 3 questions. It also wrote the first drafts of the written parts: the decision in 1d, the next measurement in 1e, the memo in 2b and the Why lines in Part 3. I used no other AI tool or assistant. I read the answers before submitting them, and I am responsible for the content of this file.

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---|---|---|---|
| A | 0.4833 | 0.5833 | 0.6833 | 0.5564 |
| B | 0.4500 | 0.6500 | 0.7333 | 0.5541 |

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
| A, k=3 | 0.5833 | 0.4573 | 0.6994 |
| A, k=5 | 0.6833 | 0.5577 | 0.7869 |
| B, k=3 | 0.6500 | 0.5236 | 0.7583 |
| B, k=5 | 0.7333 | 0.6099 | 0.8287 |

### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
| A, k=3 | 26379.77 |
| A, k=5 | 34926.22 |
| B, k=3 | 16769.37 |
| B, k=5 | 18908.89 |

### 1d. Decision (at most 150 words)

Chosen candidate: B, k=3

The budget is 22600 USD a year, so A,k=3 (26380) and A,k=5 (34926) are out. Only B,k=3 (16769) and B,k=5 (18909) are affordable. Their recalls are 0.65 [0.52, 0.76] and 0.73 [0.61, 0.83]. The intervals overlap, so B,k=5 is not shown to be better, and I choose the cheaper B,k=3, saving about 2140 USD a year. Recall of B,k=3 per language (20 questions each): EN 0.85 (17/20), RU 0.55 (11/20), KK 0.55 (11/20). Russian and Kazakh users are served worst: almost half of their questions miss the right chunk, and together they are 59 % of traffic. With 20 questions per language the intervals are wide (about 0.34 to 0.74). B,k=5 stays within budget as the fallback if a larger test shows a real gain for RU and KK.

### 1e. Next measurement (at most 60 words)

Test B,k=3 against B,k=5 on at least 200 new questions per language, so each language interval is about 6 points wide on each side. If B,k=5 beats B,k=3 for RU or KK with non-overlapping intervals, switch to B,k=5, which costs 18909 USD and fits the budget.

## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
| H | 0.8833 | KK, clean | 0.7667 | 0.5907 | 0.8821 | 19250.10 |
| L | 0.8056 | KK, degraded | 0.6667 | 0.4878 | 0.8077 | 10800 |

Personal-data pages per year sent abroad under H: 394200

### 2b. Memo (at most 300 words)

To: Head of Operations.

**Who is served worst.** Both pipelines are weakest on Kazakh scans. H: Kazakh clean, 23/30 = 0.77 [0.59, 0.88]. L: Kazakh degraded, 20/30 = 0.67 [0.49, 0.81]. Overall figures hide this: L is 0.81 overall yet misreads one degraded Kazakh invoice in three. I am not very sure: cells have only 30 invoices and the H and L intervals overlap everywhere (overall H [0.83, 0.92], L [0.74, 0.86]), so the data do not prove H is more accurate.

**Personal data.** In 18 % of 6,000 daily pages a name and ИИН are printed: 394,200 pages a year. Law No. 94-V covers the ИИН. 
- Where does the scan go? H: a data centre abroad, country unknown. L: our server in Kazakhstan.
- Who stores it? H: the vendor; whether it keeps or trains on pages is unknown, and a vendor statement is not a compliance review. L: the bank.
- How long? H: unknown, I need the retention period in writing. L: we set it.

Whether sending ИИН abroad under H is allowed is for legal counsel to confirm.

**Safeguards anyone can verify.** Process scans on L only; firewall logs show no outbound traffic to external APIs. Delete scans after a fixed period, with a monthly deletion report. Encrypt storage; review access logs quarterly. Send Kazakh and degraded scans to human review; re-test 30 invoices per cell quarterly.

**Choice: L.** It costs 10,800 USD a year against 19,250 for H, and personal data stay in the country. The accuracy gap is not shown to be real. Residual risk: wrong extractions on degraded Kazakh scans (up to about half at the lower bound), so human review stays, and the bank itself must secure and delete the scans.

## Part 3: Explain your own numbers

### Q1

Number: 2.44
Why (at most 40 words): Output tokens cost 5 times input tokens. Kazakh output grew only 1.43 times while input grew 2.86 times, so the pricier output dilutes the bill ratio (2.44) below the token ratio (2.75).

### Q2

Number: 368
Why (at most 40 words): Yes. The chunk is 526 e5 tokens; the sentence starts at about token 368 and ends before 388, inside the 512 the model reads. Only the last 14 tokens are cut, silently, with no error.

### Q3

Number: 42.9
Why (at most 40 words): Dividing by T never changes the ranking: Monday stays first. It only flattens the probabilities (top token 64.0 % at T=1). Every token keeps nonzero probability, so the set that can be sampled is unchanged.

### Q4

Number: 14
Why (at most 40 words): The leaderboard prefers X (72 against 68) because saying "I don't know" earns nothing. That teaches models to guess. With the penalty, Y scores 30 and X scores 16.

### Q5

Number: 8.00
Why (at most 40 words): The model has 160 tokens per parameter against Chinchilla's 20, so it is over-trained by that criterion. Chinchilla argued that models like GPT-3 were undertrained for their data: too large, too few tokens.
