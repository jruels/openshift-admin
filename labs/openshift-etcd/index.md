# Understanding etcd in OpenShift

## Overview

etcd is a highly consistent, distributed key-value store used for critical data management in OpenShift. It serves as the backbone for storing the cluster’s configuration and state. OpenShift uses etcd to ensure high availability, reliability, and consistency of data across the cluster. Understanding how to interact with etcd is essential for maintaining and troubleshooting OpenShift clusters.



This lab will guide you through several key commands to interact with etcd in OpenShift 4.15, providing a hands-on understanding of its functionality and operations.





#### Access etcd pod

To begin interacting with etcd, you need to access the etcd pod running in the control plane.

List the pods in the `openshift-etcd` namespace

**Command:**

```bash
oc get pods -n openshift-etcd -l app=etcd
```



Expected output: 

```
NAME                                             READY   STATUS    RESTARTS   AGE
etcd-ip-10-0-20-149.us-west-1.compute.internal   4/4     Running   0          2d5h
```



Now remote into the etcd container

**Command:**

```sh
oc rsh -n openshift-etcd -c etcd <etcd_pod_name>
```

**Description:**

This command opens a remote shell session into the etcd container running in the `openshift-etcd` namespace. Replace `<etcd_pod_name>` with the name of the etcd pod.

Expected Output:

A shell prompt within the etcd pod, e.g., `sh-4.2$`.



#### Check etcd Endpoint Health

Once inside the etcd pod, you can check the health of the etcd endpoints.

**Command:**

```sh
etcdctl endpoint health --cluster -w table
```

**Description:**

This command uses `etcdctl`, the command-line tool for interacting with etcd, to check the health of all etcd endpoints in the cluster. The `-w table` flag formats the output as a table.

**Expected Output:**

```
+--------------------------+--------+------------+-------+
|         ENDPOINT         | HEALTH |    TOOK    | ERROR |
+--------------------------+--------+------------+-------+
| https://10.0.20.149:2379 |   true | 9.257113ms |       |
+--------------------------+--------+------------+-------+
```



#### List etcd Members

To view the members of the etcd cluster:

**Command:**

```bash
etcdctl member list -w table
```

**Description:**

This command lists the etcd cluster members in a table format, displaying their IDs, status, and roles.



**Expected Output:**

```
+------------------+---------+-------------------------------------------+--------------------------+--------------------------+------------+
|        ID        | STATUS  |                   NAME                    |        PEER ADDRS        |       CLIENT ADDRS       | IS LEARNER |
+------------------+---------+-------------------------------------------+--------------------------+--------------------------+------------+
| 7f8c6620a7e252be | started | ip-10-0-20-149.us-west-1.compute.internal | https://10.0.20.149:2380 | https://10.0.20.149:2379 |      false |
+------------------+---------+-------------------------------------------+--------------------------+--------------------------+------------+
```



#### List all keys

To list all keys stored in etcd:  

**Command:**

```bash
etcdctl get "" --prefix --keys-only
```



#### Query a Specific Key

Choose a key from the output above and run the following command, replacing <key> with your choice. 

e.g. `/kubernetes.io/apiregistration.k8s.io/apiservices/v2.autoscaling`

**Command:**

```bash
etcdctl get <key>
```



#### Enter Node Debug Mode

To perform certain maintenance tasks, you may need to use the `debug` option to connect to a node.

**Command:**

```sh
oc debug --as-root node/<control_plane_node>
```

**Description:**

This command initiates a debug session on the specified control plane node. As discussed, etcd runs on the control plane, and we must connect to it back up the data. Replace `<node_name>` with the name of the control plane node you wish to debug (e.g., `ip-10-0-20-149.us-west-1.compute.internal`).

**Expected Output:**

A debug pod is created, and you are presented with a shell prompt, e.g., `sh-4.2#`.

#### Change Root Directory

Within the debug session, change the root directory to access the node's file system.

**Command:**

```sh
chroot /host
```

**Description:**

This command changes the root directory to `/host`, allowing you to interact with the node's file system as if you were directly logged into the node.

#### Perform Cluster Backup

Finally, perform a cluster backup using the provided script.

**Command:**

```sh
/usr/local/bin/cluster-backup.sh /home/core/assets/backup
```

**Description:**

This command runs a script to back up the cluster data, specifying the backup location. The backup script captures the current state of the etcd database and stores it in the specified directory.

**Expected Output:**

A series of messages indicating the progress and completion of the backup, e.g.,

```
Backing up etcd data...
etcdctl save_snapshot /home/core/assets/backup/etcd-snapshot.db
Snapshot saved successfully.
Backup complete. Files saved to /home/core/assets/backup.
```

### Congratulations

In this lab, you have learned to interact with etcd in OpenShift 4.15 by accessing the etcd pod, checking endpoint health, listing etcd members, querying specific keys, debugging nodes, changing root directories, and performing cluster backups. These commands and their outputs provide a fundamental understanding of managing and troubleshooting etcd within an OpenShift cluster.

