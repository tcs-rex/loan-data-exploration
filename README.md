# Exploring Prosper Loan Data

## **Dataset**
### Overview
> The Prosper Loan Dataset is a large master dataset with approx. 114k rows and 81 columns. The data spans an approximately ~8-year period (11-2005 to 03-2014). The dataset was fairly clean and tidy, however some unnecessary columns and rows with NaNs were dropped; once the dataset was cleaned the historical subset dataframe was created (approx. 55k rows by 38 columns).

### Dataset Summary
>* A single large master dataset with ~114k rows by 81 columns.
>* Data from ~8yr period (Nov-2005 to Mar-2014).
>* Various dtypes (i.e. floats, ints, objects/strings, bools).
>* There are a numerous categorical dtype candidates (e.g. CreditGrade, ProsperRating, ProsperScore, LoanStatus, etc.).
>* Quality: The data was fairly clean with the exception of some NaNs and some dtype conversions needed.
>* Tidiness: The data was fairly tidy, with the exception of a minor number of duplicate elements in the dataframe.

### Data Wrangling
>Data wrangling steps taken to help generate meaningful and insightful visualizations and observations:
>* Reviewed dataset options, selected Prosper Loan Data and reviewed variable definitions.
>* Gathered (loaded dataset).
>* Initial assessment/exploration, initial cleaning/conditioning (e.g. descriptive stats, checking uniques, counts, duplicates, nulls; addressing as needed.)
>* Developed a preliminary plan/focus (e.g. performance related variables, and risk/concern related variables).
>* Iterative re-assessment and accessing/massaging/cleaning/featuring data in support of the visualization efforts (e.g. slicing, copying, sorting, pivoting, transposing, reindexing, querying, assigning, converting dtypes, etc.)

### Investigation Overview

> The investigation was focused on gaining insights on loan performance and risk-related variables, as well as explore potential correlations. To accomplish this, the exploratory work focused on a historical subset of loans; loans that have been successfully "Completed" (i.e. good loans) as well as "Chargedoff" loans (i.e. bad loans). Within the confines of this subset, Lender Yield (perf.), Income Ranges (risk), Loan Categories (risk), and influence of time (considering the 2007-2008 Great Recession) were investigated.


## **Summary of Findings**
>This section is sub-divided into the 3 visualization categories, and by the variables/features plotted.

### Univariate Visualization
#### LoanStatus
>* The bulk of the data have "Current", "Completed", "Chargedoff", or "Defaulted" statuses.
Roughly ~10% and ~33% fall into the "Chargedoff" and "Completed" buckets (i.e. collectively they amount to roughly ~43-44% of the master (unmodified) dataset.
>* The percent "Completed" to "Chargedoff" is positive (i.e. roughly ~75% of loans were paid vs ~25% were charged-off), but I believe these percentages would conventionally/historically be much further removed from one another...perhaps the data is skewed more heavily toward charge-off than normal because the dataset spans the financial crisis of 2007-2008? (When we dig into the data a bit more later, maybe we can gain some confidence in this hypothesis.)
>* Past due statuses account for only fractional amounts of the current loans (which is good news for Prosper, and what I believe one would expect to observe.)

#### LenderYield
>* The data appears to be right-skewed.
>* The smaller bins sizes in the 2nd plot reveal significant spikes near the right end of the distribution (in the range of 0.3 to 0.35 range).

#### IncomeRange
>* The most prevalent income range was the $25k to \$49,999 bin, with higher income earners not actively borrowing as much comparatively.
>* There appears to have been loans given even when there was no employment or no income (and there appears to be a large number in the "Not Displayed" bin, which might be similar to the former 2 bins (no employment/no income) in terms of risk.

#### ListingCategory
>* There are a wide variety of loan categories with a wide variance in terms of frequency (i.e. count).
>* The top 5 largest loan categories are "Debt Consolidation", "Not Available", "Other", "Business", and "Home Improvement", with the top 2 categories more than doubling the third highest category.
>* It seems odd that so many loans fell into "Other" and "Not Available".
Perhaps the "Debt Consolidation" loans stemmed from the Financial Crisis ("Great Recession").

#### LoanOriginalAmount
>* The data is very skewed to the right and looks like a good candidate for utilizing a log transformation (to yield a more balanced distribution / better model).
>* The bulk of Prosper loans in the subset of interest ("Completed" + "Chargedoff") are at the low end in the $1,000 to \$6,000 range.
>* Post log-transform, the data is now roughly normally distributed.
>* The bulk of loans in the subset are still shown at the low end in the $1,000 to \$6,000 range (as expected for a correct transformation and axis rescaling)

### Bivariate Visualizations
#### scatter_matrix (high-level scan with select data)
>* 'LenderYield' may have a slight negative correlation with 'OpenRevolvingAccounts'; perhaps a little stronger negative correlation with 'LoanOriginalAmount'; and possibly a strong negative correlation with 'StatedMonthlyIncome' (or no correlation at all) but it is difficult to tell in this plot.
>* 'OpenRevolvingAccounts' may also have a strong negative correlation with 'StatedMonthlyIncome' (or no correlation at all) and possibly a small negative correlation with 'LoanOriginalAmount'.
Due to the presence of outliers the resultant axis scale makes it very difficult to read some of the 'StatedMonthlyIncome' plots - these may be investigate later as individual bivariate plots.

#### StatedMonthlyIncome
>* There were significant outliers, which had a material effect on the scatter plot/regression line. (If the outliers weren't discounted, there would have appeared to be a potential negative correlation. After the outliers and plot scaling were addressed, we can see that a potential correlation does not appear to exist.
>* The outlier threshold (i.e. max whisker) of ~$11k was lower than expected.

#### LoanOriginalAmount, ListingCreationDate (Quarterly Aggregation)
>* There have been huge fluctuations in the amount of loans/borrowing in the above time period (~8years).
>* The distribution looks bi-modal with 2 distinct upward rises each followed by even steeper declines in the amount of loans/borrowing.
>* The distribution depicts a boom and bust cyclical scenario (at least for these 8 years or so)

#### LoanOriginalAmount, LenderYield, ListingCreationDate (Quarterly Aggregations)
>* Both variables appear to somewhat correlated to one another, e.g. as one variable goes up the other variable sometimes follows, however, there are some leads/lags and chatter indicating the variables may not be strongly correlated.
>* Also, interestingly the 'LenderYield' did not take as much of a hit relative to the severe downturn in loans/borrowing...While it did dip around 2009, it didn't dip much before resuming its upward trajectory.
>* The 'LenderYield' ended the charted period well above were it began, however in contrast the amount of loans/borrowing ('LoanOriginalAmounts') ended the period almost where it began.

#### LenderYield, LoanOriginalAmount
>* With ~50k points in the first plot, over-plotting was an issue - it was mostly addressed using sampling as can be seen in the 2nd figure (directly above), but jitter and transparency also helped in clarifying the plot.
>* Overall there does appear to be a potential small negative correlation between 'LoanOriginalAmount' and 'LenderYield' (i.e. as 'LoanOriginalAmount' increases, we might expect that 'LenderYield would decrease). This potential correlation could be explained by smaller interest rates being charged on larger loans for example.
>* Even with the over-plotting addressed we can still see vertical banding in the data around round whole numbers. This might be explained by loans for round-numbered items (e.g. a boat or car costing $5,000) or lines of credit/loans that have a round value (e.g. \$10,000 personal loan).
>* There is significant variance in the data (which might be expected since 'LenderYield' and interest rates in general fluctuate from quarter-to-quarter and year-to-year, and even more importantly every loan applicant's financial situation (and therefore risk to the lender) is different).
>* There are some outliers but not many relative to the overall sample so there effects should be minimal.

### Multivariate Visualizations
#### LenderYield, LoanOriginalAmount, ListingCategory (feature 'LoanCat')
>* This plot is not a good explanatory plot. However, the plot does indicate that regardless of what type/category the loan was, the LenderYield vs. LoanOriginalAmount appears to be negatively correlated.
>* However, the differences in the best fit line slopes do indicate that each loan category may have some small differing influence on the 'LenderYield' vs. 'LoanOriginalAmount' (Note: each category would need to be looked at separately, as there are various counts in each).
>* Additional outliers are visible (since the full dataset is being used and not a random sample), but based on the relative number of these they will not be removed as they will not have a significant impact on the results.
>* The bulk of the scatter plot data appears to fall within a box delineated by approximately ~0.04 to ~0.34 in term of "LenderYield", and about ~1,000 to ~25,000. (Perhaps these are the limits of their typical loan offerings).
>* And the highest density in the scatter plot exists roughly between $1,000 and \$6000 as we have seen before.

#### IncomeRange, LoanStatus, ListingCreationDate (feature 'crisis')
>* There are definitely differences between the counts in the various categories, however, in an effort to make deltas easier to see/compare, another clustered bar chart will be plotted using a differences/deltas feature.

#### IncomeRange, LoanStatus, ListingCreationDate (features 'crisis', 'Delta')
>* The upper portion of the plot likely represents the riskier group of loans since the borrower didn't report or didn't have income, or had low income...This being said, we see a major improvement in these Income Range categories (e.g. for "Completed" post-crisis there were ~4,600 fewer income "Not Displayed" loans and ~700 fewer loans in the $0 and "Not employed" categories).
>* From the lower portion of the chart we see that post-crisis we have significantly more borrowing that falls into the higher income ranges.
>* Also of note, is that the overall total of "Chargedoff" (i.e. bad) loans is significantly less post-crisis.
>* So overall, it appears that this lender has really improved its application/loaning practices post crisis.
Other Notes:
>* one surprising observation was that the "Not employed" income range category actually went up for both LoanStatus types post-crisis...perhaps some folks had assets they could borrow against.

#### LoanStatus, ListingCategory, ListingCreationDate (features 'LoanCat', 'crisis')
>* Post-crisis there were zero (0) ambiguous "Not Available" loans listed.
>* Post-crisis there was a dramatic increase in 'Debt Consolidation" loans (which is to be expected based on job and business losses which resulted because of the financial crisis/crash).
>* Surprising, was that another ambiguous loan listing category "Other" went up post-crisis (I would have thought it would have went down due to stricter lending and better reporting).
>* The number of 'Chargedoff' loans were less post crisis.
>* So again overall, lending practices (i.e. more explanatory/accurate application loan data reporting) improved post crisis.


## **Key Insights for Presentation**

### Slide 1
#### Investigation Overview
> The investigation was focused on gaining insights on loan performance and risk-related variables, as well as explore potential correlations. To accomplish this, the exploratory work focused on a historical subset of loans; loans that have been successfully "Completed" (i.e. good loans) as well as "Chargedoff" loans (i.e. bad loans). Within the confines of this subset, Lender Yield (perf.), Income Ranges (risk), Loan Categories (risk), and influence of time (considering the 2007-2008 Great Recession) were investigated.

#### Dataset Overview
> The Prosper Loan Dataset is a large master dataset with approx. 114k rows and 81 columns. The data spans an approximately ~8-year period (11-2005 to 03-2014). The dataset was fairly clean and tidy, however some unnecessary columns and rows with NaNs were dropped; once the dataset was cleaned the historical subset dataframe was created (approx. 55k rows by 38 columns).

### Slide 2
#### Loan Status Distribution
>Roughly ~10% and ~33% fall into the "Chargedoff" and "Completed" buckets (i.e. collectively they amount to roughly ~43-44% of the master (unmodified) dataset, which is more than ample for an investigation.

### Slide 3
#### Quarterly Aggregate (Original) Loan Amounts & Average Lender Yield
> The main observation from this plot is the severe downturn in loans/borrowing brought on by the Great Recession 2007-2008. Some other effects of the crisis can be seen in later plots as well.
>
>A couple of additional interesting observations here are that the Lender Yield variable stayed fairly resilient through the rough financial patch (perhaps they had a lot of fixed rate loans on their books); and, it actually ended the charted-period well above were it began (in contrast to the Loans Amount variable which ended the period almost where it began).

### Slide 4
#### Income Ranges by Loan Status (Pre/Post-Crisis)
> This faceted bar plot really illustrates how lending practices were not as strict prior to the crash, and it highlights the potential risks - there are stark differences between the facets, for example:
>* Pre-crisis there were about 6,000 'Not Displayed" loans on the books (i.e. no income recorded...this seems risky!)
>* Post-crisis you can see that there are zero (0) "Not Displayed" instances, and the 3 other low/no-income bins are fairly small (and at the same time the bars representing the higher incomes are much larger, especially the "Complete" (i.e. successful) loans).
>* So it appears that the lender has really improved its application/loaning practices post crisis.

### Slide 5
#### Top 3 Loan Listing Categories (Pre/Post-Crisis) with Loan Status
>Key observations from the plot include:
>* Post-crisis there were zero (0) ambiguous "Not Available" loans listed.
>* Post-crisis there was a dramatic increase in 'Debt Consolidation" loans (which seems understandable based on the financial crash).
>* The number of 'Chargedoff" loans were less post crisis.
>* Again, overall lending practices improved post crisis, thereby reducing risk and improving investment prospects.


### Post-Exploration Design Changes
>* Improved labels (size, weight)
>* Added/modified legend
>* Removed some unnecessary chart ink (like labels that are redundant)


## **References**

>[1] Udacity Communicate Data Findings - Dataset Options (https://video.udacity-data.com/topher/2019/April/5ca78b26_dataset-project-communicate-data-findings/dataset-project-communicate-data-findings.pdf)
>
>[2] Prosper website (https://www.prosper.com/about)
>
>[3] Udacity exploration and slide deck templates.
