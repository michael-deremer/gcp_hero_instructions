## Google Cloud Billing Account Pre-Subscription Periodic
### Whats it do, Why do we need it?
> This pre sub job provides the updated information needed to create accurate SA's.
> SA's are the main way our cx team interacts to show value to our customer.
### What do i do?
* Permissions error: Contact the cx channel and ping the customer > settings > account manager, if no one is assigned contact clay & andrew

## Google Cloud Billing Account Daily
### Whats its do, Why do we need it?
> Captures all the things we need daily 🤷‍♂️
##### OOM
> Dragana noticed this first around Oct 6, 2025. It seems like some solvers require more than the ecs instances memory (6GB) and the oom happens.
> Its noteworthy that spot gets 8GB and periodic gets 6GB so its more likely to happen on periodic.
> So far this has only happened once in prod and the next run was ok. Keep an eye on this. We will have to address if it starts happening frequently.
> [Slack link](https://prosperops.slack.com/archives/C05QE9203RS/p1759771594751189?thread_ts=1759771574.158699&cid=C05QE9203RS)

##### Rerun
> You can directly start a new execution of the BillingAccountDaily state machine using the new execution button from the failed run.
> This is the goto path for things like a timeout failure. Just rerun it.



## GcpBillingAccountHourly
CopyIncrementalBillingExportData -> CreateBigQueryResources -> CaptureGcpProjects -> CaptureIamPermissions
To search for subsequent success: search for GcpBillingAccountHourly state machine using gcp billing account id.
##### Copy IncrementalBillingExportData
Copies customers billing data from their dataset into prosperops. It uses a partition marker to find the last successful partition inported. Imports data every hour for current day until its finalized. 
Typical Failure: the service bigquery has thrown an exception, no http status code was specified. Tons of failures but all recovered. 
OnFailure: next run can recover missed hours.

##### CaptureIamPermisssions
OnFailure: just let it run the next hour.



## GcpActionPlanner
Rerun via buttons on the billing account page.
