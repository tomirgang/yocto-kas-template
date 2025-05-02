# Yocto Kas DevContainer Template

This repo provides a template for an DevContainer based [Yocto](https://docs.yoctoproject.org/5.0.8/) workspace
using [Kas](https://kas.readthedocs.io/en/latest/index.html) as setup tool.

## Manual use container

- Build the container:
```bash
docker build \
    -t yocto_kas_ub2204 \
    --build-arg HOST_USER=$(id -u) \
    --build-arg HOST_GROUP=$(id -g) \
    .devcontainer
```
- Run the container:
```bash
docker run --rm -it \
    -u developer \
    -v ~/.ssh:/home/developer/.ssh \
    -v ~/.gnupg:/home/developer/.gnupg \
    -v .:/workspace \
    -w /workspace \
    yocto_kas_ub2204 \
    bash
```
