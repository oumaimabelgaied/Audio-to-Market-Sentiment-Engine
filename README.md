# Earnings Call Audio–Text Analysis: Post-Call Volatility

Exploratory pipeline relating vocal and textual features of S&P 500 earnings
calls to stock volatility after the call. Based on the dataset and task of
Qin & Yang (ACL 2019), "What You Say and How You Say It Matters".

## What I built
- **Labels:** 7/15/30-day post-call volatility, computed from daily prices
  (yfinance) with the paper's formula: log of the standard deviation of
  daily returns. 3-day labels are missing for 7 calls (a 3-calendar-day
  window often contains fewer than 3 trading days).
- **Audio features (librosa):** per-sentence pitch (pYIN), RMS energy and
  zero-crossing rate for the lead executive's clips, aggregated per call.
- **Text features (FinBERT, ProsusAI/finbert):** per-sentence
  P(positive) - P(negative), aggregated per call (mean, std, share of negative
  sentences, first-vs-second-half trend).
- **Sample:** 18 calls from 2017, chosen across sectors.

## Results
- Label sanity check: past volatility vs future volatility has Spearman
  rho = 0.15 / 0.23 / 0.51 at 7 / 15 / 30 days, the same pattern as the
  paper's baseline.
- With all 18 calls, no feature correlates significantly with volatility
  (largest |rho| = 0.42).
- Excluding the 12-sentence Goldman Sachs call, mean vocal energy vs 30-day
  volatility reaches rho = -0.55 (n = 17, nominal p ~ 0.02). This does not
  survive correction for the 30 comparisons run, and is treated as a
  hypothesis for a larger sample, not a finding. A check against
  zero-crossing rate found no sign that it reflects noisy audio, but
  recording conditions cannot be measured here.

## Limitations
- 18 calls is far too few to detect modest effects (roughly 85 would be
  needed for rho = 0.3). The paper used 576.
- One call and one speaker per company: pitch mostly reflects who is
  speaking, and cannot be normalized to each speaker's own baseline.
- Goldman Sachs has only 12 usable sentences; Facebook lost 5 audio clips to
  memory errors.
- FinBERT was trained on written financial news. It scores content, not tone,
  and misreads some sentences (e.g. "operating costs were down 3%" scored
  negative).
- The audio folder is labeled CEO, but the dataset keeps the most-spoken
  executive.

## Data
Audio and transcripts: GeminiLn/EarningsCall_Dataset (not redistributed here).
drive link : https://drive.google.com/drive/folders/1BKCANORbcmUJKkOkBOghw6uNHPqS_az1

##References:
Qin & Yang ACL 2019 paper :"What you say and How you say it matters"

used the same dataset : 2017 earning calls from S&P500 companies , short-listed 18 diverse companies ( high + low volatility ) 
for this particular project 

kogan 2009 : for volatility calculations