# Beyond the Overall: Quantifying Pro Wrestling Skill in AEW

What makes a great pro wrestler? This project analyzes 502 AEW wrestlers from an AEW 2K-style ratings dataset (16 skill attributes plus an Overall rating) and asks which skills actually separate elite wrestlers from everyone else.

**Interactive dashboard:** https://claude.ai/artifact/D9cnKKBNdVpDJHqJAxPBaa

## Data

- `aew_2k_style_wrestler_ratings_MAX_2026-10-08` (CSV and XLSX), 502 rows, no missing values
- 365 male, 137 female; 109 current, 248 former, 145 historical/guest/enhancement
- Rating basis: 357 "Editorial / established AEW wrestler", 145 "Estimated / short AEW appearance"
- Overall: mean 73.5, SD 12.0, range 54 to 97
- 16 skills used: Mic Skills, Charisma, Storytelling, Ring Psychology, Technical Wrestling, Grappling, Striking, High Flying, Power, Athleticism, Selling, Bumping, Timing, Tag Team, Hardcore, Submission
- Entrance is not in the file, so it is not part of any model

## Methods

1. **Driver analysis.** Pearson correlation of each skill with Overall, plus a ridge regression (alpha chosen by cross-validation) with weights standardized by each skill's SD.
2. **Archetypes.** Each wrestler's 16 skills are centered on their own mean (to remove overall quality), then clustered with K-means (k=5, 20 restarts). Clusters are matched to the five proposed archetypes by comparing cluster profiles to each archetype's defining skills (Hungarian assignment). Cluster quality is checked with the silhouette score.
3. **Wrestling Completeness Index (WCI).** `WCI = 100 - SD(16 skills)`. High means balanced across skills.
4. **AEW Value Rating.** `Actual Overall - Expected Overall`, where expected Overall is the ridge prediction from the 16 skills. Positive means rated above what the skills predict (underrated by the formula's logic); negative means below.
5. **Specialists and percentiles.** Top wrestlers per skill and each wrestler's percentile within the 502.

## Findings

- **Invisible skills track Overall most.** Correlation with Overall: Storytelling .94, Ring Psychology .93, Timing .92, Selling .90, Tag Team .89, Charisma .88. Weakest: Submission .61, Power .57, High Flying .41.
- **Overall is nearly a formula.** The 16 skills explain 97% of the variance in Overall, and the dataset's own `Calculated_Average` correlates .97 with Overall. In the regression, Charisma, Storytelling, Hardcore and Ring Psychology carry the largest weights, while Technical Wrestling, Striking and Tag Team add little once the others are included. Skills are highly collinear, so treat individual weights as indicative, not causal.
- **Styles are tendencies, not clean groups.** Silhouette is 0.27 (moderate). Cluster sizes: Technician 31, High Flyer 70, Powerhouse 75, Sports Entertainer 161, Hardcore Brawler 165. Raw (uncentered) clustering mostly splits good from bad wrestlers (silhouette .46 at k=2, about .31 to .35 for k=3 to 7), which is why profiles are centered first.
- **Moneyball residuals are small** (roughly within ±10 points). Largest negative: Matt Menard, Tomohiro Ishii, Zack Gibson, Shingo Takagi, Jeff Hardy. Largest positive: Rebel, Vickie Guerrero, Satnam Singh, Brandi Rhodes, Jora Johl. Positive residuals skew toward personality-driven and estimated-rating rows, so they likely reflect non-skill factors such as star power.

## Limitations

- Ratings are editorial, not measured. Analyses describe how the ratings were constructed, not real in-ring performance.
- 145 ratings are estimated for short appearances; the Moneyball tab excludes them by default.
- WCI rewards uniformly low profiles, so interpret it alongside Overall (the dashboard uses an Overall 80+ filter).
- Archetype labels are assigned by matching to the proposed definitions; the data supports weak, overlapping styles.

## Next step: from ratings database to research dataset

Add AEW debut and last appearance dates, matches, wins and losses, win percentage, title and world title wins, PPV matches and wins, average match rating, years active, primary style and rating confidence. That allows the real question: do the skills that make someone highly rated translate into success in AEW?

## Dashboard tabs

Drivers, Archetypes, Completeness, Moneyball, Specialists, Wrestler Finder (search, radar chart, percentiles).
