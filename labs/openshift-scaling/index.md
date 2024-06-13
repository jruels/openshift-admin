# Scaling OpenShift 

## Overview

In this hands-on lab, you will explore the scaling capabilities of OpenShift by investigating MachineSets and MachineConfigs, and setting up both Cluster Autoscaler and Machine Autoscaler. You will then design a cluster and machine autoscaling solution that will scale nodes in the cluster when more resources are required.

### Investigate configuration

#### Investigate MachineSets

In this step, you will explore the MachineSets in your cluster to understand their configuration and role in managing worker nodes. MachineSets are crucial for defining how many replicas of a machine should be running in your cluster, and they help ensure high availability and resource management.

**Navigate to MachineSets**:

* In the OpenShift web console, go to the Administrator perspective.
* Navigate to Compute > MachineSets.
* Review MachineSets:
  * Examine the list of MachineSets.
    * Click on the machine type with `worker-us-west-1b` in the name. 
      * Look at the YAML, Machines, and Events. 



#### Investigate MachineConfigs

This step involves reviewing MachineConfigs to see how node configurations are managed and applied across the cluster. Understanding MachineConfigs is essential for ensuring that all nodes are consistently configured according to your organization’s policies.

**Navigate to MachineConfigs:**

* In the web console, navigate to **Compute > MachineConfigs**.
* Review MachineConfigs:
  * Review the list of MachineConfigs.
  * Check out the `00-master` and `00-worker` MachineConfigs
    * Review the YAML for each. 

### Create a ClusterAutoscaler

You will create a ClusterAutoscaler to automatically adjust the number of nodes in the cluster based on resource demands. This helps maintain optimal performance by ensuring that there are enough nodes to handle the workload, scaling up during peak times and scaling down when demand decreases.



Creating an autoscaler requires the command line. 

Create a YAML file with the following: 

```yaml
apiVersion: "autoscaling.openshift.io/v1"
kind: "ClusterAutoscaler"
metadata:
  name: "default"
spec:
  podPriorityThreshold: -10
  resourceLimits:
    maxNodesTotal: 3
    cores:
      min: 8
      max: 128
    memory:
      min: 4
      max: 256
  logVerbosity: 4
  scaleDown:
    enabled: true
    delayAfterAdd: 10m
    delayAfterDelete: 5m
    delayAfterFailure: 30s
    unneededTime: 5m
    utilizationThreshold: "0.4"
```

Submit the manifest to the cluster with `oc apply`. 



Confirm the autoscaler was created successfully. 

```bash
oc -n openshift-machine-api get clusterautoscaler default
```



You should see: 

```
NAME      AGE
default   4h51m
```



Confirm the configuration is correct. 

```bash
oc -n openshift-machine-api get clusterautoscaler default -o yaml
```



#### Create a MachineAutoscaler 

The ClusterAutosclaler requires a MachineAutoscaler. Set up a MachineAutoscaler to automatically manage the scaling of worker nodes based on the load. The Machine Autoscaler works alongside the Cluster Autoscaler to ensure that the right number of nodes is available to meet the resource needs of the cluster efficiently.



##### **Navigate to Machine Autoscalers:**

* In the web console, go to **Compute > Machine Autoscalers**.

* Click **Create Machine Autoscaler**.

  * Fill in with the following YAML:

    ```yaml
    apiVersion: "autoscaling.openshift.io/v1beta1"
    kind: "MachineAutoscaler"
    metadata:
      name: "trainer-qj2pm-worker-us-west-1b"
      namespace: "openshift-machine-api"
    spec:
      minReplicas: 1
      maxReplicas: 2
      scaleTargetRef:
        apiVersion: machine.openshift.io/v1beta1
        kind: MachineSet
        name: trainer-qj2pm-worker-us-west-1b
    ```

* Click **Create** to deploy the Machine Autoscaler.



#### **Check the Number of Nodes and List Machines**

Verify the current number of nodes and list all machines in the cluster to understand the existing resource allocation. This step is crucial for monitoring and ensuring that your autoscaling configurations function as expected.



##### **Navigate to Nodes:**

* In the web console, go to **Compute > Nodes**.
* Review the list of nodes to see the current number of nodes in the cluster.
* **List Machines:**
  * In the web console, go to **Compute > Machines**.
  * Review the list of machines to see details of each machine, including the machine type and status.

Run the following commands: 

```bash
oc get nodes 
```

```bash
oc -n openshift-machine-api get machines
```



Deploy a test batch job to verify autoscaling works as expected. 

Use the following YAML to create the batch job. 

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  generateName: work-queue-
spec:
  template:
    spec:
      containers:
      - name: work
        image: busybox
        command: ["sleep",  "300"]
        resources:
          requests:
            memory: 500Mi
            cpu: 500m
      restartPolicy: Never
  backoffLimit: 4
  completions: 50
  parallelism: 50
```



Submit to batch job

```bash
oc apply -f batch.yaml 
```



Confirm the batch job is creating MANY pods

```bash
oc get batch 
```



Now that the cluster is overwhelmed with the batch job pods, monitor the ClusterAutoscaler pod and confirm it adds a new node. 

List the pods in the `openshift-machine-api` namespace 

```bash
oc -n openshift-machine-api get pods
```



Then monitor the logs of the cluster-autoscaler-default pod.

```bash
oc -n openshift-machine-api logs -l cluster-autoscaler=default -f
```



You should see the autoscaler provisioning a new node. Confirm a new node is added to the cluster in the Web Console and with the `oc` command. 



#### Congratulations! 

By completing this lab, you understand the steps required to enable autoscaling in OpenShift. 

