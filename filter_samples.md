### Metadata

#### Query all entity sets
```
https://analytics.dev.azure.com/{org}/{project}/_odata/v3.0
```

#### Query metadata
```
https://analytics.dev.azure.com/{org}/{project}/_odata/v3.0/$metadata
```

### Filter data

#### Query a single work item
```
https://analytics.dev.azure.com/{org}/{project}/_odata/v3.0/WorkItems? $filter=WorkItemId eq 1696440
```

#### Query a single work item and select just the title
```
https://analytics.dev.azure.com/{org}/{project}/_odata/v3.0/WorkItems? $filter=WorkItemId eq 1696440 & $select=Title
```

#### Query a single work item and return area and iteration path (related entities using navigation)
```
https://analytics.dev.azure.com/{org}/{project}/_odata/v3.0/WorkItems? 
  $filter=WorkItemId eq 1696440 & 
  $select=Title &
  $expand=Area($select=AreaName,AreaPath)
```

#### Query work item in Compass area that contains A11y in Title and was created after Feb 01, 2020
```
https://analytics.dev.azure.com/{org}/{project}/_odata/v3.0/WorkItems? 
  $filter=contains(Area/AreaPath, 'Compass') and contains(Title, 'A11y') and CreatedDate ge 2020-02-01Z  &
  $select=Title &
  $expand=Area($select=AreaName,AreaPath)
```
  
#### Query all titles of descendants of a work item
```
https://analytics.dev.azure.com/{org}/{project}/_odata/v3.0/WorkItems? 
  $filter=WorkItemId eq 1206751 &
  $select=Descendants &
  $expand=Descendants($select=Title)
 ```
 
#### Query descendants of a work item that contains 'perf in the title
```
https://analytics.dev.azure.com/{org}/{project}/_odata/v3.0/WorkItems? 
  $filter=WorkItemId eq 1206751 &
  $select=Descendants &
  $expand=Descendants($select=Title;$filter=contains(Title, 'perf')) 
```

#### Query all work items created after Jan 01, 2020, in Compass area where any descendant contains the word 'view'
```
https://analytics.dev.azure.com/{org}/{project}/_odata/v3.0/WorkItems? 
  $filter=contains(Area/AreaPath, 'Compass') and CreatedDate ge 2020-01-01Z and Descendants/any(d: contains(d/Title, 'view')) &
  $select=Descendants, Title &
  $expand=Descendants($select=Title)
```

  
  

  


