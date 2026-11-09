# Submission Template: Module 4 Lab 1: pandas on a Real Dataset

Name: Daz Arnaud    Date: November 8, 2026

Fill this in as you work. Sections marked PREDICT must be written before you run the step.

---

## 0. Your unique values

- **The number of distinct data quality problems you found:** 3 (21 missing unit_price values, 13 negative shipping_cost values, order_date loaded as text)
- **Your decision and stated cost for the missing unit_price values:** Kept the 21 rows, left the price missing, and added a price_missing flag. Cost: order value and revenue skip those 21 orders, so totals are understated, most for Apparel and Electronics (7 missing each).
- **The one problem you found that is not in the troubleshooting table:** order_date is a date column that loaded as text (str). Every value parses, so it is a type problem rather than bad data.

## 1. Predictions

I did not record these predictions before running the notebook. I am leaving them unclaimed rather than presenting a later guess as a prior prediction.

### Step 1: how many distinct data quality problems do you think are in this file?

**My prediction (written before running):**

> No prediction was recorded before the run.

**What actually happened:**

> I found 3 problems: 21 missing unit_price values, 13 negative shipping_cost values, and order_date stored as text.

**Why the difference, or why I was right:**

> I cannot compare this result with a prediction I did not record.

### Step 5: will dropping the missing unit_price rows change the mean of any other column by more than one percent?

**My prediction (written before running):**

> No prediction was recorded before the run.

**What actually happened:**

> No. Mean quantity went from 5.088 to 5.079 (-0.17%) and mean shipping_cost went from 12.180 to 12.209 (+0.24%). Both are under one percent.

**Why the difference, or why I was right:**

> I cannot compare this result with a prediction I did not record.

## 2. Break it on purpose

### Break 1: Compute the mean of shipping_cost before removing the negative values, then after.

**Exact error or behavior I observed:**

```
Before: 12.179559999999999
After : 12.817782340862424
```

**What this tells me about how it works:**

> No error is raised. The 13 negative values quietly pulled the average down by about 64 cents per order. pandas averages whatever numbers are present, so impossible values have to be found and removed by me.

### Break 2: Fill the missing unit_price values with zero instead of your chosen approach, then recompute average order value by category.

**Exact error or behavior I observed:**

```
             zero_filled  my_cleaned  difference
category                                        
Apparel           812.02      873.14      -61.12
Electronics       853.49      918.43      -64.94
Grocery           910.04      929.40      -19.36
Home             1006.92     1037.13      -30.21
Toys             1045.05     1065.95      -20.90
```

**What this tells me about how it works:**

> Again no error. Filling with zero treats the 21 orders as free, so every category's average drops. Apparel and Electronics drop the most because they have 7 missing prices each.

### Break 3: Convert the order_date column to datetime with pd.to_datetime and errors='raise'.

**Exact error or behavior I observed:**

```
datetime64[us] 2026-01-01 00:00:00 2026-10-28 00:00:00
```

**What this tells me about how it works:**

> No error. Every date in this file is valid ISO format, so errors='raise' had nothing to raise on. The dates run from January 1 to October 28, 2026.

## 3. Results

**Your column by column profile notes**

> order_id: 500 unique, no duplicates. order_date: text, all values parse as dates. category: 5 values, 96 to 103 rows each. region: 4 values. quantity: 1 to 9, no impossible values. unit_price: 21 missing, range 5.07 to 398.81. shipping_cost: no missing, but 13 negative values (minimum -22.56) and 1 exactly zero. payment_method: card, paypal, gift.

**Your three decision blocks with stated costs**

> 1. WHAT: 13 shipping_cost values are negative. DID: set them to missing. COSTS: shipping averages use 487 orders, and if they were really refunds, that information is lost.
> 2. WHAT: 21 unit_price values are missing. DID: kept the rows, left the price missing, added a price_missing flag. COSTS: order value and revenue skip those orders, so totals are understated.
> 3. WHAT: order_date loaded as text. DID: converted with pd.to_datetime. COSTS: almost none here, but on a future file a bad date would need handling.

**Your three grouped aggregation answers**

> Highest average quantity per order: Toys (5.30). Highest average shipping cost: South (13.46). Highest average order value: Toys (1065.95).

## 4. Reflection

[AI-assisted draft for review under the accommodation described by Daz. The Canvas page asks for student-authored reflection. Confirm with the instructor if this accommodation applies to these two answers.]

### 1. What does the change in average quantity tell you about the missing prices?

> The average quantity was 5.088 for all 500 orders and 5.079 after removing the 21 orders with missing prices. The decrease was about 0.17 percent. This small change gives me little evidence that the missing prices are concentrated in unusually large or small orders, but it does not prove the prices are missing at random. I kept those rows for quantity and shipping analysis. I left their order values missing instead of treating them as free orders.

### 2. What should a reader know before using the cleaned file?

> The file contains 500 orders, but 21 unit prices remain missing, so order-value and revenue summaries exclude those orders. I changed 13 negative shipping costs to missing, so shipping averages use 487 orders and no longer include those negative entries. I added a price_missing flag and converted order_date to a date type so readers can identify incomplete prices and sort dates correctly.

## 5. AI use disclosure

- **Tools used:** Claude (Anthropic) and Codex (OpenAI)
- **What I asked for:** Help writing and running the pandas code for profiling, cleaning, grouped averages and the break-it tests, and drafting the decision blocks and results notes.
- **What I used from it:** The notebook code and outputs, the decision blocks, the break-it observations and the results section.
- **Verification performed:** Codex checked that all 12 code cells have saved outputs and none has a saved error. Codex compared the template numbers with the recorded notebook output. Daz should review the wording and run the notebook once more before submission.

## 6. Checklist

- [ ] Predictions written before running. None were recorded before the run.
- [x] Actual results recorded, including the ones I got wrong
- [x] Exact error text captured for every break it item
- [x] Notebook runs top to bottom with outputs visible
- [x] PDF exported after the full run
- [ ] Reflection reviewed by Daz and confirmed within the approved accommodation.
- [ ] Files uploaded individually, no archive
