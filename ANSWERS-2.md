# Midterm 1: answers

GitHub username: arailymtoishybekova-debug
Session code: ember-92
Fingerprint: b819539e

Fill in the three lines above from my_data/DATA.md. Do not rename headings or table columns: a script reads the numbers. Write only in the empty cells and lines. Percentages as 0.78 or 78 %, money as plain numbers.

## AI-use declaration

I used Claude (Anthropic) during this exam. It wrote a Python script that computes recall, MRR, Wilson intervals and costs from the formulas in the README, and I ran it on my own data. It also helped me draft the wording of the decision (1d), the memo (2b) and the Part 3 explanations. I checked the numbers against the formulas and the data, and I can explain every number and sentence below. I used no other tools.

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---|---|---|---|
| A | 0.483 | 0.583 | 0.683 | 0.556 |
| B | 0.450 | 0.650 | 0.733 | 0.554 |

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
| A, k=3 | 0.583 | 0.457 | 0.699 |
| A, k=5 | 0.683 | 0.558 | 0.787 |
| B, k=3 | 0.650 | 0.524 | 0.758 |
| B, k=5 | 0.733 | 0.610 | 0.829 |

### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
| A, k=3 | 26379.77 |
| A, k=5 | 34926.22 |
| B, k=3 | 16769.37 |
| B, k=5 | 18908.89 |

### 1d. Decision (at most 150 words)

Chosen candidate: B, k=5

I recommend B, k=5. Both A candidates cost $26,380 and $34,926 a year, above the $22,600 budget, so they are out. B, k=3 costs $16,769 and B, k=5 $18,909, both within budget. B, k=5 has the highest recall, 0.733 [0.610, 0.829], but all four intervals overlap, so my data does not show that any candidate is better than another; I choose B, k=5 because the extra $2,140 over B, k=3 is affordable and it never has lower recall. Per language, its recall is EN 0.950, RU 0.600, KK 0.650. Russian speakers are served worst (about 4 in 10 questions fail), although they are 43 % of traffic; Kazakh speakers are next. The per-language samples are only 20 questions each, so these intervals are wide (RU [0.387, 0.781]).

### 1e. Next measurement (at most 60 words)

Run B, k=3 and B, k=5 on at least 200 real questions per language, taken from production logs. If B, k=5 does not beat B, k=3 for Russian and Kazakh with non-overlapping intervals, switch to B, k=3 and save about $2,140 a year.

## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
| H | 0.883 | KK, clean | 0.767 | 0.591 | 0.882 | 19250.10 |
| L | 0.806 | KK, degraded | 0.667 | 0.488 | 0.808 | 10800 |

Personal-data pages per year sent abroad under H: 394200

### 2b. Memo (at most 300 words)

To: Head of Operations. Recommendation: pipeline L.

Who is served worst. Overall accuracy is H 0.883 and L 0.806, but an overall figure hides the weak spots. H's weakest cell is Kazakh clean scans, 0.767 [0.591, 0.882]; L's weakest is Kazakh degraded scans, 0.667 [0.488, 0.808]. These intervals overlap, so with 30 invoices per cell I cannot claim H is better in that cell. Kazakh is the weak language in both pipelines. I am only moderately sure, because 30 per cell is small.

Personal data. In 18 % of pages a name and ИИН are printed: 394,200 pages a year. The ИИН falls under Kazakhstan's law No. 94-V. Three questions. Where does the scan go? H: every page to a data centre abroad; L: stays on our server in Kazakhstan. Who stores it? H: the vendor, and I do not know whether it keeps images; L: the bank. For how long? H: unknown, must be asked in writing; L: we decide. A vendor's statement is not a compliance review, and this is not legal advice; the legal team must confirm whether sending ИИН abroad is allowed.

Safeguards. (1) On L, delete images after a fixed number of days, checked by a monthly audit of a sample. (2) Keep an access log of who opens scans. (3) Send Kazakh degraded and low-confidence pages to human review, and report accuracy per cell monthly. (4) Use H only after the vendor's written retention terms are reviewed and ИИН pages are kept on L.

Choice. L costs $10,800 a year against $19,250 for H and keeps personal data in Kazakhstan. Its overall accuracy is 7.7 points lower. I choose L. Remaining risk: about one in three Kazakh degraded invoices is wrong, so payment errors are possible unless human review catches them.

## Part 3: Explain your own numbers

### Q1

Number: 2.44
Why (at most 40 words): Token ratio is 2.75, but output tokens cost five times more than input and grow only 1.43× (139 to 199), while input grows 2.86×. Cheaper input dominates the Kazakh total, so the bill grows less than the tokens.

### Q2

Number: 368
Why (at most 40 words): A 512 cl100k chunk is about 526 e5 tokens, so only the last 14 are cut. The sentence ends before about 388, inside the window, so it can retrieve the chunk. The truncation is silent: no error appears.

### Q3

Number: 42.9
Why (at most 40 words): The ranking did not change (Monday still first), only the probabilities flatten. Every token still has non-zero probability, so the set of tokens that can be sampled is unchanged; unlikely ones are just sampled more often.

### Q4

Number: 14
Why (at most 40 words): The public leaderboard prefers X (72 against 68), because "I don't know" scores zero, the same as a wrong answer. That teaches a model to guess instead of abstaining. With a penalty, Y wins (30 against 16).

### Q5

Number: 8.00
Why (at most 40 words): The model has 160 tokens per parameter against Chinchilla's 20, so it is heavily over-trained by that criterion. Chinchilla said models such as GPT-3 were under-trained: too large for the amount of data they saw.
