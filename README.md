# MyBatisCodegenDatabaseFirstApproach1

**MyBatis ORM Framework Demo** with code generator that uses local database data to generate Java classes.

## Overview

To start using MyBatis Generator (MBG) with your Gradle project, especially for PostgreSQL, here's a comprehensive approach to integrating MBG into your Gradle project.

## 1. MyBatis Code Generator Configuration

The `application.yml` file is used at runtime for Spring Boot configuration, but it won’t be used by MyBatis Generator. You’ll need to explicitly create a MyBatis Generator configuration file to drive the code generation process. **MBG generates the code based on this configuration**, and it won’t create the `mybatis-config.xml` for you.

## 2. Steps to Configure MyBatis Generator in a Gradle Project

You have two main options for using MyBatis Generator:

- **Approach 1**: Using a Gradle Plugin for MBG (this approach is covered below).
- **Approach 2**: Running MBG directly from the command line (with downloaded JARs) - Approach 2 is not presented here.

### Approach 1: Using MyBatis Generator Gradle Plugin

#### Step 1: Add the MyBatis Generator Gradle Plugin

In general, the official MBG plugin is developed for Eclipse IDE and Maven. However, for Gradle in IntelliJ, I found several plugins. I decided to use:

```gradle
id "com.teleniasoftware.MybatisGenerator" version "3.0.0"

This plugin has a history of development and works well with Gradle. Add the following minimum dependencies to your build.gradle file:
dependencies {
    implementation 'org.mybatis.spring.boot:mybatis-spring-boot-starter:3.0.3'
    developmentOnly 'org.springframework.boot:spring-boot-devtools'
    runtimeOnly 'org.postgresql:postgresql'
    implementation 'org.mybatis.generator:mybatis-generator-core:1.4.0'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testImplementation 'org.mybatis.spring.boot:mybatis-spring-boot-starter-test:3.0.3'
    testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
}

Additional Information
For more details on available runtimes, visit the MyBatis Generator Quickstart page.
The database tables should already exist to match the table configuration in generatorConfig.xml.
