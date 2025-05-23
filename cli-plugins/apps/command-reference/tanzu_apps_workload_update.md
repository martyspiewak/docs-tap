## Tanzu Apps Workload Update

Update configuration of an existing workload.

### Synopsis

Update configuration of an existing workload. To export the current configuration run 'tanzu apps workload export NAME'. 

Workload configuration options include:
- source code to build
- runtime resource limits
- environment variables
- services to bind

```
tanzu apps workload update [name] [flags]
```

### Examples

```
tanzu apps workload update my-workload --debug=false
tanzu apps workload update my-workload --local-path .
tanzu apps workload update my-workload --env key=value
tanzu apps workload update --file workload.yaml
```

### <a id='update-options'>Options

```
      --app name                       application name the workload is a part of
      --debug                          put the workload in debug mode, --debug=false to disable
      --dry-run                        print kubernetes resources to stdout rather than apply them to the cluster, messages normally on stdout will be sent to stderr
      --env "key=value" pair           environment variables represented as a "key=value" pair, or "key-" to remove. This flag may be specified multiple times
  -f, --file file path                 file path containing the description of a single workload, other flags are layered on top of this resource
      --git-branch branch              branch within the git repo to checkout
      --git-commit SHA                 commit SHA within the git repo to checkout
      --git-repo url                   git url to remote source code
      --git-tag tag                    tag within the git repo to checkout
  -h, --help                           help for update
      --image image                    pre-built image, skips the source resolution and build phases of the supply chain
      --label "key=value" pair         label is represented as a "key=value" pair, or "key-" to remove. This flag may be specified multiple times
      --limit-cpu cores                the maximum amount of cpu allowed, in CPU cores (500m = .5 cores)
      --limit-memory bytes             the maximum amount of memory allowed, in bytes (500Mi = 500MiB = 500 * 1024 * 1024)
      --live-update                    put the workload in live update mode, --live-update=false to disable
      --local-path path                path on the local file system to a directory of source code to build for the workload
  -n, --namespace name                 kubernetes namespace (defaulted from kube config)
      --param "key=value" pair         additional parameters represented as a "key=value" pair, or "key-" to remove. This flag may be specified multiple times
      --service-ref object reference   object reference for a service to bind to the workload "database=rabbitmq.com/v1beta1:RabbitmqCluster:my-broker", or "database-" to delete. This flag may be specified multiple times.
      --source-image image             image containing source code to build
      --tail                           show logs while waiting for workload to become ready
      --tail-timestamp                 show logs and add timestamp to each log line while waiting for workload to become ready
      --type type                      distinguish workload type
      --wait                           waits for workload to become ready
      --wait-timeout duration          timeout for workload to become ready when waiting (default 10m0s)
  -y, --yes                            accept all prompts
```

### Options Inherited from Parent Commands

```
      --context name      name of the kubeconfig context to use (default is current-context defined by kubeconfig)
      --kubeconfig file   kubeconfig file (default is $HOME/.kube/config)
      --no-color          disable color output in terminals
  -v, --verbose int32     number for the log level verbosity (default 1)
```

### See Also

* [tanzu apps workload](tanzu_apps_workload.md)	 - Workload lifecycle management

