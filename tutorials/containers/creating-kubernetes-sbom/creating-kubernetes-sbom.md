# Creating SBOMs from Kubernetes using BOM CLI

## Introduction

This tutorial illustrates how to create an SBOM from a kubernetes project using the ```bom``` CLI.

## Requirements

* Kubernetes.
* Bom CLI.
* Go language or cURL.

## Installation

### By Go

Install the CLI by running the command:

```bash
go install sigs.k8s.io/bom/cmd/bom@latest
```

### By cURL (Linux)

Install the CLI by running the command:

```bash
curl -L  https://github.com/kubernetes-sigs/bom/releases/download/v0.2.2/bom-linux-amd64  -o bom
sudo mv ./bom /usr/local/bin/bom
sudo chmod +x /usr/local/bin/bom`bash
```

Verify install by running:

```bash
bom -h
```

You should see the resultant output:

```bash
bom (Bill of Materials)

bom is a little utility that lets software authors generate
SPDX manifests to describe the contents of a release. The
SPDX manifests provide a way to list and verify all items
contained in packages, images, and individual files while
packing the data along with licensing information.

bom is still in its early stages and it is an effort to open
the libraries developed for the Kubernetes SBOM for other
projects to use.

Usage:
  bom [command]

Available Commands:
  completion  Generate the autocompletion script for the specified shell
  document    bom document → Work with SPDX documents
  generate    bom generate → Create SPDX SBOMs
  help        Help about any command
  validate    bom validate → Check artifacts against an sbom
  version     Prints the version

Flags:
  -h, --help               help for bom
      --log-level string   the logging verbosity, either 'panic', 'fatal', 'error', 'warning', 'info', 'debug', 'trace' (default "info")

Use "bom [command] --help" for more information about a command.

```

## Usage

### From Local Project

```bash
bom generate <path-to-local-directory>
```

### From Remote Image

To create an SBOM from a remote image, run:

```bash
bom generate --image <image-registry-address>
```

To output to a file, run:

```bash
bom generate <path-to-local-directory>  --output <output-name> --format <sbom-format>
```

Where the format is one of:

* tag-value
* json

An sbom with the designated name and format will be created.

## Example SBOM

This section illustrates an SPDX JSON SBOM of a Debian image, produced via Kubernetes bom.

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
        #xml-container {
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
        <a href="./spdx.json">kubernetes sbom</a>
    </h3>
    <div id="json-container">
        <pre id="json-display"></pre>
    </div>
    <script>
        function display_json(url, elementid){
        fetch(url)
            .then(response => response.json())
            .then(data => {
                document.getElementById(elementid).textContent = JSON.stringify(data, null, 2);
            })
            .catch(error => console.error('Error fetching JSON:', error));
        }
        function display_xml(url, elementid){
        fetch(url)
            .then(response => response.text())
            .then(data => {
                document.getElementById(elementid).textContent = data;
            })
            .catch(error => console.error('Error fetching JSON:', error));
        }
    display_json('./spdx.json', 'json-display');
    </script>
</body>
</html>


## References

* Kubernetes-Sigs. (n.d.). GitHub - kubernetes-sigs/bom: A utility to generate SPDX-compliant Bill of Materials manifests. GitHub. [https://github.com/kubernetes-sigs/bom](https://github.com/kubernetes-sigs/bom)

* Production-Grade Container orchestration. (n.d.). Kubernetes. [https://kubernetes.io/](https://kubernetes.io/)