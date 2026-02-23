#E-Commerce Transaction Data Cleaning & Quality Improvement Project

##Project Overview

Olist is a Brazilian e-commerce marketplace that connects small and medium-sized merchants with major online retailers across Brazil. Operating across diverse product categories, payment methods, and geographic regions, the platform processes tens of thousands of transactions monthly, generating substantial revenue through credit card, boleto (Brazilian payment voucher), voucher, and debit card payment systems.

This project focused on a dataset of 99,540 e-commerce transactions spanning order placement, payment processing, fulfillment logistics, and final delivery. The raw data exhibited significant quality issues including duplicate records (90 instances), inconsistent text formatting across categorical fields, missing payment information in 81 orders, and invalid transaction entries with zero-value payments. These deficiencies threatened the reliability of any downstream business analysis, particularly revenue reporting, customer behavior segmentation, and operational performance tracking.

By applying systematic data cleaning methodologies in Microsoft Excel, this project transformed unreliable raw data into a production-ready dataset of 99,367 clean, validated records. The cleaning process removed 173 problematic entries (0.17% of the dataset) while preserving all legitimate transactions. The refined dataset now supports accurate financial reporting, delivery performance analysis, payment method optimization, and data-driven decision-making across the e-commerce operation.



##Tools & Methodology

The entire data cleaning workflow was executed in Microsoft Excel, leveraging a combination of formulas, built-in data validation tools, filtering mechanisms, and systematic quality checks to ensure both efficiency and thoroughness.

The dataset consisted of 99,540 rows and 12 columns, capturing critical transactional elements including order identifiers, customer details, order status classifications, timestamps across the complete order lifecycle (purchase, payment approval, carrier pickup, customer delivery), estimated delivery dates, payment sequence information, payment method selection, installment structure, and final transaction value in Brazilian Real.



##Pre-Cleaning Data Profiling

Prior to executing any cleaning operations, a comprehensive data quality assessment was conducted to map the full landscape of issues. This profiling exercise revealed that 24% of records (23,995 entries) had improperly formatted zip codes due to leading zeros being dropped during data import—a common issue when numeric fields are stored without proper formatting constraints. Additionally, 90 duplicate orders were identified, representing potential double-counting in revenue calculations. Payment data was entirely absent for 81 orders, rendering them unusable for financial analysis. Three orders displayed "not_defined" as the payment method, and three additional orders recorded zero-value payments—both categories representing data entry failures or system errors.

Text standardization issues were pervasive: the payment_type field contained the same value written in multiple formats ("credit_card", "CREDIT CARD", "Credit_Card", "creditcard", "credit card"), making aggregation and categorical analysis unreliable without correction.

Each quality issue was then addressed through a targeted intervention designed to preserve data integrity while removing ambiguity. Changes were implemented sequentially, with validation checks performed after each step to prevent cascading errors.



##Data Cleaning Process & Findings

1. Text Standardization Across Categorical Fields

Issue Identified:

Payment method and order status fields exhibited inconsistent capitalization, spacing, and delimiter usage. For example, credit card payments appeared as "credit_card" (standard format), "CREDIT CARD" (all caps), "Credit_Card" (mixed case), "creditcard" (no delimiter), and "credit card" (space instead of underscore). This variability prevented accurate grouping and made any analysis of payment preferences unreliable.

#Cleaning Method

The TRIM and LOWER functions were applied to all categorical text fields to enforce uniform lowercase formatting and remove extraneous whitespace. A new column was created alongside each original field, populated with the formula =TRIM(LOWER(original_cell)). Once validated, the cleaned values were copied and pasted as static text, replacing the original inconsistent data.

Residual formatting issues that survived the initial formula application—specifically 1,531 instances of "creditcard" (no underscore) and 59 instances of "debit card" (space instead of underscore)—were corrected using Excel's Find & Replace function with exact match criteria to avoid unintended replacements.


#Business Impact:

This standardization ensures that payment method analysis accurately reflects customer behavior. Without this correction, a summary of credit card usage would have fragmented the same payment type across five separate categories, artificially deflating the prominence of credit card transactions and potentially misleading marketing or partnership strategies with payment processors.

##2. Removal of Duplicate Transactionsppp

Issue Identified

Ninety records were exact duplicates across all 12 columns, suggesting either system errors during data export or duplicate order submissions that were not properly flagged in the source database.

#Cleaning Method

Excel's Remove Duplicates tool was applied to the full dataset with all columns selected as comparison criteria. This ensured that only perfect duplicates—rows identical in every field—were removed, while similar but legitimately distinct orders (such as repeat purchases by the same customer) were preserved.


Result:

Ninety duplicate rows eliminated.

Business Impact:

Duplicate removal prevents revenue double-counting, which would have inflated financial performance metrics by 0.09%. In a high-volume e-commerce environment, even small percentage errors compound into significant misrepresentations of business health. Removing these duplicates ensures that order counts, revenue totals, and customer transaction frequencies reflect actual business activity.

#3. Elimination of Incomplete and Invalid Payment Records

Issue Identified:

Eighty-one orders existed in the dataset with completely missing payment information—no payment type, no payment value, no installment data, and no sequence information. These records could not be analyzed for revenue contribution, payment preferences, or financial reconciliation purposes.

Additionally, six orders contained structurally invalid payment data: three had "not_defined" listed as the payment method (a placeholder value indicating a system failure), and three recorded payment values of exactly zero, which is logically inconsistent with completed e-commerce transactions.

Cleaning Method:

Excel's filtering functionality was used to isolate rows where payment_type was blank. All 81 flagged rows were selected via row numbers and deleted in a single operation. The same filtering approach was applied to identify and remove the three "not_defined" payment entries and the three zero-value transactions.

Result

Eighty-seven problematic payment records removed (81 blank + 3 undefined + 3 zero-value).

Business Impact

Orders without payment data contribute no value to financial reporting, customer segmentation, or operational analysis. Retaining them would have skewed average calculations, introduced noise into payment method trends, and complicated reconciliation between order volume and actual revenue. Their removal ensures that every record in the cleaned dataset represents a complete, analyzable business transaction.

#4. Strategic Retention of Missing Delivery Dates

Issue Identified:

A significant proportion of records exhibited missing values in delivery-related date fields: 160 orders (0.16%) had no payment approval timestamp, 1,783 orders (1.79%) had no carrier pickup date, and 2,965 orders (2.98%) had no customer delivery date.

Cleaning Decision:

These missing values were intentionally preserved rather than filled or deleted.

Business Rationale:

Missing delivery dates do not represent data quality failures—they represent orders that have not yet progressed through the corresponding stage of fulfillment. Cancelled orders will never have a delivery date. Orders still in processing will not have an approval timestamp. Orders awaiting carrier pickup will not have a pickup date. Filling these fields with placeholder values (such as "N/A" or zeros) would misrepresent the order lifecycle and make it impossible to calculate accurate metrics such as on-time delivery rates, average fulfillment times, or cancellation frequencies.

By preserving these blanks, the dataset retains the ability to distinguish between "not yet delivered" and "delivered late," which is essential for operational performance tracking.

#5. Numerical Formatting for Financial Clarity

Issue Identified

Payment values and installment counts were stored in general number format without thousands separators, decimal precision, or currency indicators, making them difficult to interpret at a glance and prone to misreading in financial reports.

Cleaning Method:

The payment_installments column was formatted as Number with zero decimal places, as installment counts are always whole integers. The payment_value column was formatted as Accounting with comma separators for thousands and two decimal places for cents, ensuring consistency with Brazilian Real (BRL) currency standards.

Business Impact:

Professional numerical formatting improves readability in dashboards and reports, reduces interpretation errors during stakeholder presentations, and ensures that currency values are displayed with appropriate precision for financial reconciliation.


#6. Outlier Identification and Retention

Issue Identified:

One transaction recorded a payment value of $13,664.08, which is 86 times higher than the dataset's average payment of $158.36. This value sits far outside the interquartile range and could be flagged as a potential data entry error.



Cleaning Decision:

The outlier was retained in the dataset without modification.

Business Rationale:

High-value transactions are not inherently invalid. In e-commerce environments, bulk purchases, B2B orders, and premium product categories legitimately generate payments that exceed typical consumer spending patterns. Removing this transaction without verification would result in the loss of $13,664 in recorded revenue—a material omission.

The outlier was flagged for stakeholder review but preserved in the cleaned dataset to ensure completeness. If subsequent investigation reveals it to be erroneous, it can be removed in a future iteration with documented justification.


##Key Quality Improvements

The cleaning process delivered measurable improvements across multiple dimensions of data quality:

Completeness: All retained records now contain valid, complete payment information, ensuring 100% usability for financial analysis.

Consistency:Categorical fields are uniformly formatted in lowercase with standardized delimiters, eliminating ambiguity in grouping and aggregation operations.

Accuracy:Duplicate records have been eliminated, preventing revenue overstatement and ensuring one-to-one correspondence between dataset rows and actual business transactions.

Validity: Invalid entries (undefined payment types, zero-value payments) have been removed, ensuring that every record represents a legitimate, analyzable transaction.

Dataset Reduction: From 99,540 initial rows to 99,367 final rows—a reduction of 173 records (0.17%)—indicating that the vast majority of the source data was salvageable and that cleaning operations were surgical rather than destructive.

##Recommendations for Continuous Data Quality Improvement

Based on the issues identified during this cleaning exercise, the following systemic improvements are recommended to prevent recurrence and reduce future cleaning overhead:

1. Implement Dropdown Constraints for Categorical Data Entry

Replace free-text input fields for payment_type and order_status with dropdown menus containing only valid, pre-formatted options (e.g., "credit_card", "boleto", "voucher", "debit_card"). This prevents users from introducing formatting variations and ensures categorical consistency at the point of data capture.

Expected Impact:Elimination of 90%+ of text formatting issues, reducing cleaning time by an estimated 2-3 hours per dataset iteration.

2. Enforce Data Validation Rules at the Database Level

Program the transaction database to reject entries that violate logical business rules:
- Payment values must be greater than zero.
- Payment type cannot be null or undefined.
- Order delivery dates cannot precede order purchase dates.

These constraints should be enforced before data is committed to storage, preventing invalid entries from entering the dataset in the first place.

Expected Impact: Complete elimination of structurally invalid records, improving data trustworthiness and reducing analyst intervention.

3. Automate Text Normalization During Data Ingestion

Apply automatic text standardization (lowercase conversion, whitespace trimming, delimiter normalization) as data is written to the database, rather than relying on post-hoc cleaning by analysts. This can be implemented via database triggers or ETL (Extract, Transform, Load) pipeline rules.

Expected Impact: Immediate consistency across all categorical fields, with zero manual effort required.

4. Implement Unique Constraints to Prevent Duplicate Submissions

Set order_id as a primary key with a uniqueness constraint in the database schema. This ensures that the system automatically rejects any attempt to insert a duplicate order, whether due to user error or system malfunction.

Expected Impact: Elimination of duplicate records at the source, preventing downstream data integrity issues.

5. Establish a Data Quality Monitoring Dashboard

Develop a real-time or near-real-time dashboard that tracks key data quality indicators:
- Percentage of orders with missing payment information (target: <0.1%)
- Count of undefined or null categorical values (target: 0)
- Rate of duplicate order submissions per day (target: 0)
- Distribution of payment values to flag unusual outliers

This dashboard should alert data stewards when quality thresholds are breached, enabling proactive intervention before issues compound.

Expected Impact: Early detection of systematic data quality degradation, reducing the volume of retroactive cleaning required.

6. Document and Standardize Cleaning Procedures

Maintain a living data quality playbook that documents:
- Acceptable value ranges for each field (e.g., payment_value: $1 - $20,000)
- Standard formatting conventions for text fields
- Business rules for handling missing data (delete vs. retain)
- Step-by-step cleaning workflows for common issues

This playbook should be accessible to all team members and updated as new edge cases are encountered.

Expected Impact: Consistency across analysts, reduced onboarding time for new team members, and institutional knowledge preservation.

##Limitations & Considerations

1. Temporal Scope

This analysis is based on a static snapshot of 99,540 transactions captured at a single point in time. Without date range context or historical comparison datasets, it is not possible to determine whether the observed data quality issues represent a worsening trend, a stable baseline, or an improvement over prior periods. Establishing trend analysis would require access to longitudinal data with consistent collection intervals.

2. External Data Dependencies

The dataset does not include cost of goods sold (COGS), shipping costs, platform fees, or refund information. As a result, the payment_value field represents gross transaction value rather than net profitability. Any business recommendations derived from this data should be interpreted as revenue-focused rather than margin-focused.

3. Assumptions in Missing Data Treatment

The decision to retain missing delivery dates assumes that these represent orders still in progress or cancelled orders. If the data extraction process itself introduced missingness (e.g., incomplete exports or API failures), this assumption would be invalid. Validation with the source system administrator is recommended to confirm that blanks represent true lifecycle states rather than extraction errors.

4. Outlier Retention Without Verification

The $13,664 outlier was retained based on the assumption that high-value transactions are plausible in an e-commerce context. However, this transaction was not verified against external records (invoices, payment processor logs, customer service notes). If further investigation reveals it to be erroneous, the dataset would require re-cleaning and any dependent analyses would need revision.

##Conclusion

This project demonstrates that the Olist e-commerce dataset, despite exhibiting multiple categories of quality issues, was fundamentally sound and required only targeted interventions rather than wholesale reconstruction. By removing 173 problematic records (0.17% of the dataset) and standardizing text formatting across categorical fields, the dataset was transformed from an unreliable raw export into a production-ready analytical asset.

The cleaned dataset now supports accurate revenue reporting ($1,603,568,042 in total gross transaction value), payment method analysis across four distinct payment types, delivery performance tracking across 99,367 completed and in-progress orders, and customer behavior segmentation. Business stakeholders can now confidently use this data to inform inventory decisions, optimize payment partnerships, and evaluate regional fulfillment performance without concern for data integrity failures.

The six systemic recommendations outlined above—dropdown constraints, database-level validation, automated normalization, duplicate prevention, quality monitoring, and procedural documentation—represent a roadmap for elevating data quality from reactive cleaning to proactive prevention. Implementing even a subset of these measures would significantly reduce future cleaning overhead, improve analyst productivity, and increase confidence in data-driven decision-making.




