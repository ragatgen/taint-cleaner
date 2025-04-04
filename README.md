High-Level Purpose:
This DaemonSet runs on every node in your AKS cluster.
It checks if the node has the taint DeletionCandidateOfClusterAutoscaler:PreferNoSchedule and removes it if present.

Bug
DeletionCandidateOfClusterAutoscaler soft taint remaining on nodes indefinitely #7964
https://github.com/kubernetes/autoscaler/issues/7964

While the fix is applied

https://github.com/kubernetes/autoscaler/pull/7954/files

Why is there a ServiceAccount, ClusterRole, and ClusterRoleBinding?
Even though you're running on AKS, pods don’t inherit cluster-admin privileges by default. Here's why these are required:

✅ ServiceAccount
Provides a dedicated Kubernetes identity for the pod (taint-cleaner) to interact with the API server.
This isolates permissions and avoids using overly privileged defaults.
✅ ClusterRole
Grants minimum required permissions to interact with node objects:
get: to read node details
list: needed when kubectl accesses node metadata
patch: to remove the taint from the node
Scoped only to "nodes" and read/patch actions.
✅ ClusterRoleBinding
Binds the ClusterRole to the taint-cleaner ServiceAccount at cluster scope.
Necessary because nodes are cluster-wide resources, not namespace-specific.
