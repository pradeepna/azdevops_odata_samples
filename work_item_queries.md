#### Query work item state changes

```
https://analytics.dev.azure.com/{org}/{project}/_odata/v3.0/WorkItemRevisions?
  $top=1000 &
  $apply=
    groupby((WorkItemId, State), aggregate(Revision with max as MaxRevision, RevisedDate with max as RevisedDate)) &
  $orderby=WorkItemId, MaxRevision
```
