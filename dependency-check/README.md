# github-release-action

This GitHub Action performs the standard pattern of a Java/Maven-based Pull Request pipeline

## Usage

See [action.yml](action.yml).

### Inputs

| Name                        | Description                                                                            | Required | Default                       |
|-----------------------------|----------------------------------------------------------------------------------------|----------|-------------------------------|
| `application_name`          | Name of the Application. Drives Docker.                                                | yes      |                               |
| `dockerhub_username`        | Username to DockerHub for extended pull limits                                         | no       |                               |
| `dockerhub_token`           | Personal Access Token to DockerHub                                                     | no       |                               |
| `java_version`              | Version of Java to build with                                                          | no       | 8                             |
| `java_distro`               | Distribution of Java to use                                                            | no       | temurin                       |
| `java_cache`                | Build Cache. `maven`, `gradle`, `sbt`, ''                                              | no       | maven                         |
| `require_assume_role`       | Forces the pipeline to assume a role with the pattern `toolbox-${GITHUB_REPO}-ci-role` | no       | false                         |
| `save_logs`                 | Indicates to upload test logs to GitHub                                                | no       | false                         |
| `aws_credential_timeout`    | Lifetime of AWS credentials (seconds)                                                  | no       | 1800                          |
| `build_command`             | Command to build package                                                               | no       |                               |
| `docker_context`            | Location to start the Docker context                                                   | no       |                               |
| `launch_command`            | Command to launch application for tests                                                | no       |                               |
| `integration_tests_command` | Command to launch integration tests                                                    | no       |                               |
| `test_files`                | Line-delimited list of file patterns to check. Supports *, **, ?, !, and []            | no       | ./target/*-reports/TEST-*.xml |
| `cleanup_command`           | Command to cleanup Docker mess                                                         | no       |                               |
| `falcon-client-id`           | falcon-client-id to authorize with Crowdstrike                                                         | yes       |                               |
| `falcon-client-secret`           | falcon-client-secret to authorize with Crowdstrike                                                         | yes       |                               |

### Example

```yaml
- name: Pull Request Pattern
  uses: AirVantage/java-pull-request-pattern@v60
  with:
    application_name: auth-server
    build_command: bash ./release/auth-server.sh build
    docker_context: ./release/docker
    launch_command: bash ./release/auth-server.sh docker-run
    integration_tests_command: bash ./release/auth-server.sh tests
    cleanup_command: |
      docker rm -f dynamodb
      docker rm -f mysql
      docker rm -f avsched
    falcon-client-id: ${{ secrets.DOCKER_FALCON_CLIENT_ID }}  # organization secrets
    falcon-client-secret: ${{ secrets.DOCKER_FALCON_CLIENT_SECRET }} # organization secrets
```
