## Example Caller
```yml
jobs:
  build-apache:
    uses: alchatti/.github/.github/workflows/docker-build-push.yml@main
    with:
      context: .
      dockerfile: ./apache/Dockerfile
      tags: |
        alchatti/dummy:8.4-apache-bookworm
        alchatti/dummy:8.4-apache-latest
      build_args: |
        PHP=8.4
        OS=bookworm
      test_command: |
        docker run -d --name dummy-test -p 8080:8080 "$IMAGE"
        sleep 10
        docker logs dummy-test
        docker exec dummy-test php -v
        docker exec dummy-test php -m | grep -Ei 'gd|opcache|pdo_mysql|pdo_pgsql|zip'
        docker exec dummy-test apache2 -v
        docker exec dummy-test php-fpm -v
        docker exec dummy-test curl -fsS http://127.0.0.1:8080/ || true
        docker rm -f dummy-test
```
