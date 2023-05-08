1. SavingsPlanNegation query ^efc1a4
 ```json
{
"sqlStatement" : 
 " SELECT
    timeInterval_Month AS Month,
    \"AWS-Cost-Summary-Description\" as event,
    SUM(CAST(effective_cost AS double)) as effective_cost,
    sum(CAST(usage_quantity AS double)) as usage_quantity,
    \"AWS-Service-Category\" AS AWS_Service_Category,
    \"AWS-Pricing-Unit\" as pricing_unit,
    \"AWS-Regions\" AS AWS_Regions,
    Services.\"ServicesDescription\" AS Services_ServicesDescription,
    Region.\"RegionDescription\" AS Region_RegionDescription,
    Services.\"Services\" AS Services_Services,
    Services.\"ServiceItemDirectCharge\" AS Services_ServiceItemDirectCharge,
    Services.\"ServiceItemDescription\" AS Services_ServiceItemDescription,
    case
        when \"AWS-PublicOnDemandRate\" = 'unknown' then null
        else CAST(\"AWS-PublicOnDemandRate\" as double)
    end as rate
FROM
    AWS_COST_HISTORY
WHERE
    (\"AWS-Cost-Summary-Description\" LIKE '%Negation%') and effective_cost != 0
GROUP BY
    timeInterval_Month,
    \"AWS-Regions\",
    Region.\"RegionDescription\",
    Services.\"ServicesDescription\",
    Services.\"Services\",
    Services.\"ServiceItemDirectCharge\",
    Services.\"ServiceItemDescription\",
    \"AWS-Service-Category\",
    \"AWS-PublicOnDemandRate\",
    \"AWS-Cost-Summary-Description\",
    \"AWS-Pricing-Unit\" ",

"dataGranularity" : "MONTHLY" ,
"timeRange" : {
  "last": 1,
  "excludeCurrent": true
} ,
"limit" : -1
}
``` 
2. 