#### Gerrit setup
- Access request: https://vmware.slack.com/archives/C01RQHN4K0C/p1695365054240359
- Use your local ssh RSA pub key setup in https://bellevue-ci-gerrit.eng.vmware.com/settings/ssh-keys#SSHKeys
- Also update your full name for PRs in https://bellevue-ci-gerrit.eng.vmware.com/#/settings/

#### Gitlab setup
- Add your local ssh RSA pub key setup in https://gitlab.eng.vmware.com/-/profile/keys
- You can test your access using `ssh -T git@gitlab.eng.vmware.com`

#### Code Stream access
- Access now ticket: https://accessnow.vmware.com/ECM/jbpmworkflowmanagement/showrequestdetails/ManagerEntitlementOwnerApproval.25088619?requesttypes=0&reqid=307582

#### CSP(CMBU_VMC_Nimbus_Org) access
- Can be raised from workflow in https://vmware.slack.com/archives/C01RQHN4K0C
- Confirm that you have access by navigating to the Ensemble UI at [https://www.be.symphony-dev.com/ensemble/](https://www.be.symphony-dev.com/ensemble/) and select the org with data.
-  For ***GraphQL*** execution create a token with 'all roles' for your user. API token can be generated at [https://console-stg.cloud.vmware.com/csp/gateway/portal/#/user/tokens](https://console-stg.cloud.vmware.com/csp/gateway/portal/#/user/tokens)
- `export CSP_USER_TOKEN=<insert your API token here>` in local and run `~/ensemble-project$ ./scripts/ensemble-cli.sh auth_json` this will generate a Bearer token. Navigate to [https://www.be.symphony-dev.com/ensemble/graphql](https://www.be.symphony-dev.com/ensemble/graphql "https://www.be.symphony-dev.com/ensemble/graphql") comment the `mutation` code and uncomment the AWS query part and add the token copied from `auth_json` script run to `Headers` and trigger the request it should return response.

#### AWS access
- Example access request: https://accessnow.vmware.com/ECM/jbpmworkflowmanagement/showrequestdetails/MultiLevelApprovalFlowForRestApps.25093309?requesttypes=0&reqid=307705
- Ref doc: https://confluence.eng.vmware.com/display/VOS/Request+AWS+console+access#RequestAWSconsoleaccess-ListofAWSAccountsandRoles

#### EKS access


#### Vault access:
- Example request: https://help.vmware.com/ticket/TKT6738959

#### EDS access(Not sure if required for GraphQL changes):

#### LINT access:
- Ref doc: https://confluence.eng.vmware.com/display/SYM/CMBU+Monitoring+%28Lint-Mon%29+Org+Access
- Access now example request: https://accessnow.vmware.com/ECM/jbpmworkflowmanagement/showrequestdetails/ManagerEntitlementOwnerApproval.25093493?requesttypes=0&reqid=307708

#### ensemble-cost project setup
- Clone `ensemble-project` using `git clone git@gitlab.eng.vmware.com:cmbu/ensemble-project.git` and run `make` command it'll auto download all the related repos
- Install Java17(required)
- Run `make hooks` , `make clean` and `make build`
- For Intellij setup open `CostingApp` class and run the application after import in Intellij(Make sure to set SDK to Java17)
	- Add in Application VM options:
  ```java
	  		--add-opens=java.base/java.nio=ALL-UNNAMED --add-opens=jdk.management/com.sun.management.internal=ALL-UNNAMED --add-opens=java.base/jdk.internal.misc=ALL-UNNAMED --add-opens=java.management/com.sun.jmx.mbeanserver=ALL-UNNAMED --add-opens=jdk.internal.jvmstat/sun.jvmstat.monitor=ALL-UNNAMED --add-opens=java.base/sun.reflect.generics.reflectiveObjects=ALL-UNNAMED --add-opens=java.base/java.io=ALL-UNNAMED --add-opens=java.base/java.nio=ALL-UNNAMED --add-opens=java.base/java.util=ALL-UNNAMED --add-opens=java.base/java.util.concurrent=ALL-UNNAMED --add-opens=java.base/java.util.concurrent.locks=ALL-UNNAMED --add-opens=java.base/java.lang=ALL-UNNAMED
```


#### Code walkthrough
- finops resource holds types of inputs like FinopsPropertyFilterInput
- `query_federation.graphqls` is the starting point
- `FinOpsEntityListPricesDataFetcher` -> `FinOpsEntityListPricesDataLoader` -> `CHPricingService` 