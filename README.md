# Rocket Fuel × TaskaBella: Ad Incrementality Reanalysis and Follow-up Experiment Design

This project reanalyzes case data (UC Berkeley Haas) from a randomized display-ad experiment that Rocket Fuel ran for TaskaBella from November 2015 to February 2016. It quantifies incremental purchases and ROI, evaluates the size of the control group, and proposes two follow-up experiments: a randomized frequency cap and a bid multiplier for light users.

The raw data and the case document are not in this repository (copyright). `§` refers to section numbers in the notebook. The dashboard is `index.html`.

## Decisions

1. Keep the campaign. The ads raised conversion by 0.77pp (43.1% relative), and ROI was 32.2%. The 95% confidence interval for ROI is 2.3% to 62.2%, so the size of the profit is uncertain.
2. 44.9% of ad impressions ($56,676) went to users with 1-29 or 200+ impressions, where measured lift is close to zero. Test the 200+ end with a randomized frequency cap. The 1-29 end cannot be changed with a cap and needs a separate bid-multiplier test.
3. Keep the 4% control group. Cutting it by 1 percentage point would recover $1,809.58 in incremental value, but the 200+ band would drop from about 240 control users to 180, weakening the segment analysis that found the problem in decision 2.

## Problem

TaskaBella was about to reallocate its handbag ad budget and needed to know how many purchases the ads caused. The brand already had strong word-of-mouth on social media, so many buyers might have purchased without seeing an ad (case p. 2).

## Hypotheses

- Users who saw the ad (test) convert at a higher rate than users who saw a public service announcement (control).
- The value of incremental conversions ($40 each) exceeds the media cost ($9 CPM). Both figures are case assumptions (p. 2).

## Experiment design (designed and run by the two companies)

- Unit and size: user (hashed cookie ID), 588,101 users
- Assignment: random, 96% ad and 4% PSA. The PSA ran in the same size and position as the ad.
- Period: November 2015 to February 2016
- Outcome: purchased the handbag during the campaign
- Analysis basis: intention-to-treat (every assigned user counts)

## Validation

- Sample ratio: control share 4.00%, no gap from the plan (chi-square p = 0.9998, §22)
- Balance: the distributions of impression band, top-exposure day, and top-exposure hour differ between groups (p < 0.001 for each, §23). Results split by these variables are read as heterogeneous effects, not causal effects.
- Tests: two-proportion z-test and 95% confidence intervals. Comparisons across bins, bands, days, and hours use Holm correction and Cochran's Q.

## Results

| Metric | Value | Notebook |
|---|---|---|
| Conversion rate (test / control) | 2.55% / 1.79% | §6 |
| Absolute lift (95% CI) | 0.77pp (0.60 to 0.94pp) | §6, §8 |
| Relative lift | 43.1% | §6 |
| P-value | 1.71 × 10⁻¹³ | §7 |
| Incremental conversions (95% CI) | 4,343 (3,360 to 5,326) | §9, §24 |
| Incremental value / campaign cost | $173,719 / $131,375 | §10 |
| ROI (95% CI) | 32.2% (2.3% to 62.2%) | §10, §24 |
| Opportunity cost of the control group | $7,238.29 | §11 |
| Minimum detectable effect at 4% control | 0.248pp (13.9% relative) | §25 |

Results by impressions per user. The band cutoffs were chosen after seeing the data, so this table is exploratory (§27).

| Impressions per user | Share of test users | Share of ad impressions | Lift (95% CI) | Share of incremental | ROI (95% CI) |
|---|---|---|---|---|---|
| 1-29 | 76.9% | 31.7% | 0.00pp (-0.12 to 0.12) | 0.1% | -99.7% (-152.1% to -47.3%) |
| 30-129 | 20.7% | 46.1% | 3.37pp (2.75 to 3.99) | 90.6% | 170.3% (120.5% to 220.1%) |
| 130-199 | 1.4% | 8.9% | 5.21pp (1.84 to 8.58) | 9.5% | 47.1% (-48.0% to 142.1%) |
| 200+ | 1.0% | 13.2% | -0.13pp (-4.65 to 4.38) | -0.2% | -101.8% (-163.9% to -39.8%) |

- Across the 13 ten-impression bins, the bins significant after Holm correction are 50-99 and 110-119. Lift differs across bins (Cochran's Q p = 3.94 × 10⁻³¹, §26).
- Lift differs by day (Q p = 0.00035), but the day distribution itself differs between groups, so this is descriptive only. Thursday and Sunday are not significant after Holm correction (§29).
- There is no evidence that lift differs by hour (Q p = 0.839). Only 14:00 is significant after Holm correction (§29).

## Recommended actions

1. Keep the campaign, and report ROI as a range in future campaign reviews.
2. Run a randomized frequency-cap test (design only, §30).
   - Arms: PSA control 4%, no cap 48%, 200-impression cap 48%
   - Analysis population: users who reach their 200th impression opportunity. Both ad arms are treated the same until that point, so the comparison stays randomized.
   - Primary metric: incremental profit per triggered user = $40 × conversion-rate difference + ad cost saved
   - Decision rule: adopt the cap if the lower bound of the one-sided 97.5% confidence interval for the conversion-rate difference is above the break-even loss of -2.78pp
   - Expected saving: $6,376 in ad cost (5.06% of ad impressions)
   - Sample size: 2,676 triggered users per arm, or 263,228 users per arm in total. A campaign of this size provides 282,288 per arm.
   - If the cap passes, test a 130-impression cap next.
3. Run a randomized bid-multiplier test for users below 30 impressions (design only, §31).
   - The data cannot tell whether light users have low intent (control conversion 0.67% vs 4.56% in the 30-129 band) or saw too few ads. Raising bids while a user has fewer than 30 impressions separates the two.
   - The case data has no bid or auction data, so the extra cost is measured in the test, not assumed.
   - At this campaign's size the test can detect a 0.118pp conversion difference between ad arms, so it can only judge multipliers that add at least $0.047 per user (21.1% of current ad spend per user).
4. Keep the control share at 4%. The overall test alone could have detected this campaign's lift with a 0.40% control group, but that figure uses the lift observed after the fact and cannot set a design (§25).

## Methods

- Intention-to-treat comparison, pooled two-proportion z-test, unpooled 95% Wald intervals
- Sample ratio mismatch check (chi-square goodness of fit) and balance checks on post-assignment exposure variables (chi-square test of independence)
- Holm correction within each family of comparisons (13 bins, 4 bands, 7 days, 16 hours) and Cochran's Q for heterogeneity
- Minimum detectable effect by control share; post-hoc minimum control share reported separately
- Follow-up designs: trigger-based analysis with a non-inferiority margin at break-even (frequency cap); intention-to-treat profit comparison with measured cost (bid multiplier)

## How to run

1. Place `Rocket Fuel data analysis.xlsx` from the case materials in `data/`.
2. Install Python 3 with pandas, openpyxl, matplotlib, scipy, and statsmodels.
3. Open `rocket_fuel_ads_experiment.ipynb` and run all cells. Result tables are written to `outputs/`.
4. `index.html` is the dashboard. Its numbers are the aggregates in `outputs/dashboard_data.json` (written by §32), embedded in the page; no user-level data is included.

| Part | Sections | Content |
|---|---|---|
| 1 | §1 to §21 | Course analysis: lift, significance, incremental conversions, ROI, control cost, frequency, day, hour |
| 2 | §22 to §33 | Randomization and balance checks, ROI range, control size, corrected frequency and timing tests, two follow-up designs, limitations |

## Limitations

- Reanalysis of case data. The case notes that some names and figures were altered (p. 1).
- The $40 value per conversion and the $9 CPM are case assumptions. The ROI range reflects sampling uncertainty in the lift only.
- Frequency, day, and hour are measured after assignment and differ between groups, so results split by them describe heterogeneity, not causal effects.
- The frequency bands were chosen after seeing the data.
- Both follow-up experiments were designed, not run.
