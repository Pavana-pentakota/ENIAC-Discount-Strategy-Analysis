# ENIAC Discount Strategy (Phase 2)

Analysis of Eniac's discounting, seasonality, and product pricing — Python (pandas, seaborn) analysis and recommendations on discount depth, category mix, and campaign timing

> 📌 **Part 2.** This project continues from **Phase 1: ENIAC Brazilian expansion with Magist**
> - [[GitHub repository](PHASE-1-GITHUB-REPO-LINK)](https://github.com/Pavana-pentakota/ENIAC-Brazilian-expansion_with-Magist)
> - [[Confluence](https://pentakotapavanakumari.atlassian.net/wiki/spaces/MBC1/overview?homepageId=4292803).](https://pentakotapavanakumari.atlassian.net/wiki/spaces/MBC1/overview?homepageId=4292803)
> - Phase 1 asked *where and how to sell*. Phase 2 asks *how to price and promote*.



## Business Context

- Phase 1 evaluated whether Magist is a suitable fulfillment partner for Eniac's Brazil expansion.
- Phase 2 moves from market entry to pricing: how Eniac uses discounts today, and whether that approach supports revenue.
- Two concerns drove this analysis:
  - i. Are discounts applied consistently and at sensible levels?
  - ii. Do discounts and campaigns line up with when customers actually buy?

## Business Questions Answered

- How many products are being discounted?
- How large are the offered discounts as a percentage of product prices?
- How do seasonality and special dates (Christmas, Black Friday) affect sales?
- How should products be classified into categories to simplify reports and analysis?
- What is the distribution of product prices across categories?
- How could data collection be improved?

## Dataset & Sources

- Source: Eniac order data — four datasets (orderlines, orders, products, brands), prepared as part of the WBS Coding School Eniac–Magist business case
- Period covered: January 2017 – March 2018
- Final analysis file: `Orderlines_ENIAC_Project.ipynb`
- Key Features: unit price, price paid, discount size, order date, order value, product SKU, product category

## Challenges & How I Handled Them

| Challenge | How it was handled |
|---|---|
| **Corrupted `promo_price` column:** 36,169 rows had malformed prices (e.g. "1.137.99", double decimal points) | Column was unusable, so it was dropped (on the instructor's guidance) and discount size was calculated from the other price fields instead |
| **Duplicate rows:** the products table contained 8,746 duplicates | Removed before analysis so SKU counts and category counts were not inflated |
| **Missing values:** 46 prices, 50 product types, 7 descriptions | Identified and handled during cleaning; small individually, but easy to miss without routine checks |
| **Dates stored as text** | Converted to true datetime before the seasonality analysis |
| **Inconsistent column names across tables** (e.g. `order_id` vs `id_order`) | Manually remapped so the tables could be joined |
| **Product categorization:** no category field existed, and the `other` category was disproportionately large after re-running the classifier because it already held leftover `'other'` values | Built a rule-based (regex) categorizer, replaced the stale `'other'` values with `other_electronics`, and validated the category distribution before plotting |
| **Multi-category products:** some products match several categories | Each product is assigned to its first matched category. This is a simplification, so category counts are approximate |

## Key Findings & Results

- **Discount coverage:** 92.9% of products are discounted (6,313 of 6,798 SKUs); only 7.1% (485) are not.
- **Discount size:** the median discount is 17.5% of price. Most discounts are modest promotional nudges, with a long tail of steeper markdowns (up to ~100%) that may be clearance or liquidation items and are worth reviewing separately.
- **Seasonality:** monthly revenue sits at roughly €4–6M for most of 2017, then jumps to about €20M around November 2017 (Black Friday) and stays high through December (Christmas) before falling back in early 2018. Inventory and staffing should be planned around this window rather than spread evenly across the year.
- **Discounts vs. revenue:** budget items receive the highest discounts yet generate the least revenue; high-end items are barely discounted despite producing about half of the revenue.
- **Product categories:** the largest categories by number of unique products are case_cover, other electronics, storage, and charger_cable.
- **Price distribution:** price ranges vary enormously by category — from screen protectors, cables, and cases at a few tens of euros to tablets, smartphones, and networking gear in the hundreds — which is useful for pricing strategy and category-level margin analysis.
- **Data collection:** standardize number formatting, validate price fields on entry, store dates as true datetime, align column names across systems, enforce uniqueness constraints, and audit fields regularly for corruption.
- **Recommendation:** continue discounting, but with a more disciplined strategy — cap discounts at 20%, rebalance discounts toward high-end products and away from budget items, and plan campaigns around the calendar (Black Friday and the holidays).

## Visualisations

- ![Discounted vs non-discounted products](screenshots/discounted_products.png)
- ![Distribution of discount size](screenshots/discount_distribution.png)
- ![Monthly revenue with seasonal peaks](screenshots/monthly_revenue_seasonality.png)
- ![Top 15 product categories](screenshots/top_categories.png)
- ![Price distribution by category](screenshots/price_by_category.png)

## Tools Used

- Python — pandas for data cleaning and analysis; matplotlib and seaborn for visualisation
- Google Colab — notebook environment
- Regular expressions — rule-based product categorization
- PowerPoint — final presentation of findings and recommendations

## Project Structure

```
eniac-discount-strategy/
├── README.md
├── data/
│   └── brands.csv
│   └── orderlines.csv
│   └── orders.csv
│   └── products.csv
├── notebooks/
│   └── Orderlines_ENIAC_Project.ipynb
├── presentation/
│   └── Eniac_discount_Strategy.pptx
└── screenshots/
    ├── Discounted vs non-discounted products.png
    ├── Distribution of discount size.png
    ├── Monthly revenue with seasonal peaks.png
    ├── Top 15 product categories.png
    └── Price distribution by category.png
```

## How to Use This Project

- Download this repository.
- To explore the analysis: open the notebook in `notebooks/` in Jupyter or Google Colab.
- The cleaned dataset is in the `data/` folder if you'd like to explore it directly.

## Next Steps: Testing the Discount Message with A/B Testing

The analysis above shows *how much* to discount and *when*. The natural next question is *how to present* deals so customers respond. Eniac tested this on its homepage with an A/B test on the call-to-action (CTA) button.

**Experiment setup**
- Four button variants were compared: white "SHOP NOW", red "SHOP NOW", white "SEE DEALS", and red "SEE DEALS".
- Each variant was shown to a share of homepage visitors, and clicks were tracked (data: `eniac_a.csv`, `eniac_b.csv`, `eniac_c.csv`, `eniac_d.csv`).

**Approach**
- Compared click-through rates across the four variants.
- Used a chi-square test to check whether the differences in clicks were statistically significant or just random variation.

**Why it follows from this project**
- Phase 2 recommends a more disciplined discount strategy: capped discounts and campaigns planned around the calendar. The "SEE DEALS" versus "SHOP NOW" wording tests whether promotion-focused messaging drives more engagement than neutral messaging, and the colour comparison tests whether button design matters.
- Together, the two analyses connect pricing decisions (how much and when) with customer behaviour (how they respond).

**Continued in the A/B testing project:** https://github.com/Pavana-pentakota/Eniac-A-B-Testing 

## Author
Pavana Pentakota | www.linkedin.com/in/pavanapentakota
