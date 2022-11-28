# Kpack Dependencies

This project provides a [Carvel package](https://carvel.dev/kapp-controller/docs/latest/packaging) with a set of buildpacks, stacks, and builders to use with [kpack](https://github.com/pivotal/kpack), a Kubernetes-native implementation of [Cloud Native Buildpacks](https://buildpacks.io). It relies on the [Paketo Buildpacks](https://paketo.io), which provide support for multiple languages and frameworks, including Java, Spring, GraalVM, Go, Python, NodeJs, and more.

## Prerequisites

* Kubernetes 1.24+
* Carvel [`kctrl`](https://carvel.dev/kapp-controller/docs/latest/install/#installing-kapp-controller-cli-kctrl) CLI.
* Carvel [kapp-controller](https://carvel.dev/kapp-controller) deployed in your Kubernetes cluster. You can install it with Carvel [`kapp`](https://carvel.dev/kapp/docs/latest/install) (recommended choice) or `kubectl`.

  ```shell
  kapp deploy -a kapp-controller -y \
    -f https://github.com/vmware-tanzu/carvel-kapp-controller/releases/latest/download/release.yml
  ```

## Dependencies

Kpack Dependencies requires the Kpack package to be already installed in the cluster. You can install it
from the [Kadras package repository](https://github.com/arktonix/kadras-packages).

## Installation

First, add the [Kadras package repository](https://github.com/arktonix/kadras-packages) to your Kubernetes cluster.

  ```shell
  kubectl create namespace kadras-packages
  kctrl package repository add -r kadras-repo \
    --url ghcr.io/arktonix/kadras-packages \
    -n kadras-packages
  ```

Then, install the Kpack Dependencies package.

  ```shell
  kctrl package install -i kpack-dependencies \
    -p kpack-dependencies.packages.kadras.io \
    -v 0.3.2 \
    -n kadras-packages
  ```

### Verification

You can verify the list of installed Carvel packages and their status.

  ```shell
  kctrl package installed list -n kadras-packages
  ```

### Version

You can get the list of Kpack Dependencies versions available in the Kadras package repository.

  ```shell
  kctrl package available list -p kpack-dependencies.packages.kadras.io -n kadras-packages
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
    -v 0.3.2 \
    -n kadras-packages \
    --values-file values.yml
  ```

## Upgrading

You can upgrade an existing package to a newer version using `kctrl`.

  ```shell
  kctrl package installed update -i kpack-dependencies \
    -v <new-version> \
    -n kadras-packages
  ```

You can also update an existing package with a newer `values.yml` file.

  ```shell
  kctrl package installed update -i kpack-dependencies \
    -n kadras-packages \
    --values-file values.yml
  ```

## Other

The recommended way of installing the Kpack Dependencies package is via the [Kadras package repository](https://github.com/arktonix/kadras-packages). If you prefer not using the repository, you can install the package by creating the necessary Carvel `PackageMetadata` and `Package` resources directly using [`kapp`](https://carvel.dev/kapp/docs/latest/install) or `kubectl`.

  ```shell
  kubectl create namespace kadras-packages
  kapp deploy -a kpack-dependencies-package -n kadras-packages -y \
    -f https://github.com/arktonix/kpack-dependencies/releases/latest/download/metadata.yml \
    -f https://github.com/arktonix/kpack-dependencies/releases/latest/download/package.yml
  ```

## Support and Documentation

For support and documentation specific to kpack, check out [github.com/pivotal/kpack](https://github.com/pivotal/kpack).

## References

This package is based on the original kpack-dependencies package used in [Tanzu Community Edition](https://github.com/vmware-tanzu/community-edition) before its retirement.

## Supply Chain Security

This project is compliant with level 2 of the [SLSA Framework](https://slsa.dev).

<img src="https://slsa.dev/images/SLSA-Badge-full-level2.svg" alt="The SLSA Level 2 badge" width=200>
