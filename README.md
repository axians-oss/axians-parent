# Axians Parent

This is the über parent for all Maven based Java projects in Axians Netherlands. It contains the common settings and plugins for all projects.

## Usage
To use this parent, add the following to your project's `pom.xml`:
```xml
<parent>
    <groupId>nl.axians</groupId>
    <artifactId>axians-parent</artifactId>
    <version>5</version>
</parent>
```
The default Java version being used is 21. You can override this by setting the `maven.compiler.source`, `maven.compiler.target` and `maven.compiler.release` properties in your project's `pom.xml`. To use Java 17 for example you can add the following properties in your `pom.xml`:
```xml
<properties>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
    <maven.compiler.release>17</maven.compiler.release>
</properties>
```
The default encoding is UTF-8. You can override this by setting the `project.build.sourceEncoding` and `project.reporting.outputEncoding` properties in your project's `pom.xml`. To use ISO-8859-1 for example you can add the following properties in your `pom.xml`:
```xml
<properties>
    <project.build.sourceEncoding>ISO-8859-1</project.build.sourceEncoding>
    <project.reporting.outputEncoding>ISO-8859-1</project.reporting.outputEncoding>
</properties>
```

## Profiles
The following profiles are available:
- `private-release` - This profile is used to release the project to a private Git artifact repository. It will enable the `maven-release-plugin` to create and deploy a release of your project. It will also activate the `maven-javadoc-plugin` to generate the Javadoc and the `maven-source-plugin` to attach the source code to the release. It expects the `<scm>`, `<repository>` and `<distributionManagement>` sections to be present in the project's pom.xml.

- `public-release` - This profile is used to release the project to a public Git and the public Maven artifact repository. It will enable the `maven-release-plugin` to create and deploy a release of your project. It will also activate the `maven-javadoc-plugin` to generate the Javadoc and the `maven-source-plugin` to attach the source code to the release. It will add and configure the `central-publishing-maven-plugin` will be configured to publish the artifacts to the Sonatype public Maven repository. The artifacts will be signed using the `maven-gpg-plugin`. You also need to add a `<server>` tag in your `settings.xml` with the id `ossrh` and your Sonatype token username and password. **Note**: This profile currently does not work in a CI pipeline as it requires user input to enter the GPG passphrase.

- `qa` - This profile is used to run the quality analysis tools. It will enable the following plugins:
  - `dependency-check-maven` to check for OWASP vulnerabilities.
  - `jacoco-maven-plugn` to analyse test coverage.
  - `sonar-maven-plugin` to perform SonarQube code analysis.
  - `owasp-sonar-maven-plugin` to transform the OWASP vulnerabilities report generated by the `dependency-check-maven` to a format that can be uploaded to SonarCloud.

- `docker` - This profile is used to build a simple docker image of your Java application. It uses the `kubernetes-maven-plugin` for this using the `eclipse-temurin:21-jre-alpine` base image. Only port 8080 will be exposed and no agents will be installed. You should add this profile to the `release-private` or `release-public` goal to build the docker image during the release process.

## License
This project is licensed under the Apache License 2.0. See the [LICENSE](LICENSE.md) file for details.


