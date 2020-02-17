# Quebecoind

[Quebecoin](https://quebecoin.ca/) uses peer-to-peer technology to operate with no central authority or banks;
managing transactions and the issuing of quebecoins is carried out collectively by the network.

## Introduction

This chart bootstraps a single node Quebecoin deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.
Docker image was taken from [Quebecoind for Docker](https://github.com/containeriser/docker-quebecoin) - many thanks!

## Prerequisites

- Kubernetes 1.10+
- PV provisioner support in the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install --name my-release .
```

The command deploys quebecoind on the Kubernetes cluster in the default configuration.
The [configuration](#configuration) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following table lists the configurable parameters of the quebecoind chart and their default values.

Parameter                 	 	| Description                        				| Default
------------------------------- | ------------------------------------------------- | ----------------------------------------------------------
`image.repository`         		| Image source repository name       				| `containeriser/docker-quebecoin`
`image.tag`                		| `quebecoind` release tag.            				| `0.17.1`
`image.pullPolicy`         		| Image pull policy                  				| `IfNotPresent`
`service.rpcPort`          		| RPC port                           				| `8332`
`service.p2pPort`          		| P2P port                           				| `8333`
`service.testnetPort`      		| Testnet port                       				| `18332`
`service.testnetP2pPort`   		| Testnet p2p ports                  				| `18333`
`service.selector`         		| Node selector                      				| `tx-broadcast-svc`
`persistence.enabled`      		| Create a volume to store data      				| `true`
`persistence.accessMode`   		| ReadWriteOnce or ReadOnly          				| `ReadWriteOnce`
`persistence.size`         		| Size of persistent volume claim    				| `300Gi`
`resources`                		| CPU/Memory resource requests/limits				| `{}`
`configurationFile`        		| Config file ConfigMap entry      				    |
`terminationGracePeriodSeconds` | Wait time before forcefully terminating container | `30`

For more information about Quebecoin configuration please see [Quebecoin.conf_Configuration_File](https://en.bitcoin.it/wiki/Running_Bitcoin#Bitcoin.conf_Configuration_File).

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```bash
$ helm install --name my-release -f values.yaml .
```

> **Tip**: You can use the default [values.yaml](values.yaml)

## Persistence

The quebecoind image stores the Quebecoind node data (Blockchain and wallet) and configurations at the `/quebecoin` path of the container.

By default a PersistentVolumeClaim is created and mounted into that directory. In order to disable this functionality
you can change the values.yaml to disable persistence and use an emptyDir instead.

> *"An emptyDir volume is first created when a Pod is assigned to a Node, and exists as long as that Pod is running on that node. When a Pod is removed from a node for any reason, the data in the emptyDir is deleted forever."*

!!! WARNING !!!

Please NOT use emptyDir for production cluster! Your wallets will be lost on container restart!

## Customize quebecoind configuration file

```yaml
configurationFile:
  quebecoind.conf: |-
    server=1
    printtoconsole=1
    rpcuser=rpcuser
    rpcpassword=rpcpassword
```
