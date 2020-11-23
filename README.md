# rook-ceph 설치 가이드



## 1. Ceph 소스 다운로드 및 POD 배포

git clone --single-branch --branch release-1.2 https://github.com/rook/rook.git

<pre>
    <code>
kubeadmin@kworker01:~/installer$  git clone --single-branch --branch release-1.2 https://github.com/rook/rook.git
Cloning into 'rook'...
remote: Enumerating objects: 60, done.
remote: Counting objects: 100% (60/60), done.
remote: Compressing objects: 100% (26/26), done.
remote: Total 40355 (delta 40), reused 51 (delta 34), pack-reused 40295
Receiving objects: 100% (40355/40355), 13.76 MiB | 6.17 MiB/s, done.
Resolving deltas: 100% (27455/27455), done.
kubeadmin@kworker01:~/installer$ ls
rook
</code>
</pre>

## 2. 제공되고있는 예제에 있는 yaml을 통해 설치 진행

다음 3개의 yaml을 순차적으로 설치
<pre>
$ kubectl apply -f rook/cluster/examples/kubernetes/ceph/common.yaml
$ kubectl apply -f rook/cluster/examples/kubernetes/ceph/operator.yaml
$ kubectl apply -f rook/cluster/examples/kubernetes/ceph/cluster.yaml
</pre>
 - common.yaml파일을 원본 그대로 설치하면 secrets naming에 sa가 붙게 되는데, hyper cloud와 이름을 같게 하려면 common.yaml에서 rook-csi-cephfs-provisioner-ha의 이름을 rook-csi-cephfs-provisioner으로 바꿔줘야 된다. 

<pre>
    <code>
1)kubeadmin@kworker01:~/installer$ kubectl apply -f rook/cluster/examples/kubernetes/ceph/common.yaml
namespace/rook-ceph created
Warning: apiextensions.k8s.io/v1beta1 CustomResourceDefinition is deprecated in v1.16+, unavailable in v1.22+; use apiextensions.k8s.io/v1 CustomResourceDefinition
customresourcedefinition.apiextensions.k8s.io/cephclusters.ceph.rook.io created
customresourcedefinition.apiextensions.k8s.io/cephclients.ceph.rook.io created
customresourcedefinition.apiextensions.k8s.io/cephfilesystems.ceph.rook.io created
customresourcedefinition.apiextensions.k8s.io/cephnfses.ceph.rook.io created
customresourcedefinition.apiextensions.k8s.io/cephobjectstores.ceph.rook.io created
customresourcedefinition.apiextensions.k8s.io/cephobjectstoreusers.ceph.rook.io created
customresourcedefinition.apiextensions.k8s.io/cephblockpools.ceph.rook.io created
customresourcedefinition.apiextensions.k8s.io/volumes.rook.io created
customresourcedefinition.apiextensions.k8s.io/objectbuckets.objectbucket.io created
customresourcedefinition.apiextensions.k8s.io/objectbucketclaims.objectbucket.io created
Warning: rbac.authorization.k8s.io/v1beta1 ClusterRoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRoleBinding
clusterrolebinding.rbac.authorization.k8s.io/rook-ceph-object-bucket created
Warning: rbac.authorization.k8s.io/v1beta1 ClusterRole is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRole
clusterrole.rbac.authorization.k8s.io/rook-ceph-cluster-mgmt created
clusterrole.rbac.authorization.k8s.io/rook-ceph-cluster-mgmt-rules created
Warning: rbac.authorization.k8s.io/v1beta1 Role is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 Role
role.rbac.authorization.k8s.io/rook-ceph-system created
clusterrole.rbac.authorization.k8s.io/rook-ceph-global created
clusterrole.rbac.authorization.k8s.io/rook-ceph-global-rules created
clusterrole.rbac.authorization.k8s.io/rook-ceph-mgr-cluster created
clusterrole.rbac.authorization.k8s.io/rook-ceph-mgr-cluster-rules created
clusterrole.rbac.authorization.k8s.io/rook-ceph-object-bucket created
serviceaccount/rook-ceph-system created
Warning: rbac.authorization.k8s.io/v1beta1 RoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 RoleBinding
rolebinding.rbac.authorization.k8s.io/rook-ceph-system created
clusterrolebinding.rbac.authorization.k8s.io/rook-ceph-global created
serviceaccount/rook-ceph-osd created
serviceaccount/rook-ceph-mgr created
serviceaccount/rook-ceph-cmd-reporter created
role.rbac.authorization.k8s.io/rook-ceph-osd created
clusterrole.rbac.authorization.k8s.io/rook-ceph-osd created
clusterrole.rbac.authorization.k8s.io/rook-ceph-mgr-system created
clusterrole.rbac.authorization.k8s.io/rook-ceph-mgr-system-rules created
role.rbac.authorization.k8s.io/rook-ceph-mgr created
role.rbac.authorization.k8s.io/rook-ceph-cmd-reporter created
rolebinding.rbac.authorization.k8s.io/rook-ceph-cluster-mgmt created
rolebinding.rbac.authorization.k8s.io/rook-ceph-osd created
rolebinding.rbac.authorization.k8s.io/rook-ceph-mgr created
rolebinding.rbac.authorization.k8s.io/rook-ceph-mgr-system created
clusterrolebinding.rbac.authorization.k8s.io/rook-ceph-mgr-cluster created
clusterrolebinding.rbac.authorization.k8s.io/rook-ceph-osd created
rolebinding.rbac.authorization.k8s.io/rook-ceph-cmd-reporter created
podsecuritypolicy.policy/rook-privileged created
clusterrole.rbac.authorization.k8s.io/psp:rook created
clusterrolebinding.rbac.authorization.k8s.io/rook-ceph-system-psp created
rolebinding.rbac.authorization.k8s.io/rook-ceph-default-psp created
rolebinding.rbac.authorization.k8s.io/rook-ceph-osd-psp created
rolebinding.rbac.authorization.k8s.io/rook-ceph-mgr-psp created
rolebinding.rbac.authorization.k8s.io/rook-ceph-cmd-reporter-psp created
serviceaccount/rook-csi-cephfs-plugin-sa created
serviceaccount/rook-csi-cephfs-provisioner-sa created
role.rbac.authorization.k8s.io/cephfs-external-provisioner-cfg created
rolebinding.rbac.authorization.k8s.io/cephfs-csi-provisioner-role-cfg created
clusterrole.rbac.authorization.k8s.io/cephfs-csi-nodeplugin created
clusterrole.rbac.authorization.k8s.io/cephfs-csi-nodeplugin-rules created
clusterrole.rbac.authorization.k8s.io/cephfs-external-provisioner-runner created
clusterrole.rbac.authorization.k8s.io/cephfs-external-provisioner-runner-rules created
clusterrolebinding.rbac.authorization.k8s.io/rook-csi-cephfs-plugin-sa-psp created
clusterrolebinding.rbac.authorization.k8s.io/rook-csi-cephfs-provisioner-sa-psp created
clusterrolebinding.rbac.authorization.k8s.io/cephfs-csi-nodeplugin created
clusterrolebinding.rbac.authorization.k8s.io/cephfs-csi-provisioner-role created
serviceaccount/rook-csi-rbd-plugin-sa created
serviceaccount/rook-csi-rbd-provisioner-sa created
role.rbac.authorization.k8s.io/rbd-external-provisioner-cfg created
rolebinding.rbac.authorization.k8s.io/rbd-csi-provisioner-role-cfg created
clusterrole.rbac.authorization.k8s.io/rbd-csi-nodeplugin created
clusterrole.rbac.authorization.k8s.io/rbd-csi-nodeplugin-rules created
clusterrole.rbac.authorization.k8s.io/rbd-external-provisioner-runner created
clusterrole.rbac.authorization.k8s.io/rbd-external-provisioner-runner-rules created
clusterrolebinding.rbac.authorization.k8s.io/rook-csi-rbd-plugin-sa-psp created
clusterrolebinding.rbac.authorization.k8s.io/rook-csi-rbd-provisioner-sa-psp created
clusterrolebinding.rbac.authorization.k8s.io/rbd-csi-nodeplugin created
clusterrolebinding.rbac.authorization.k8s.io/rbd-csi-provisioner-role created
</code>
</pre>

<pre>
<code>
2) kubeadmin@kworker01:~/installer$ kubectl apply -f rook/cluster/examples/kubernetes/ceph/cluster.yaml
cephcluster.ceph.rook.io/rook-ceph created
</code>
</pre>

<pre>
<code>
3) kubeadmin@kworker01:~/installer$ kubectl apply -f ./rook/cluster/examples/kubernetes/ceph/operator.yaml
configmap/rook-ceph-operator-config created
deployment.apps/rook-ceph-operator created    
    </code>
</pre>



## 3. 설치된 pod 조회.  약 7분정도 필요



kubectl get pod -n rook-ceph

<pre>
    <code>
kubeadmin@kworker01:~/installer$ kubectl get pod -n rook-ceph
NAME                                           READY   STATUS    RESTARTS   AGE
csi-cephfsplugin-cmrr8                         3/3     Running   0          11m
csi-cephfsplugin-gzgnd                         3/3     Running   1          11m
csi-cephfsplugin-provisioner-558b4777b-h7f7q   4/4     Running   0          11m
csi-cephfsplugin-provisioner-558b4777b-pzlm2   4/4     Running   0          11m
csi-rbdplugin-fzvv7                            3/3     Running   1          11m
csi-rbdplugin-m7dgw                            3/3     Running   1          11m
csi-rbdplugin-provisioner-55494cc8b4-n677c     5/5     Running   0          11m
csi-rbdplugin-provisioner-55494cc8b4-rn8gx     5/5     Running   0          11m
rook-ceph-mon-a-canary-77df4c47c9-ls6wt        1/1     Running   0          103s
rook-ceph-mon-b-canary-85b8d98df8-885nv        0/1     Pending   0          103s
rook-ceph-operator-85bcfbf89-9jvxl             1/1     Running   0          12m
rook-discover-v5x6d                            1/1     Running   0          11m
rook-discover-zm5hf                            1/1     Running   0          11m    
    </code>
</pre>



## 4. MDS POD가 배포 (Shared Storage 옵션 변경 가능)

<pre>
    <code>
kubeadmin@kworker01:~/installer$ kubectl apply -f rook/cluster/examples/kubernetes/ceph/filesystem.yaml
cephfilesystem.ceph.rook.io/myfs created
    </code>
</pre>



## 5. StorageClass Driver 적용

kubectl apply -f rook/cluster/examples/kubernetes/ceph/csi/cephfs/storageclass.yaml



<pre>
    <code>
kubeadmin@kworker01:~/installer$  kubectl apply -f rook/cluster/examples/kubernetes/ceph/csi/cephfs/storageclass.yaml
storageclass.storage.k8s.io/csi-cephfs created
    </code>
</pre>



## 6. Shared Storage 확인

kubectl get CephFileSystem -A

<pre>
    <code>
kubeadmin@kworker01:~/installer$  kubectl get CephFileSystem -A
NAMESPACE   NAME   ACTIVEMDS   AGE
rook-ceph   myfs   1           39s
    </code>
</pre>



