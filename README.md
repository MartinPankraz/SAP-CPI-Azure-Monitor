# SAP-CPI-Azure-Monitor
Repos contains artifacts to implement SAP CPI monitoring with Azure Monitor Workbooks and Log Analytics

Both approaches need an OData user (CPI_USER) configured to call the CPI API regularly for existing failed messages on the processing log.

## Use BTP Alert Notification Service from CPI as relay

- Use iFlow "Send notifications for failed Message Processing Logs"
- Configure Alert Notification Service on BTP Cockpit. Check config guide that is attached to the iFlow package (Documents section) or see SAP [docs](https://blogs.sap.com/2019/10/14/receive-notifications-for-failed-sap-cloud-platform-integration-flows-via-any-channel-with-alert-notification/).

## Use Azure Log Analytics REST API from CPI directly

Current implementation supports only service keys for CPI OData API, because S-User password changes afect the iflow and the timer based execution easily locks the S-User (max 5 failed login attempts allowed).

- Use iFlow "Send notifications for failed MPL to Azure Log Analytics"
- Configure secure parameter _AzureLogAnalyticsSharedKey_ on "Operations View" with primary key of your Azure Log Analytics Workspace or adapt secure param name on the iFlow config. Find the primary key on the Azure portal -> Agents Management -> Primary Key.
- Configure Azure Log Analytics Workspace ID on the iFlow. Find the ID on the Overview pane on the Azure portal.
- Configure Service Key on CPI Integration Runtime (service plan API)
- Fill iFlow parameter "CPI API OAUTH" with token endpoint url from service key
- Create User credential (CPI Security Material) and fill iFlow parameter "CPI API Service Key"

## References
Link to Blog post: https://blogs.sap.com/2020/11/11/broadcast-cpi-errors-to-azure-monitor-via-scp-alert-notifications/