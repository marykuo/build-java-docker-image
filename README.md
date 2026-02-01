# build-java-docker-image

Building a Docker image for a Java application using Maven

## 使用方式

### Docker Hub

```yaml
steps:
  - name: Build Java application and Push Docker Image
    uses: marykuo/build-java-docker-image@v2
    with:
      java-version: 17
      maven-version: 3.9.6
      docker-registry: docker.io
      docker-username: ${{ secrets.DOCKERHUB_USERNAME }}
      docker-password: ${{ secrets.DOCKERHUB_PASSWORD }}
      image-name: docker.io/your-username/your-repo-name
      image-tag: tag
      publish-latest: false
      dockerfile-path: ./Dockerfile
```

### Private Docker Registry

```yaml
steps:
  - name: Build Java application and Push Docker Image
    uses: marykuo/build-java-docker-image@v1
    with:
      java-version: 17
      maven-version: 3.9.6
      docker-registry: my-nexus.example.com
      docker-username: ${{ secrets.NEXUS_USERNAME }}
      docker-password: ${{ secrets.NEXUS_PASSWORD }}
      image-name: my-nexus.example.com/your-repo-name/your-image-name
      image-tag: tag
      publish-latest: false
      dockerfile-path: ./Dockerfile
```

### Google Artifact Registry (GAR)

```yaml
steps:
  - name: Build Java application and Push Docker Image
    uses: marykuo/build-java-docker-image@v1
    with:
      java-version: 17
      maven-version: 3.9.6
      docker-registry: <location>-docker.pkg.dev
      docker-username: _json_key
      docker-password: ${{ secrets.GAR_JSON_KEY }}
      image-name: <location>-docker.pkg.dev/your-project-id/your-repo-name/your-image-name
      image-tag: tag
      publish-latest: true
      dockerfile-path: ./Dockerfile
```

## 腳本流程說明

1. 使用指定版本的 Java 和 Maven 版本 setup 環境。
1. 使用 `actions/checkout@v4` 來取得 git 相關資訊。
1. 使用 `mvn clean test` 進行測試。
1. 使用 `mvn clean package -DskipTests` Build Java application。
1. 使用指定的 Dockerfile 建立 Docker Image，並 push 到指定的 Docker Registry。
