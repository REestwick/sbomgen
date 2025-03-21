# Creating SBOMS For Rust Projects

## Introduction

This tutorial illustrates how to produce an SBOM using Rust projects using the Cargo-Sbom and CycloneDX-Rust-Cargo CLIs.

## Requirements

* [Rust language](https://www.rust-lang.org/tools/install)

* Cargo

## Installation

### Cargo SBOM

Run:

```bash
cargo install cargo-sbom
```

To verify installation, run:

```bash
cargo sbom --help
```

You should see the resultant output:

```
Create software bill of materials (SBOM) for Rust

Usage: cargo sbom [OPTIONS]

Options:
      --cargo-package <CARGO_PACKAGE>
          The specific package (in a Cargo workspace) to generate an SBOM for. If not specified this is all packages in the workspace.
      --output-format <OUTPUT_FORMAT>
          The SBOM output format. [default: spdx_json_2_3] [possible values: spdx_json_2_3, cyclone_dx_json_1_4]
      --project-directory <PROJECT_DIRECTORY>
          The directory to the Cargo project. [default: .]
  -h, --help
          Print help
  -V, --version
          Print version

```

### Cyclonedx-Rust-Cargo

Run:

```bash
cargo install cargo-cyclonedx
```

To verify installation, run:

```bash
cargo cyclonedx --help
```

You should see the resultant output:

```
Creates a CycloneDX Software Bill-of-Materials (SBOM) for Rust project

Usage: cargo cyclonedx [OPTIONS]

Options:
      --manifest-path <PATH>             Path to Cargo.toml
  -f, --format <FORMAT>                  Output BOM format: json, xml
  -v, --verbose...                       Use verbose output (-vv very verbose/build.rs output)
  -q, --quiet                            No output printed to stdout
  -a, --all                              List all dependencies instead of only top-level ones
      --top-level                        List only top-level dependencies (default)
      --output-cdx                       Prepend file extension with .cdx
      --output-pattern <PATTERN>         Prefix patterns to use for the filename: bom, package
      --output-prefix <FILENAME_PREFIX>  Custom prefix string to use for the filename
  -h, --help                             Print help

```
## Usage

### Cargo SBOM

Navigate to the Rust project that you wish to create the SBOM for.

Run:

```bash
cargo sbom --output-format <sbom-format>
```

This outputs an sbom to the terminal in one of the predetermined formats:

* ```spdx_json_2_3```
* ```cyclone_dx_json_1_4```

This output can be redirected to a file via:

```bash
cargo sbom --output-format <sbom-format> > <filename>
```

#### Notes

* The SPDX output may feature license outputs that are not part of the SPDX License List, e.g. "MIT/Apache-2.0" (incorrect) as opposed to "MIT OR Apache-2.0" (correct).

### Cyclonedx-rust-cargo

Navigate to the Rust project that you wish to create the SBOM for.

Run:

```bash
cargo cyclonedx -f json -a
```

You should see an output "bom.json" in any folder containing rust source files.

#### Notes

* In multi folder/multi module projects, a separate SBOM file is created in the root of each module. If this is not desirable, the cargo-sbom CLI may be more applicable.

* Error messages may be seen, however an sbom is still built. This appears to be a [known issue](https://github.com/CycloneDX/cyclonedx-rust-cargo/compare/main...ctron:cyclonedx-rust-cargo:feature/improve_logs_1).

## Example SBOM

This section illustrates CycloneDX JSON and XML SBOMs, from the Cargo-SBOM and CycloneDX-Rust-Cargo codebases, created from Cargo-SBOM and CycloneDX-Rust-Cargo.

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
        #json-container2 {
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
        <a href="./cargo-sbom.cdx.json">cargo-sbom (json)</a>
    </h3>
    <div id="json-container">
        <pre id="json-display"></pre>
    </div>
    <h3>
        <a href="./cargo-sbom.cdx.xml">cargo-sbom (xml)</a>
    </h3>
    <div id="xml-container">
        <pre id="xml-display"></pre>
    </div>
        <h3>
        <a href="./sbom-rs.cdx.json">sbom-rs (json)</a>
    </h3>
    <div id="json-container2">
        <pre id="json-display2"></pre>
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
    display_json('./cargo-sbom.cdx.json', 'json-display');
    display_xml('./cargo-sbom.cdx.xml', 'xml-display');
    display_json('./sbom-rs.cdx.json', 'json-display2');
    </script>
</body>
</html>


## References

* CycloneDX. (2023). cyclonedx-rust-cargo. [https://github.com/CycloneDX/cyclonedx-rust-cargo](https://github.com/CycloneDX/cyclonedx-rust-cargo)

* Psastras. (2023). cargo-sbom. [https://github.com/psastras/sbom-rs](https://github.com/psastras/sbom-rs)

