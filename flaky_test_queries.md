### Flaky test queries

#### Are the same flaky tests running in multiple pipelines

```
https://analytics.dev.azure.com/{org}/{project}/_odata/v4.0-preview/TestResults?
  $apply=
    filter(IsFlaky eq true and Outcome eq 'Failed' and CompletedDate gt 2020-02-01Z)/
    groupby((Test/TestName, Test/TestSK, Pipeline/PipelineName, Pipeline/PipelineSK), 
             aggregate($count as FailedFlakyTestPipelineWiseCount)) /
    groupby((Test/TestSK,Test/TestName), aggregate($count as DistinctPipelines))/
    filter(DistinctPipelines gt 3)&
  $orderby=DistinctPipelines desc
```

```
https://analytics.dev.azure.com/{org}/{project}/_odata/v4.0-preview/TestResults?
  $apply=
    filter(IsFlaky eq true and Outcome eq 'Failed' and CompletedDate gt 2020-02-01Z)/
    groupby((Test/TestName, Test/TestSK, Pipeline/PipelineName, Pipeline/PipelineSK), 
              aggregate($count as FailedFlakyTestPipelineWiseCount))
```

#### Which tests always fail in all my pipelines

```
https://analytics.dev.azure.com/{org}/{project}/_odata/v4.0-preview/TestResults?
  $apply=
    filter(CompletedDate gt 2020-02-01Z)/
    groupby((TestSK, Test/TestName, PipelineSK), 
            aggregate($count as OccurenceCount, 
                      iif(Outcome eq 'Failed', 1, 0) with sum as FailedCount)
             ) /
    groupby((TestSK, Test/TestName), 
            aggregate(OccurenceCount with sum as TotalOccurenceCount, 
                      FailedCount with sum as TotalFailedCount, 
                      $count as PipelineCount, 
                      iif(FailedCount gt 0, 1, 0) with sum as FailedPipelineCount))/
     filter(FailedPipelineCount eq PipelineCount) &
  $orderby=TotalFailedCount desc
```

```
https://analytics.dev.azure.com/{org}/{project}/_odata/v4.0-preview/TestResults?
  $apply=
    filter(CompletedDate gt 2020-02-01Z)/
    groupby((TestSK, Test/TestName, PipelineSK), 
             aggregate($count as OccurenceCount, 
             iif(Outcome eq 'Failed', 1, 0) with sum as FailedCount))
```

#### Which pipelines have the fewest flaky tests?

```
https://analytics.dev.azure.com/{org}/{project}/_odata/v4.0-preview/TestResults?
  $apply=
    filter(CompletedDateSK gt 2020-02-01Z and IsFlaky eq true) /
    groupby((PipelineSK, Pipeline/PipelineName, TestSK), aggregate($count as FlakyCount)) /
    groupby((PipelineSK, Pipeline/PipelineName), aggregate($count as uniqueflakyTestsCount)) & 
  $orderby= uniqueflakyTestsCount

```


