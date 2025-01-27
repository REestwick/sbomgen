# Creation of SBOM in Maven Project Via The CycloneDX-Maven-Plugin

## Introduction

This document illustrates the generation of an SBOM from a maven project utilizing the cyclonedx-maven-plugin. The maven project used here is onedev.

## Install repo

Clone the repository onedev using the command:

```bash
git clone https://github.com/theonedev/onedev.git
```

## Alter pom.xml

Open the pom.xml file of the project and add the following to the plugins section:

```xml
<plugin>
    <groupId>org.cyclonedx</groupId>
    <artifactId>cyclonedx-maven-plugin</artifactId>
    <version>2.7.9</version>
    <executions>
        <execution>
            <phase>package</phase>
            <goals>
                <goal>makeAggregateBom</goal>
            </goals>
        </execution>
    </executions>
    <configuration>
        <projectType>application</projectType>
        <schemaVersion>1.4</schemaVersion>
        <includeBomSerialNumber>true</includeBomSerialNumber>
        <includeCompileScope>true</includeCompileScope>
        <includeProvidedScope>true</includeProvidedScope>
        <includeRuntimeScope>true</includeRuntimeScope>
        <includeSystemScope>true</includeSystemScope>
        <includeTestScope>false</includeTestScope>
        <includeLicenseText>false</includeLicenseText>
        <outputFormat>all</outputFormat>
        <outputName>onedev-cyclonedx</outputName>
    </configuration>
</plugin>
```

## Build

Build and create the SBOM with the command


```bash
mvn clean install
```

## Conclusion

The resultant SBOM in both JSON and XML format should be found in the onedev/target directory.


## Troubleshooting

### Environment:

SBOMs were generated on two computers, both running Java version 11.0.20.1, with Maven versions 3.9.3 and 3.6.3, on Ubuntu 20.04 and 22.04 respectively.

### Build Failure:

* Delete directory “.m2” found in home directory with command 

```bash
rm -rf path/to/.m2
```
(Ubuntu).

* Ensure the correct parameters for the cyclonedx-maven-plugin are set, schema: 1.4, version: 2.7.9, name: onedev-cyclonedx. 

* Ensure that the maven version installed is the one found in your operating systems package manager, or is the most stable release for your system.

* Prior to running:

    ```bash
    mvn clean install
    ```

    run

    ```bash
    mvn clean -Dmaven.clean.failOnError=false
    ```

## Example SBOM

The following section illustrates a CycloneDX JSON SBOM of the project OneDev, created by the CycloneDX Maven Plugin.

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pretty JSON Display</title>
    <style>
        #json-container {
            height: 400px; /* Set a fixed height */
            overflow-y: auto; /* Enable vertical scrolling */
            border: 2px solid #ccc; /* Optional: add a border for visibility */
            padding: 10px;
        }
        pre {
            margin: 0;
            white-space: pre-wrap;
            word-wrap: break-word;
        }
    </style>
</head>
<body>
    <h3>
        <a href="./onedev-cyclonedx.json">onedev</a>
    </h3>
    <div id="json-container">
        <pre id="json-display"></pre>
    </div>
    <script>
        fetch('./onedev-cyclonedx.json')
            .then(response => response.json())
            .then(data => {
                document.getElementById('json-display').textContent = JSON.stringify(data, null, 2);
            })
            .catch(error => console.error('Error fetching JSON:', error));
    </script>
</body>
</html>

## References

* CycloneDX (2023). CycloneDX-maven-plugin (Version 2.7.9). [https://github.com/CycloneDX/cyclonedx-maven-plugin](https://github.com/CycloneDX/cyclonedx-maven-plugin)

* Theonedev (2023). Onedev (Version 9.1.5). [https://github.com/theonedev/onedev/tree/main](https://github.com/theonedev/onedev/tree/main)

