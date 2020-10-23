#### Query work item state changes

```
https://analytics.dev.azure.com/{org}/{project}/_odata/v3.0/WorkItemRevisions?
  $top=1000 &
  $apply=
    groupby((WorkItemId, State), aggregate(Revision with max as MaxRevision, RevisedDate with max as RevisedDate)) &
  $orderby=WorkItemId, MaxRevision
```

#### Bugs that have been re-activated after they were closed or resolved

```
https://analytics.dev.azure.com/{org}/{project}/_odata/WorkItemRevisions?
    $apply=filter(WorkItemType eq 'Bug') /
           groupby((WorkItemId, WorkItem/Title), 
                   aggregate(
                       iif(State eq 'Active', ChangedDate, 1970-01-01Z) with max as MaxActiveChangedDate, 
                       iif(State eq 'Resolved' or State eq 'Closed', ChangedDate, 2100-01-01Z) with min as MinResolvedOrClosedDate
                    )
            ) /
            filter(MaxActiveChangedDate gt MinResolvedOrClosedDate)
```
