# Config Stream Encoder

* Sometimes you need to stream configs, including binary, to action jobs 
  * Obfuscating also secrets, etc

Examples of this pattern include the following use cases:

## Terraform configuration

* Need to get configs for Terraform execution without exposing secrets

```yaml
jobs:

  build-env:
    name: 🔑 build-config
    runs-on: devsecops
    steps:
      - name: Make .env file with the params required for the build
        uses: seceng-devsecops-platform/devsecops-platform-github-workflows/actions/build/config-stream@main
        with:
          file-paths: |
            file1.do
            file2.config
            file4.binary
          dir-paths: |
            dir1
            dir2
```

* Now, the connection between the job that created the .env file `docker-compose-build-envs: .extra.env` is the name that must be provided as a param of the build
* The build will use the param above to create a .env file with specific build params to the CloudNative build done by Vionix build

```yaml
  docker-devsecops:
    name: 🐳 docker
    needs: [build-env]
    secrets: inherit
    with:
      docker-compose-file: docker-compose.yaml
      docker-compose-context: actions/processor
      docker-compose-service: ghas-operator-onborard-processor
      # Adds extra build envs into the docker compose context dir
      docker-compose-build-envs: .extra.env
      # Dockerfile scans for the project
      docker-build-sonarqube-project-id: seceng-devsecops-platform_viasat-ghas-k8s-operator_AYvOp1hAPmv_YrGp3xfu
```
