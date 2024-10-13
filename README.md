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

```

Also, add this section for MyBatis Generator configuration to your build.gradle:
```gradle
mybatisGenerator {
verbose = true
configFile = file("src/main/resources/generatorConfig.xml")
}

task runMybatisGenerator(type: JavaExec) {
main = 'org.mybatis.generator.api.ShellRunner'
classpath = sourceSets.main.runtimeClasspath
args = ['-configfile', "$projectDir/src/main/resources/generatorConfig.xml", '-overwrite']
}
```
Step 2: Create the MyBatis Generator Configuration File
In your project, create the file src/main/resources/generatorConfig.xml and populate it with the following configuration (tailored for PostgreSQL):
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <context id="PostgreSQLTables" targetRuntime="MyBatis3">

        <!-- Database Connection Information -->
        <jdbcConnection driverClass="org.postgresql.Driver"
                        connectionURL="jdbc:postgresql://localhost:5444/test1"
                        userId="username"
                        password="password"/>

        <!-- Java Model (POJOs) Package -->
        <javaModelGenerator targetPackage="demo.mybatiscodegendatabasefirstapproach1.generatedClasses.models" 
                            targetProject="src/main/java"/>

        <!-- SQL Mapper Interface Package -->
        <sqlMapGenerator targetPackage="demo.mybatiscodegendatabasefirstapproach1.generatedClasses.mappers" 
                         targetProject="src/main/resources"/>

        <!-- XML Mapper Package -->
        <javaClientGenerator type="XMLMAPPER" 
                             targetPackage="demo.mybatiscodegendatabasefirstapproach1.generatedClasses.mappers" 
                             targetProject="src/main/java"/>

        <!-- Table Configuration -->
        <table tableName="books" domainObjectName="Book"/>
    </context>
</generatorConfiguration>
```
Step 3: Run MyBatis Generator
To generate the Java classes using MyBatis, run the following command:
```cmd 
./gradlew runMybatisGenerator```

Step 4: Ensure Database and Table Match Configuration
Make sure you have an existing SQL database and tables that match the configuration. For example, there must be a table named books in the database:
```<table tableName="books" domainObjectName="Book"/>```

Additional Information
For more details on available runtimes, visit the MyBatis Generator Quickstart page.
The database tables should already exist to match the table configuration in generatorConfig.xml.
