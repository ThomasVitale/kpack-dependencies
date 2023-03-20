# Kpack Dependencies

![Test Workflow](https://github.com/kadras-io/kpack-dependencies/actions/workflows/test.yml/badge.svg)
![Release Workflow](https://github.com/kadras-io/kpack-dependencies/actions/workflows/release.yml/badge.svg)
[![The SLSA Level 3 badge](https://slsa.dev/images/gh-badge-level3.svg)](https://slsa.dev/spec/v0.1/levels)
[![The Apache 2.0 license badge](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Follow us on Twitter](https://img.shields.io/static/v1?label=Twitter&message=Follow&color=1DA1F2)](https://twitter.com/kadrasIO)

> **Warning**
> This package has been replaced by [buildpacks-catalog](https://github.com/kadras-io/buildpacks-catalog).

A Carvel package providing a set of buildpacks, stacks, and builders to use with [kpack](https://github.com/kadras-io/package-for-kpack), a Kubernetes-native implementation of [Cloud Native Buildpacks](https://buildpacks.io) to build application source code into OCI images.

This package relies on the [Paketo Buildpacks](https://paketo.io) implementation, which provide support for multiple languages and frameworks, including Java, Spring, GraalVM, Go, Python, NodeJs, and more.

## 🚀&nbsp; Getting Started

### Prerequisites

* Kubernetes 1.24+
* Carvel [`kctrl`](https://carvel.dev/kapp-controller/docs/latest/install/#installing-kapp-controller-cli-kctrl) CLI.
* Carvel [kapp-controller](https://carvel.dev/kapp-controller) deployed in your Kubernetes cluster. You can install it with Carvel [`kapp`](https://carvel.dev/kapp/docs/latest/install) (recommended choice) or `kubectl`.

  ```shell
  kapp deploy -a kapp-controller -y \
    -f https://github.com/carvel-dev/kapp-controller/releases/latest/download/release.yml
  ```

### Dependencies

Kpack Dependencies requires the [kpack](https://github.com/kadras-io/package-for-kpack) package. You can install it from the [Kadras package repository](https://github.com/kadras-io/kadras-packages).

### Installation

Add the Kadras [package repository](https://github.com/kadras-io/kadras-packages) to your Kubernetes cluster:

  ```shell
  kubectl create namespace kadras-packages
  kctrl package repository add -r kadras-packages \
    --url ghcr.io/kadras-io/kadras-packages \
    -n kadras-packages
  ```

<details><summary>Installation without package repository</summary>
The recommended way of installing the kpack-dependencies package is via the Kadras <a href="https://github.com/kadras-io/kadras-packages">package repository</a>. If you prefer not using the repository, you can add the package definition directly using <a href="https://carvel.dev/kapp/docs/latest/install"><code>kapp</code></a> or <code>kubectl</code>.

  ```shell
  kubectl create namespace kadras-packages
  kapp deploy -a kpack-dependencies-package -n kadras-packages -y \
    -f https://github.com/kadras-io/kpack-dependencies/releases/latest/download/metadata.yml \
    -f https://github.com/kadras-io/kpack-dependencies/releases/latest/download/package.yml
  ```
</details>

Install the Kpack Dependencies package:

  ```shell
  kctrl package install -i kpack-dependencies \
    -p kpack-dependencies.packages.kadras.io \
    -v ${VERSION} \
    -n kadras-packages
  ```

> **Note**
> You can find the `${VERSION}` value by retrieving the list of package versions available in the Kadras package repository installed on your cluster.
> 
>   ```shell
>   kctrl package available list -p kpack-dependencies.packages.kadras.io -n kadras-packages
>   ```

Verify the installed packages and their status:

  ```shell
  kctrl package installed list -n kadras-packages
  ```

## 📙&nbsp; Documentation

Documentation, tutorials and examples for this package are available in the [docs](docs) folder.
For documentation specific to kpack, check out [github.com/pivotal/kpack](https://github.com/pivotal/kpack).

## 🎯&nbsp; Configuration

The Kpack Dependencies package can be customized via a `values.yml` file.

  ```yaml
  kp_default_repository:
    name: ghcr.io/thomasvitale/kpack
  ```

Reference the `values.yml` file from the `kctrl` command when installing or upgrading the package.

  ```shell
  kctrl package install -i kpack-dependencies \
    -p kpack-dependencies.packages.kadras.io \
    -v ${VERSION} \
    -n kadras-packages \
    --values-file values.yml
  ```

### Values

The Kpack Dependencies package has the following configurable properties.

<details><summary>Configurable properties</summary>

| Config | Default | Description |
|-------|-------------------|-------------|
| `kp_default_repository.name` | `""` | The default repository to use for builder images and dependencies. For example, GitHub Container Registry: `ghcr.io/my-org/my-repo`; GCR: `gcr.io/my-project/my-repo`; Harbor: `myharbor.io/my-project/my-repo`, Dockerhub: `docker.io/my-username/my-repo`.|

</details>

## 🛡️&nbsp; Security

The security process for reporting vulnerabilities is described in [SECURITY.md](SECURITY.md).

## 🖊️&nbsp; License

This project is licensed under the **Apache License 2.0**. See [LICENSE](LICENSE) for more information.

## 🙏&nbsp; Acknowledgments

This package is inspired by the original kpack-dependencies package used in the [Tanzu Community Edition](https://github.com/vmware-tanzu/community-edition) project before its retirement.
