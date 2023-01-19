# Sonar Cloud
## How to use the Workflow
---
### **Implementation**
---
Create your workflow:<br/>
workflow has to be located from your root directory on<br/>
```
.github/workflows/[your_workflow].yml
```
### **Inputs**
---
<br/>
Two Inputs are required for this workflow to run:<br/>

**path**: 
1. The full path required to reach the first solution of your 'solution-matrix'
2. This must be a string 
3. Elements separated by '/'
```
'input_string'
```

**solution-matrix**: 
1. A stringified JSON string that contains an array of all the solutions that will be evaluated by this scan run.
2. Separated by commas
  Syntax:
  ```
  "[ 'solution_1', 'solution_2'... 'solution_n' ]"
  ```

Paste this template and replace the values for the ones needed:

```yml
name: SonarCloud
on:
  push:
    branches:
      - [your_branch] # Not required if going to production
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  # ~ Sonar cloud Scan 
  sonarcloudScan_node:
    name: SonarCloudScan
    uses: zenergy/actions-reusable-workflows/.github/workflows/sonarcloud-node.yml@feat/DIGINT-294-sonarcloud

    with:
      path: '[full path of the first solution in the matrix below]'
      solution-matrix: "[ '[solution_1]', '[solution_2]', '[solution_3]' ]"
    secrets:
      SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
      AZURE_ARTIFACTS_TOKEN: ${{ secrets.AZURE_ARTIFACTS_TOKEN }}
```
---
### **Examples**
---
There will be 2 main styles or structures to provide the inputs, an example of each is shown below:

(Repo-example: openloop-events)

```yml
name: SonarCloud
on:
  push:
    branches:
      - feature/DIGINT-294** #  branch used for developing and testing the flow
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  # ~ Sonar cloud Scan 
  sonarcloudScan_node:
    name: SonarCloudScan
    uses: zenergy/actions-reusable-workflows/.github/workflows/sonarcloud-node.yml@feat/DIGINT-294-sonarcloud

    with:
      path: "openloop-events/apf-openloop-saleevents"
      solution-matrix: "[ 'apf-openloop-saleevents', 'apf-lcf-saleevents-facade', 'tf' ]"
    secrets:
      SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
      AZURE_ARTIFACTS_TOKEN: ${{ secrets.AZURE_ARTIFACTS_TOKEN }}
```
(Repo-example: fleet-integrations)
```yml
name: SonarCloud
on:
  push:
    branches:
      - feature/DIGINT-294** #  branch used for developing and testing the flow
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  # ~ Sonar cloud Scan - CargoCircular
  sonarcloud_node:
    name: SonarCloudScan
    uses: zenergy/actions-reusable-workflows/.github/workflows/sonarcloud-node.yml@feat/DIGINT-294-sonarcloud

    with:
      path: 'fleet-integrations/CargoCircular/apf-cargo-circular'
      solution-matrix: "[ 'apf-cargo-circular', 'tf' ]"
    secrets:
      SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
      AZURE_ARTIFACTS_TOKEN: ${{ secrets.AZURE_ARTIFACTS_TOKEN }}

    # ~ Sonar cloud Scan - RollingPlan
  sonarcloud_node_1:
    name: SonarCloudScan
    uses: zenergy/actions-reusable-workflows/.github/workflows/sonarcloud-node.yml@feat/DIGINT-294-sonarcloud

    with:
      path: 'fleet-integrations/RollingPlan/apf-rolling-plan'
      solution-matrix: "[ 'apf-rolling-plan', 'tf' ]"
    secrets:
      SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
      AZURE_ARTIFACTS_TOKEN: ${{ secrets.AZURE_ARTIFACTS_TOKEN }}
```
**NOTICE:**<br/>
- You need to run the flow once per project, this will be the second structure type (monorepo)<br/>
- Here it runs the scan one for all the solutions inside: CargoCircular<br/>
- Once more for the solutions inside: RollingPlan<br/>
- Notice the name of the job needs to be different for the second run:<br/>
First run job name: _**sonarcloud_node**_<br/>
Second run job name: _**sonarcloud_node_1**_<br/>






Pages:
<br/>
**Confluence**: To be added <br/>
**Sonar Cloud**: https://sonarcloud.io/  
