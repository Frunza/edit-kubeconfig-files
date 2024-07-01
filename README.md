# Edit kubeconfig files

## Motivation

When working with `kubernetes` you usually have more clusters in your `kubeconfig` file. How should you best add, remove and update contexts?

## Prerequisites

A Linux or MacOS machine for local development. If you are running Windows, you first need to set up the *Windows Subsystem for Linux (WSL)* environment.

You need [kubecm](https://kubecm.cloud/), [kubectl](https://kubernetes.io/docs/reference/kubectl/) and [kubectx](https://github.com/ahmetb/kubectx) on your machine.

Have at least one cluster added to your `kubeconfig` file, and one other ready to use for add or update.

## Usage

First of all, let's view your `kubeconfig` file:
```sh
kubectl config view
```

If you have more clusters available, you will notice the structure: the `kubeconfig` file contains a list of clusters, contexts and users. For a configuration, the name of the cluster, context and user is identic. While theoretically, it is possible to work manually with the values of those components, it is very easy to mess up.

If you want to add a new context from one of your new clusters, you usually download or obtain the configuration in some way; let's say it is called *myNewConfig.yml*

To add this new configuration to your `kubeconfig` file you can:
- navigate from the command line to the location of *myNewConfig.yml*
- call:
```sh
kubecm add -f myNewConfig.yml
```

If the configuration already exists in your `kubeconfig`, `kubecm` will not overwrite any existing values. You will get some options to rename the context components, but providing the same value will just force the tool to add some random text in the name of a new configuration. The best thing to do in such a case is to delete your current configuration first and add it again with the new values.

To remove a configuration for *cluster1* for example, you can call:
```sh
kubectl config delete-cluster cluster1
kubectl config delete-context cluster1
kubectl config delete-user cluster1
```

These commands will delete the *cluster1* configuration.

So, to update a configuration, you should first delete it, and afterwards add it again.

Tip: If you use the same `kubernetes` distribution for different clusters, usually the default context has the same name, like *cluster1* as used in the examples. In such a scenario, it is a good idea to rename the configuration to something else like *devCluster*, *productionCluster*, etc. You can rename the context components (cluster, context and user)in the obtained `kubeconfig` file just before using the `add` command.

To view all your contexts in your `kubeconfig` file, you can run the following command:
```sh
kubectx
```

To change the cluster in your `kubeconfig` file, for *cluster1* for example, you can use the following command:
```sh
kubectx cluster1
```
