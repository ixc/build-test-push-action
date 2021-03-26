# Build, test, and push Docker images

A composite run steps action (see [GitHub Actions]) to build a Docker image, test via Docker Compose, and push on success.

Use this instead of the official [Build and push Docker images] action when you want to run tests via `docker-compose` and not push the image if the tests fail.

## Usage

```yaml
name: Continuous Integration

on: push

jobs:
  image:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: docker/login-action@v1
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_ACCESS_TOKEN }}
      - uses: ixc/build-test-push-action@v1
        env:
          FOO: BAR
        inputs:
          build-options: --build-args FOO=BAR
          build-path: .
          compose-file: docker-compose.test.yml
          compose-options: -e FOO
          compose-project-name: sha-${{ github.sha }}
          compose-test-service: run-tests
          image-repo: ${{ github.repository }}
          image-tag: sha-${{ github.sha }}
```

[Build and push Docker images]: https://github.com/marketplace/actions/build-and-push-docker-images
[GitHub Actions]: https://github.com/features/actions
