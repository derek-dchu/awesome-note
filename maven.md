# Maven

### Common tasks and activities
* Multiple jars
* Dependencies and versions
* Project structure
* Building, publishing and deploying

#### Archetype Info
* create directory structure

#### Dependency Info

### What Maven Do
Maven Repository

#### Project Template (Archetype)
1. create an archetype
```shell
$: archetype:generate
```
  * Group ID:
  * Artifact ID:
  * Version: 
  * Package:

##### The `pom.xml` file
* Maven co-ordinates
* Metadata
* Build information
* Resources and Dependencies

```xml
	<groupId></groupId>
	<artifactId></artifactId>
	<version></version>
	<packaging></packaging>

	<name></name>
	<url></url>

	<!-- when to use the dependency -->
	<dependencies>
		<dependency>
			<scope></scope>
		</dependency>
	<dependencies>
```

* scope: in which phase the dependency is available

#### Maven Build (compile and package)
* Previous phases need to be run successfully in order to run current phase.

##### Build Lifecycle Phases
* validate
* compile
* test
* package
* install: publish an artifect to a local maven repository.
* deploy: publish an artifect to remote maven repository.

* clean: remove all jobs have been done. (e.g. remove target folder)

#### Maven Plugins
* We provide plugin block in `pom.xml` to overwrite default configuration.

```xml
pom.xml

<build>
	<plugins>
		<plugin>
			<groupId></groupId>
			<artifactId></artifactId>
			<version></version>
			<configuration></configuration>
		</plugin>
	</plugins>
</build>
```

* To use plugin
```shell
$ mvn plugin_name:supported_command
```

##### Maven-compiler-plugin
```xml
<!-- config compiler version -->
<plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-compiler-plugin</artifactId>
	<version>2.0.2</version>
	<configuration>
		<source>1.4</source>
		<target>1.4</target>
	</configuration>
</plugin>
```







