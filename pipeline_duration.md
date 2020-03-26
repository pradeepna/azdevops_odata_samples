#### Top 20 pipelines which have more than 200 succeeded pipeline runs

```
https://analytics.dev.azure.com/{org}/{project}/_odata/v3.0/PipelineRuns?
  $apply=
    filter(SucceededCount eq 1) /
    filter(RunDurationSeconds ne null) /
    groupby((PipelineSK, Pipeline/PipelineName), aggregate(RunDurationSeconds with average as AvgRunDurationSeconds, $count as TotalRuns)) /
    filter(AvgRunDurationSeconds gt 0)/
    filter(TotalRuns gt 100) &
  $orderby=AvgRunDurationSeconds desc &
  $top=20
```
