# Test eap action for Github Actions
An action for Github Actions to verify the validity of an .eap package for Axis ACAP in CI/CD pipelines.

Use this after building an ACAP in your ci/cd pipelines to verify that the file was correctly build.

Example:

```yaml
name: Build hello world ACAP

on:
  push:

jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        arch: ["armv7hf", "aarch64"]
    steps:
      - name: Checkout the code
        uses: actions/checkout@v3

      - name: Build ACAP application
        uses: fixedit-ai/build-eap-action@main
        with:
          token: ${{ secrets.FAPP_TOKEN }}
          arch: ${{ matrix.arch }}
          dir: hello_world

      - name: Validate the content of the ACAP .eap file
        uses: fixedit-ai/test-eap-action@main
        with:
          token: ${{ secrets.FAPP_TOKEN }}
          arch: ${{ matrix.arch }}
          app-path: "hello_world/*.eap"
```

Example output from a job that fails when the application was not built for the expected architecture:

![cicd screenshot](https://github.com/fixedit-ai/test-eap-action/blob/images/images/screenshot.png?raw=true)


See also:
* [install-fappcli-action](https://github.com/fixedit-ai/install-fappcli-action)
* [build-eap-action](https://github.com/fixedit-ai/build-eap-action)