# Dot Env File Generator for Docker builds

* Sometimes you need to create env vars as input to docker builds
* An external job to the DevSecOps builds can be introduced to generate such env vars

# Example 1: Golang Builds

* We require credentials to build other docker images with Golang where the Artifactory Proxy server is located
  * This uploads the generated k=v file to be used as a `.env` file for `docker buildx`

```yaml
jobs:

  build-env:
    name: 🔑 build-envs
    runs-on: devsecops
    steps:
      - name: Make .env file with the params required for the build
        uses: seceng-devsecops-platform/devsecops-platform-github-workflows/actions/build/env-pairs-to-dot-env@main
        with:
          dot-file-artifact-name: .extra.env
          dot-file-path: .extra.env
          dot-file-content: |
            # Required environment variables to pull Golang dependencies from the Operator
            ARTIFACTORY_API_HOST_URL_PATH=artifactory.viasat.com/artifactory
            # The artifactory user that can be used to retrieve data from
            # Needs to have access to the virtual repository
            ARTIFACTORY_USER=${{ secrets.ARTIFACTORY_SVC_USER }}
            # The API key used to retrieve the golang repository data
            ARTIFACTORY_API_KEY=${{ secrets.ARTIFACTORY_SVC_APIKEY }}
            # The jfrog repository where to pull data from using containers
            JFROG_REPO_VIRTUAL=vionix-devsecops-platform-golang
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
