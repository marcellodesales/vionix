# Config Stream Encoder

* Sometimes you need to stream configs, including binary, to action jobs 
  * Obfuscating also secrets, etc

Examples of this pattern include the following use cases:

## Terraform configuration

* Need to get configs for Terraform execution without exposing secrets

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

* Take a look at the config stream decoder to learn more about how to use it to decode

```yaml

      # https://github.com/docker/build-push-action/issues/225#issuecomment-727639184
      - name: Decode the config stream
        uses: seceng-devsecops-platform/devsecops-platform-github-workflows/actions/build/config-stream-decoder@main
        with:
          config-stream: ${{ steps.config.outputs.base64-stream }}

```
