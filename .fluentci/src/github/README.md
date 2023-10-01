# Github Actions

[![deno module](https://shield.deno.dev/x/codecov_pipeline)](https://deno.land/x/codecov_pipeline)
![deno compatibility](https://shield.deno.dev/deno/^1.34)
[![](https://img.shields.io/codecov/c/gh/fluent-ci-templates/codecov-pipeline)](https://codecov.io/gh/fluent-ci-templates/codecov-pipeline)


The following command will generate a `.github/workflows/codecov-updoad.yml` file in your project:

```bash
fluentci gh init -t codecov_pipeline
```

Or, if you already have a `.fluentci` folder (generated from `fluentci init -t codecov`) in your project:

```bash
fluentci gh init
```

Generated file:

```yaml
# Do not edit this file directly. It is generated by Fluent Github Actions

name: Codecov
on:
  push:
    branches:
      - main
jobs:
  codecov:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: denoland/setup-deno@v1
        with:
          deno-version: v1.37
      - name: Setup Fluent CI CLI
        run: deno install -A -r https://cli.fluentci.io -n fluentci
      - name: Setup Dagger
        run: |
            curl -L https://dl.dagger.io/dagger/install.sh | DAGGER_VERSION=0.8.1 sh
            sudo mv bin/dagger /usr/local/bin
            dagger version
      - name: Upload Coverage
        run: fluentci run codecov_pipeline
        env:
          CODECOV_TOKEN: ${{ secrets.CODECOV_TOKEN }}
```

Feel free to edit the template generator at `.fluentci/src/github/config.ts` to your needs.