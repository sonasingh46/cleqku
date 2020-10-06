# Client-go equivalent of Kubectl

## Introduction
Kubectl is a CLI tool for interacting toa  Kubernetes cluster.
Similarly, client-go is a library for interacting to a Kubernetes cluster that 
can be used in go programs. Using client-go you can do CRUD operations on 
Kubernetes resources or fetch information regarding the Kubernetes server.
This document is kind of a cheat sheet to know the exact method of the client-go
library to manipulate a particular API in Kubernetes.

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

I call this `Cleqku` cheat sheet.

## Cleqku Cheat Sheet

