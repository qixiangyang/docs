Kubecost exposes multiple APIs to obtain cost, resource allocation, and utilization data. Below is documentation on two options: the cost model API and aggregated cost model API.  

# Cost model API

The full cost model API exposes pricing model inputs at the individual container/workload level and is available at:

`http://<kubecost-address>/model/costDataModel`

Here's an example use:

`http://localhost:9090/model/costDataModel?timeWindow=7d&offset=7d`

API parameters include the following:

* `timeWindow` dictates the applicable window for measuring cost metrics. Supported units are d, h, and m.  
* `offset` shifts timeWindow backwards relative to the current time. Supported units are d, h, and m.

This API returns a set of JSON elements in this format:

```
{
  cpuallocated: [{timestamp: 1567531940, value: 0.01}]
  cpureq: [{timestamp: 1567531940, value: 0.01}]
  cpuused: [{timestamp: 1567531940, value: 0.006}]
  deployments: ["cost-model"]
  gpureq: [{timestamp: 0, value: 0}]
  labels: {app: "cost-model", pod-template-hash: "1576869057"}
  name: "cost-model"
  namespace: "cost-model"
  node: {hourlyCost: "", CPU: "2", CPUHourlyCost: "0.031611", RAM: "13335256Ki",…}
  nodeName: "gke-kc-demo-highmem-pool-b1faa4fc-fs6g"
  podName: "cost-model-59cbdbf49c-rbr2t"
  pvcData: [{class: "standard", claim: "kubecost-model", namespace: "kubecost",…}]
  ramallocated: [{timestamp: 1567531940, value: 55000000}]
  ramreq: [{timestamp: 1567531940, value: 55000000}]
  ramused: [{timestamp: 1567531940, value: 19463457.32}]
  services: ["cost-model"]
}  
```
<a name="optional-params"></a>  
Optional request parameters include the following:  

Field | Description 
--------- | ----------- 
`filterFields` | Blacklist of fields to be filtered from response. For example, appending `&filterFields=cpuused,cpureq,ramreq,ramused` will remove request and usage data.
`namespace` | Filter results by namespace. For example, appending `&namespace=kubecost` only returns data for the `kubecost` namespace


# Aggregated cost model API

The aggregated cost model API retrieves data similiar to the Kubecost Allocation frontend view and is available at:

`http://<kubecost-address>/model/aggregatedCostModel`

Here's an example use:

`http://localhost:9090/model/aggregatedCostModel?timeWindow=1d&aggregation=namespace`

API parameters include the following:

* `timeWindow` dictates the applicable window for measuring cost, usage, etc. Supported units are d, h, and m.  
* `offset` (optional) shifts timeWindow backwards relative to the current time. Supported units are d, h, and m.  
* `aggregation` is the field used to consolidate cost model data. Supported types are namespace, deployment, service, label, and pod.  
* `aggregationSubField` used for aggregation type that required sub fields, e.g. `label=product`

This API returns a set of JSON objects in this format:

```
{
  aggregator: "namespace"
  aggregatorSubField: ""
  cluster: "cluster-1"
  cpuCost: 100.031611
  environment: "default"
  gpuCost: 0
  networkCost: 0
  pvCost: 10.000000
  ramCost: 70.000529625
  totalCost: 180.032140625
}  
```

Have questions? Email us at <team@kubecost.com>.