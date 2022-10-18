# Docker Images for Continuous Integration
The Dockerfiles under this directory serve as the source of the container images
used in GitHub Actions.

## manylinux

The images with the `manylinux` tag are used to lint, build, and test the
project. They have the tag format `fairseq2-ci-manylinux:<VERSION>-<VARIANT>`,
where `<VERSION>` is the current version of the image (e.g. `1`) and `<VARIANT>`
is either `cpu` or a CUDA version specifier (e.g. `cu113`).

The images are based of PyPA's
[manylinux2014](https://github.com/pypa/manylinux) to ensure maximum binary
compatibility with different Linux distributions.

### Deployment Instructions
As of this writing, all images are readily available in the
[ghcr.io/fairinternal](https://github.com/orgs/fairinternal/packages/container/package/fairseq2-ci-wheel)
registry. You should follow these instructions if, for any reason, an image
should be updated. In such case, make sure to increment `<VERSION>` in the
Dockerfile, in GA workflows, and in the docker commands below.

#### 1. Build the Docker Image
The `<VARIANT>` must be one of `cpu`, `cu113`, `cu116`, or `cu116-clang`.

```
docker build --network host --tag ghcr.io/fairinternal/fairseq2-ci-manylinux:<VERSION>-<VARIANT> -f Dockerfile.<VARIANT> .
```

#### 2. Push to the GitHub Container Registry
If you don't already have a Personal Access Token with read and write access to
the GitHub Container Registry, follow the steps
[here](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry).

First, log in to the registry:

```
docker login ghcr.io -u <GITHUB_USERNAME> --password-stdin
```

Then, push the image:

```
docker push ghcr.io/fairinternal/fairseq2-ci-manylinux:<VERSION>-<VARIANT>
```

Lastly, log out to avoid any accidental or malicious use of the registry:

```
docker logout ghcr.io
```