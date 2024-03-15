# Config Stream Decoder

* Decoding binary from github actions

Examples of this pattern include the following use cases:

## Terraform configuration

* Take a look at the config stream decoder to learn more about how to use it to decode

```yaml

      # https://github.com/docker/build-push-action/issues/225#issuecomment-727639184
      - name: Decode the config stream
        uses: seceng-devsecops-platform/devsecops-platform-github-workflows/actions/build/config-stream-decoder@main
        with:
          config-stream: ${{ steps.config.outputs.base64-stream }}

```

* This is for an input like encoding values

```yaml
jobs:

      - id: config
        name: Make .env file with the params required for the build
        uses: seceng-devsecops-platform/devsecops-platform-github-workflows/actions/build/config-stream-encoder@main
        with:
          file-paths: |
            file1.do
            file2.config
            file4.binary
          dir-paths: |
            dir1
            dir2
```
