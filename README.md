High-Level Purpose:
This DaemonSet runs on every node in your AKS cluster.
It checks if the node has the taint DeletionCandidateOfClusterAutoscaler:PreferNoSchedule and removes it if present.

Bug
DeletionCandidateOfClusterAutoscaler soft taint remaining on nodes indefinitely #7964
https://github.com/kubernetes/autoscaler/issues/7964

While the fix is applied

https://github.com/kubernetes/autoscaler/pull/7954/files
