We have basically 3 design choices to implement a Jupyter Notebook Operator. The main goal is to implement an operator with some level 5 capabilities, i.e. the operator should execute one action to maintain running notebooks intelligently. 


### 1. JupyterHub from ODH
This [JupyterHub](https://github.com/jupyterhub/jupyterhub) implementation utilizes [kubespawner](https://github.com/jupyterhub/kubespawner) and it's hook system to start pods and do adjustments. 

**PRO**
* It's based on a lively community. 
* RBAC for accessing the notebook spawner and server UI 
* Easy to implement default profiles and user specific profiles
* Has some prometheus metrics built-in
* Python based
 
**CON** 
* It does not follow operator best practices (no CRD, stateful with DB)
* No way to leverage operator-sdk 
* Users can't `oc rsh` into their pod.
* Currently little room to contribute back upstream, since our adjustments are very OKD/ODH specific

### 2. Notebook controller from KF
This [notebook-controller](https://github.com/kubeflow/kubeflow/tree/master/components/notebook-controller) implements CRD to spawn pods in user namespaces

**PRO**
* Has a clear k8s native upstream: kubeflow
* implemented via clean CRD 
 
**CON** 
* lacks any auth
* every user deploys to his own namespace
* does not leverage the operator-sdk
* no velocity recently - not many commits
 
### 3. From scratch based on operator-sdk
Implement a new version of a notebook spawner using the [operator-sdk](https://github.com/operator-framework/operator-sdk)

**PRO**
* can use the operator-sdk and contribute back up 
* implemented via clean CRD 
* could replace the KF notebook-controller on day
 
**CON** 
* have to use go-lang
* needs to build community and adoption (which we know is not easy)
* all maintenance would be on ODH/AIOps team


---
#### Notes
* There is an issue to support the operator-sdk in [python](https://github.com/operator-framework/operator-sdk/issues/2745), but no real implementation as of yet.


