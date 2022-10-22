# Kpack Dependencies

This project provides a [Carvel package](https://carvel.dev/kapp-controller/docs/latest/packaging) with a set of buildpacks, stacks, and builders to use with [kpack](https://github.com/pivotal/kpack), a Kubernetes-native implementation of [Cloud Native Buildpacks](https://buildpacks.io). It relies on the [Paketo Buildpacks](https://paketo.io), which provide support for multiple languages and frameworks, including Java, Spring, GraalVM, Go, Python, NodeJs, and more.

## Components

* kpack-dependencies

## Prerequisites

* Install the [`kctrl`](https://carvel.dev/kapp-controller/docs/latest/install/#installing-kapp-controller-cli-kctrl) CLI to manage Carvel packages in a convenient way.
* Ensure [kapp-controller](https://carvel.dev/kapp-controller) is deployed in your Kubernetes cluster. You can do that with Carvel
[`kapp`](https://carvel.dev/kapp/docs/latest/install) (recommended choice) or `kubectl`.

```shell
kapp deploy -a kapp-controller -y \
  -f https://github.com/vmware-tanzu/carvel-kapp-controller/releases/latest/download/release.yml
```

## Dependencies

Kpack Dependencies requires the Kpack package to be already installed in the cluster. You can install it
from the [Kadras package repository](https://github.com/arktonix/carvel-packages).

## Installation

You can install the Kpack Dependencies package directly or rely on the [Kadras package repository](https://github.com/arktonix/carvel-packages)
(recommended choice).

Follow the [instructions](https://github.com/arktonix/carvel-packages) to add the Kadras package repository to your Kubernetes cluster.

If you don't want to use the Kadras package repository, you can create the necessary `PackageMetadata` and
`Package` resources for the Kpack Dependencies package directly.

```shell
kubectl create namespace carvel-packages
kapp deploy -a kpack-dependencies-package -n carvel-packages -y \
    -f https://github.com/arktonix/kpack-dependencies/releases/latest/download/metadata.yml \
    -f https://github.com/arktonix/kpack-dependencies/releases/latest/download/package.yml
```

Either way, you can then install the Kpack Dependencies package using [`kctrl`](https://carvel.dev/kapp-controller/docs/latest/install/#installing-kapp-controller-cli-kctrl).

```shell
kctrl package install -i kpack-dependencies \
    -p kpack-dependencies.packages.kadras.io \
    -v 0.1.0 \
    -n carvel-packages
```

You can retrieve the list of available versions with the following command.

```shell
kctrl package available list -p kpack-dependencies.packages.kadras.io
```

You can check the list of installed packages and their status as follows.

```shell
kctrl package installed list -n carvel-packages
```

## Configuration

The Kpack Dependencies package has the following configurable properties.

| Config | Default | Description |
|-------|-------------------|-------------|
| `kp_default_repository` | `""` | The OCI registry where to publish builder images. The same as configured in kpack. **Required**. |

You can define your configuration in a `values.yml` file.

```yaml
kp_default_repository: ghcr.io/arktonix/kpack-build
```

Then, reference it from the `kctrl` command when installing or upgrading the package.

```shell
kctrl package install -i kpack-dependencies \
    -p kpack-dependencies.packages.kadras.io \
    -v 0.1.0 \
    -n carvel-packages \
    --values-file values.yml
```

## Documentation

For documentation specific to kpack, check out [https://github.com/pivotal/kpack](https://github.com/pivotal/kpack).

## References

This package is inspired by the [Kpack Dependencies](https://github.com/vmware-tanzu/package-for-kpack-dependencies) package used in Tanzu Community Edition before its retirement.

## Supply Chain Security

This project is compliant with level 2 of the [SLSA Framework](https://slsa.dev).

<img src="https://slsa.dev/images/SLSA-Badge-full-level2.svg" alt="The SLSA Level 2 badge" width=200>
