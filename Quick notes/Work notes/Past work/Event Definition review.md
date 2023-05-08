- For `Tax` line items(rephrasing needed), should be grouped and have one line item which just say this amount should be charged.
	- Can be singular line items
	- We can ignore usage amount for `Tax/Edp/Credit/Refund/Fee`
- For `EdpDiscount` line items(rephrasing needed), should be one grouped rate irrespective of region and product types, if required we can have one line item or one line item per product (TBD)
- For `Credit` line items, try to use `cht_edp/line_item_description -> lineItem/lineItemDescription 
	- Don't need to find `CHANGE_REASON` with `credits`
	- We also need to rephrase the credit line items
	- Credit of type `lineItem/lineItemDescription`value
- Look for `MarketPlace` line items definition ❓
- For `Refund/Fee` line items(rephrasing needed)
	- A`Refund/Fee`(lineItem/lineItemType`value) of type `lineItem/lineItemDescription`value
- For `SavingsPlanNegation` line items, shouldn't be populated `CostSummaryDescription`
	- Can be removed by `CCS` service using `effective_cost != 0`
	- E.g query ![[CCS FR query#^efc1a4]]
- For `SavingsPlanRecurringFee` line items, shouldn't be populated `CostSummaryDescription
	- We can use for it finding out start/end of purchase etc.
	- Can't be avoided with `effective_cost != 0` 
	- Example line item:
	  ![[Pasted image 20231016154703.png]]
- For `DiscountedUsage/SavingsPlanCoveredUsage/Usage`, we need look into for phrasing being more descriptory (No View Optimization for anything other than this)


### RI/Savings Plan discussion
- Quantitative aspect for RIs
- How can I get most amount of quantitative for RI/Savings Plan
- If something expired how it'll impact the costs
- An RI of "this" type has expired/bought -> How do we quantify it? How do we associate cost with it/Can we say if it's 100 NFUs big/3 instances big.
- Expiration time for RI can be obtained using reservationStartTime/reservationEndTime
- We can use RISummaryLineItem that can give monthly amortized cost
- usage and summary(new) column
- 