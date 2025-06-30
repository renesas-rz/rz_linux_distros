# rz_linux_distros

This is a documentation sources for [rz_linux_distros](https://renesas-rz.github.io/rz_linux_distros) website (github.io).

## Prepare environment

Python3 and pip3 are required.

In some case you may need to make a `symlink` to pip (of python3).

### Setup from script

Working directory should be `rz_linux_distros/`.

```
. work/setup.sh
```

### Serve local server

Working directory should be `rz_linux_distros/`.

```
. serve.sh
```

## Test versioning with `mike`

By default, versioning is unused in this project.

To use versioning properly,
please un-comment below config ([mkdocs.yml](mkdocs.yml) > `extra:`) first.

```
  # version:
  #   provider: mike
```

Working directory should be `rz_linux_distros/`.

### Deploy

This will generate a static website into `local/gh-pages` branch.

`dev` will be used as version.

```
. mike/deploy.sh
```

### Serve

You must deploy first.

```
. mike/serve.sh
```

### Clean-up

This step is not mandatory.

After you complete checking your website, you may want to remove `local/gh-pages` branch.

```
. mike/remove.sh
```
