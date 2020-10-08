# Client-go equivalent of Kubectl

## Introduction
Kubectl is a CLI tool for interacting to a  Kubernetes cluster.
Similarly, client-go is a library for interacting to a Kubernetes cluster that 
can be used in go programs. Using client-go you can do CRUD operations on 
Kubernetes resources or fetch information regarding the Kubernetes server.
This document is kind of a cheat sheet to know the exact method of the client-go
library to manipulate a particular API in Kubernetes.

Client -go library can be found [here](https://github.com/kubernetes/client-go)

## Goal

Consider following: 
```bash 
$ kubectl version
```  
Above command prints the Kubernetes version. Now you are required to find
the Kubernetes version via a go program. Well, you can use methods available 
in client-go to do that.

Now obivously, we can explore the client-go and find out the right method. But
sometimes it becomes little of search/explore job especially for people who are
just getting started with Kubernetes and client-go in general.

The cheat sheet should ease the process. I call this `Cleqku` cheat sheet.

## Cleqku Cheat Sheet

We first need an `clientset` object that can help talk to Kubernetes API.

Following is the code to do it :

```go
    var kubeconfig *string
	if home := homedir.HomeDir(); home != "" {
		kubeconfig = flag.String("kubeconfig", filepath.Join(home, ".kube", "config"), "(optional) absolute path to the kubeconfig file")
	} else {
		kubeconfig = flag.String("kubeconfig", "", "absolute path to the kubeconfig file")
	}
	flag.Parse()

	// use the current context in kubeconfig
	config, err := clientcmd.BuildConfigFromFlags("", *kubeconfig)
	if err != nil {
		panic(err.Error())
	}
    // The clientset object can be used to talk to Kubernetes API
	clientset,err:=k8sclient.NewForConfig(config)
```

### **Fetching Kubernetes Server Version**

Kubectl Command : 

```bash
$ kubectl version
```
Cleqku :
 
```go
version,err:=clientset.Discovery().ServerVersion()
```

### **Fetching Kubernetes API Resources,Versions And Groups**

Kubectl Command : 

```bash
$ kubectl api-resource
$ kubectl api-versions
```
Cleqku :
 
```go
// Returns API group list
apiGrpList,err:=clientset.Discovery().ServerGroups()
// Returns API group and API resources list
apiGrpList,apiResList,err:=clientset.Discovery().ServerGroupsAndResources()
```

### **Fetching Kubernetes Resource List By GroupVersion**

Kubectl Command : 

```bash
N/A
```
Cleqku :
 
```go
groupVersion:="apps/v1"
// Returns list of resources in `apps` group and `v1` version
resList,err:=clientset.Discovery().ServerResourcesForGroupVersion(groupVersion)
```

### **Fetching Kubernetes Resources With The Server Preferred Version**

Kubectl Command : 

```bash
N/A
```
Cleqku :
 
```go
resList,err:=clientset.Discovery().ServerPreferredResources()
```

### **Fetching Kubernetes Namespaced Resources With The Server Preferred Version**

Kubectl Command : 

```bash
N/A
```
Cleqku :
 
```go
resList,err:=clientset.Discovery().ServerPreferredNamespacedResources()
```

### **CRUD Operation On Resources**
Note: 
- For CRUD operations on resources using Kubectl, please refer the
Kubectl [doc](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)

### **CRUD Operation On ConfigMap Resource**
Cleqku:
```go
ns:="exampleNamespace"
// cmClient will have methods available to do CRUD
cmClient:=clientset.CoreV1().ConfigMaps(ns)
```

### **CRUD Operation On Events Resource**

Cleqku:
```go
ns:="exampleNamespace"
// eventsClient will have methods available to do CRUD
eventsClient:=clientset.CoreV1().Events(ns)
```

### **CRUD Operation On Endpoints Resource**

Cleqku:
```go
ns:="exampleNamespace"
epClient:=clientset.CoreV1().Endpoints(ns)
```

### **CRUD Operation On LimitRanges Resource**

Cleqku:
```go
ns:="exampleNamespace"
lrClient:=clientset.CoreV1().LimitRanges(ns)
```

### **CRUD Operation On Namespaces Resource**

Cleqku:
```go
nsClient:=clientset.CoreV1().Namespaces()
```

### **CRUD Operation On Nodes Resource**

Cleqku:
```go
nodeClient:=clientset.CoreV1().Nodes()
```