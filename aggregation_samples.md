
### Simple Count Aggregations

#### Count of all work items

```
https://analytics.dev.azure.com/{org}/{project}/_odata/v3.0/WorkItems/$count
```

```
https://analytics.dev.azure.com/{org}/{project}/_odata/v3.0/WorkItems?
  $apply=aggregate($count as Count)
```

#### Count of all work items that are 'In Progress'

```
https://analytics.dev.azure.com/{org}/{project}/_odata/v3.0/WorkItems/$count?$filter=State eq 'In Progress'
```

```
https://analytics.dev.azure.com/{org}/{project}/_odata/v3.0/WorkItems?
  $apply=
    filter(State eq 'In Progress')/
    aggregate($count as Count)
  
```

### Aggregation Extension

#### Sum if remaining work

```
https://analytics.dev.azure.com/{org}/{project}/_odata/v3.0/WorkItems?
  $apply=aggregate(RemainingWork with sum as SumOfRemainingWork)
```

#### Last work item indentifier

```
https://analytics.dev.azure.com/{org}/{project}/_odata/v3.0/WorkItems?
  $apply=aggregate(WorkItemId with max as MaxWorkItemId)
```

### Group by

### Group by work item type

```
https://analytics.dev.azure.com/{org}/{project}/_odata/v3.0/WorkItems?
  $apply=groupby((WorkItemType), aggregate($count as Count))
```


#### Group by work item type and state (multiple properties)

```
https://analytics.dev.azure.com/{org}/{project}/_odata/v3.0/WorkItems?
  $apply=groupby((WorkItemType, State), aggregate($count as Count))
```

### Filter aggregated results

#### Group by state for work items of type user story

```
https://analytics.dev.azure.com/{org}/{project}/_odata/v3.0/WorkItems?
  $apply=
    filter(WorkItemType eq 'User Story')/
    groupby((State), aggregate($count as Count))
```


### Multiple aggregations in a single call

```
https://analytics.dev.azure.com/{org}/{project}/_odata/v3.0/WorkItems?
  $apply=
    aggregate(CompletedWork with sum as SumOfCompletedWork, RemainingWork with sum as SumOfRemainingWork)

```

### Cumulative flow data

```
https://analytics.dev.azure.com/{org}/{project}/_odata/v3.0/WorkItemBoardSnapshot?
  $apply=
    filter(DateValue gt 2020-01-01Z)/
    filter(BoardName eq 'Stories' and Team/TeamName eq 'Compass')/
    groupby((DateValue, ColumnName), aggregate(Count with sum as Count))&
  $orderby=DateValue

```

