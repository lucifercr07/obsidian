1. `you should avoid defining new scalars and reuse where possible.` ✅
	- Change:
		- `finOpsEventDefinition: String!` -> `finOpsEventDefinition: LocalizedString!`
		- Make it same at other places where `String` is used in schema

2. `Also you should add @auth directive if authorization is being done at GraphQL layer.` 
	- Change:
		- Add `@auth` at `finOpsQuery: FinOpsQueries! @featureStability(value: ALPHA, description: "FinOps Queries")`?

3. `in which language is this? How will you localize?` ✅
	- Change:
		- Use `LocalizedString` everywhere `String` is used

4. `note that ids are indeed a part of the Node contract and so their values must be Base64-encoded strings of format <GraphQLType>:<instance-id> there's a NodeHelper class to help encode/decode these values.` ✅
	- Change:
		- Use the helper in `ensemble-graphql-commons` for generating and decoding 'id' field values which are base64 encodings of the pattern `<GraphQLType>:<instanceId>`

5. `units?` ❓
	- Change:
		- Add units in the comments where ever `Float` is used
		- Should `Float` be defined as scalar in `common_scalars.graphqls`?
		- Currency fields corresponds to currency
		- 

6. `replace with use of common QuerySortOrder type` ✅
	- Change:
		- Use `QuerySortOrder` from `common_filter_sort.graphqls` file

7. `pls consider using QueryFilter even if not all filters are supported (document that OR, NOT aren't supported for example).` ❓
	- Change:
		- Use `QueryFilterOperator` from `common_filter_sort.graphqls` file and put in comment what all is not required/used by CCS
		- Most of filter operators are available following will need to change:
			- `MATCHES` can be replaced with `CONTAINS` from `QueryFilterOperator` type.
			- `NE` can be replaced with `NEQ` from `QueryFilterOperator` type.
			- `NOT_MATCHES` can't find a similar thing in `QueryFilterOperator` type.
			- `NOT_NULL` replace with `ISNOTNULL`
			- `NULL` replace with `ISNULL`
		- (`or, not`) are not supported for embedded query filters and `STARTSWITH, ENDSWITH` filters are not supported as filter operators. We handle it in `ensemble-cost` and throw error if these filters found in query ✅

8. `can we avoid abbreviations like RI in the public APIs? Not sure everyone will understand... I guess this is RESERVED_INSTANCE? Ditto for SPOT_INSTANCE` ❓
	- Change:
		- `RI_FLOAT` change reason to `RESERVED_INSTANCE_FLOAT` and update the comments accordingly
		- `SPOT_INSTANCE` ??

9. `cut 'n' paste bug` ✅
	- Change:
		- Update comments section in `SP_FLOAT` change reason

10. `please consider using common types defined in ensemble-graphql-common for filtering and sorting where possible`
	- Change:
		- Update at `sortRules: [FinOpsSortRuleInput!]): FinOpsCloudCostSmartSummaryEventConnection @featureStability(value: ALPHA)` and check if any other place we're using the Sort/Filter in schema ✅
		- `QuerySortOrder` should we modify schema on CCS service side or need to do mappings in `ensemble-cost` service ✅

11. `niggle maybe but does increased usage always imply increased cost?` ❓
	- Change:
		- Update comments for change reason `INCREASED_USAGE`

12. `Documentation e.g. something like Financial Operations queries, powered by Tanzu CloudHealth` ❓
	- Change:
		- Add documentation for this line `finOpsQuery: FinOpsQueries! @featureStability(value: ALPHA, description: "FinOps Queries")`

13. `basically all list values should have a '!' on their content type since null values in lists are typically never used.`
	- Change:
		- Update `finOpsReason: [FinOpsCloudCostSmartSummaryChangeReason]` to `finOpsReason: [FinOpsCloudCostSmartSummaryChangeReason!]`
		- Update `finOpsAdditionalDimensions: [FinOpsCloudCostSmartSummaryDimension]!` to `finOpsAdditionalDimensions: [FinOpsCloudCostSmartSummaryDimension!]!` ❓
		- `edges: [FinOpsCloudCostSmartSummaryEventEdge!]` looks correct

14. `If these are timestamps - all timestamps should use DateTime type and have field names xxxTime e.g. startTime, endTime. If they are not timestamps, need to specify what they are and what format the string has.` ❓
	- Change:
		- Update `endTimePeriod`, `startTimePeriod` with comments stating "`yyyy-MM` formatted time period in String?"

15. `niggle - put in order: day, week, month` ✅
	- Change:
		- Update order in `FinOpsCloudCostSmartSummaryGranularity`

16. `you seem to have a lot of 'dimension' types across the graph that all have name/value or similar, is there way of fusioning them? For what it's worth there's a common 'Tag' type with key/value strings and a TagInput type for inputs, not sure if this would serve the same purpose.` ❓
	- Change:
		- Use `Tag` or `TagInput` from `common_tag.graphqls`?

17. `all measures need units to be specified. Are these USD? Or are they dependent on something? Should currency be a field in the type?` ❓
	- Change:
		- Define a type as `Currency` ?
		- Update comments with what values can be there for the `Currency` unit e.g. `USD, EUR, INR etc.`
		- Comment should change to something like:
		  `Consumption Cost at the start period of the comparison. A non-negative number; precision unspecified.` ?