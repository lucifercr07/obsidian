- View a concise summary of all the cost generating events on the cloud
- Going broader than deeper into line items
- NOT tracked at resource level
## Reference documents:
- Implementation from Sanjna: https://gitlab.eng.vmware.com/cset/cost-management/rootcauseanalysis/-/blob/master/Execute.R
- Figma: https://www.figma.com/proto/bRWtGopyQsT98jE7g9clrm/Explore-Demo-Cloud-Smart-Summary-John-Aug-10?type=design&node-id=3323-41517&t=y90zozNNSb9575gT-1&scaling=contain&page-id=3323%3A41031&starting-point-node-id=3323%3A42789
- Requirement docs: 
	- https://onevmw.sharepoint.com/:w:/t/CHTCoreArchitects/EXzD3KPnjnJGuxL24D21eXoBwnxvP3bMAKVE1krnZk3l-g?e=jnSBrx
	- https://confluence.eng.vmware.com/display/CBO/%28Root+Cause+Analysis%29+RCA+Vision+Document
- Miro link: https://miro.com/app/board/uXjVMupAL-I=/?share_link_id=890022063499

## Implementation details:
#### 1. Supergen to generate event definitions:
- *We shall try to keep the naming conventions for Services/Categories/Regions as close to Cost history report as possible in Event definitions* 
- Use flag?? from `Customers` table to check if it's a Aria hub enabled customer and then only introduce the column via `AwsEventDefinitionColumnGenerator`
- Steps in current implementation:
	  1. Region mapping used - We will create a cache from `aws_regions` table in AssetDb and upload on S3 to refer this mapping so that our region names
	  Region cache location [here](https://s3.console.aws.amazon.com/s3/buckets/cht-faster-cubes?region=us-east-1&prefix=cubeprime/dimensional_dataset_artifacts/0/aws/aws-region/&showversions=false)
	  2. Effective cost column will be provided
	  3. Mappings done:
	     - Column `lineItem/LineItemType` : [Values](obsidian://open?vault=Quick%20notes&file=Work%20notes%2FCCS%20LineItemType.loom)
	     - 
## Cloud:
	MVP
	- AWS would be the cloud and direct customers(as a POC on VMware is done) to be started with, Granularity (Monthly/Weekly)
	- No perspective support for even definition
	- To be part of Cost history report rather than FlexReport
	- Pregenerated data/On the fly???






## Open Questions:
- Columns for deriving event definition need to dynamic - Yes
- Time based window selection - Comparison on fixed set of closed time windows(day/month/weeks)
- Tier change exploration.....
- Scopes of the data aggregation: Account level? Yes Service level? Yes 
- Is variance labelling calculated by using combination of a event definitions which constitute of the event attributes(i.e. measures) of a event definition? Yes
- Aria hub monthly release cadence....
- Effective cost = Recurring + Amortized + Prepay + on demand usage ()
- What's the use of `.after = ifelse("product_fromlocation" %in% names(.), "product_fromlocation", last_col())`