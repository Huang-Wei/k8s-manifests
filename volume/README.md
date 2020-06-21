## Overview

Demo that scheduler doesn't enforce a PVC to be bound to a more fit PV.

```script
⇒  k get po,pv,pvc
NAME                          READY   STATUS    RESTARTS   AGE
pod/deploy-6f7cdf6d66-h5qzr   1/1     Running   0          10s

NAME                   CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM         STORAGECLASS   REASON   AGE
persistentvolume/pv1   100Mi      RWO            Retain           Available                 retain                  26s
persistentvolume/pv2   200Mi      RWO            Retain           Available                 retain                  8m43s
persistentvolume/pv3   300Mi      RWO            Retain           Bound       default/pvc   retain                  4m16s

NAME                        STATUS   VOLUME   CAPACITY   ACCESS MODES   STORAGECLASS   AGE
persistentvolumeclaim/pvc   Bound    pv3      300Mi      RWO            retain         10s
```

In the above output, `pvc` is bound to `pv3` instead of smaller pvs (pv1 & pv2).

## Env

This test is run under Kind version 0.8.1, with the following config:

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
networking:
  ipFamily: ipv4
nodes:
- role: control-plane
- role: worker
- role: worker
- role: worker
```

## Solution

[KEP-1845: prioritization on volume capacity](https://github.com/kubernetes/enhancements/pull/1862)
is trying to solve this problem.
