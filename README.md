# Java Maven Setup

![Latest Release](https://img.shields.io/github/v/release/p6m-actions/java-maven-setup?style=flat-square&label=Latest%20Release&color=blue)

## Description

A GitHub Action that sets up Java JDK with Maven dependency caching for faster builds. This action wraps the official `actions/setup-java` with additional Maven dependency caching and installation capabilities.

## Usage

```yaml
- name: Setup Java & Maven
  uses: p6m-actions/java-maven-setup@v1
  with:
    java-version: '21'
    java-distribution: 'temurin'
```

## Inputs

| Name | Description | Required | Default |
|------|-------------|----------|---------|
| `java-version` | Java JDK version to install | No | `21` |
| `java-distribution` | Java distribution to install | No | `temurin` |
| `maven-version` | Maven version to install (if not using wrapper) | No | |
| `cache` | Enable Maven dependency caching | No | `true` |
| `install-dependencies` | Install Maven dependencies after setup | No | `true` |
| `maven-cache-key-suffix` | Additional suffix for Maven cache key | No | |

## Outputs

| Name | Description |
|------|-------------|
| `java-version` | The installed Java JDK version |
| `maven-version` | The installed Maven version |
| `cache-hit` | Whether the Maven cache was hit |
| `maven-cache-dir` | Path to the Maven cache directory |

## Examples

### Basic Setup

```yaml
steps:
  - uses: actions/checkout@v4
  - name: Setup Java & Maven
    uses: p6m-actions/java-maven-setup@v1
    with:
      java-version: '21'
  - name: Build
    run: mvn compile
```

### With Specific Maven Version

```yaml
steps:
  - uses: actions/checkout@v4
  - name: Setup Java & Maven
    uses: p6m-actions/java-maven-setup@v1
    with:
      java-version: '17'
      maven-version: '3.9.6'
  - name: Build
    run: mvn package
```

### Multiple Java Versions

```yaml
strategy:
  matrix:
    java-version: ['17', '21', '22']
steps:
  - uses: actions/checkout@v4
  - name: Setup Java ${{ matrix.java-version }}
    uses: p6m-actions/java-maven-setup@v1
    with:
      java-version: ${{ matrix.java-version }}
  - name: Test
    run: mvn test
```

### Disable Caching

```yaml
steps:
  - uses: actions/checkout@v4
  - name: Setup Java & Maven
    uses: p6m-actions/java-maven-setup@v1
    with:
      java-version: '21'
      cache: false
  - name: Build
    run: mvn compile
```

### Skip Dependency Installation

```yaml
steps:
  - uses: actions/checkout@v4
  - name: Setup Java & Maven
    uses: p6m-actions/java-maven-setup@v1
    with:
      java-version: '21'
      install-dependencies: false
  - name: Custom dependency resolution
    run: mvn dependency:resolve -Dverbose
  - name: Build
    run: mvn compile -o
```

### Using Maven Wrapper

```yaml
steps:
  - uses: actions/checkout@v4
  - name: Setup Java & Maven
    uses: p6m-actions/java-maven-setup@v1
    with:
      java-version: '21'
  - name: Build with wrapper
    run: ./mvnw clean package
```