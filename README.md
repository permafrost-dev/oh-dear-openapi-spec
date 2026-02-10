<p align="center">
  <img src="./assets/oh-dear-logo-rgb.png" alt="Oh Dear! Logo" width="200"/>
  <br/>
    <img src="./assets/openapi-pantone.png" alt="OpenAPI Logo" width="200"/>
    <br/>
    <img src="https://img.shields.io/github/license/permafrost-dev/oh-dear-openapi-spec?logo=opensourceinitiative&logoColor=white" alt="License: MIT" />
    <img src="https://img.shields.io/github/v/release/permafrost-dev/oh-dear-openapi-spec?logo=github&logoColor=white&nocache=1" alt="Latest Version" />
    <img 
        src="https://img.shields.io/github/release-date/permafrost-dev/oh-dear-openapi-spec?logo=github&displayDate=published_at&logoColor=white&label=last+released&color=%236BA539" 
        alt="GitHub Last Release" 
    />
    <img src="https://qlty.sh/badges/b28124b6-e46f-429d-8abb-7426ba909401/maintainability.svg" alt="Maintainability" />
</p>

This project provides both [OpenAPI](./specs/oh-dear.openapi.yaml) and [Arazzo](./specs/oh-dear.arazzo.yaml) specifications for the excellent [Oh Dear! monitoring service](https://ohdear.app).

## Table of Contents

- [Table of Contents](#table-of-contents)
- [About Oh Dear](#about-oh-dear)
- [Looking for `permafrost-dev`?](#looking-for-permafrost-dev)
- [Specifications](#specifications)
    - [OpenAPI](#openapi)
    - [Arazzo](#arazzo)
- [Repository Structure](#repository-structure)
- [Usage](#usage)
    - [Generating Documentation](#generating-documentation)
    - [Generating SDKs](#generating-sdks)
- [Validation](#validation)
- [Development](#development)
- [Status](#status)
- [Contributing](#contributing)
- [Changelog](#changelog)
- [Security Vulnerabilities](#security-vulnerabilities)
- [Credits](#credits)
- [License](#license)

## About Oh Dear

[Oh Dear!](https://ohdear.app) is an all-in-one monitoring tool for your websites. 
It offers uptime monitoring, SSL certificate checking, broken link detection, scheduled task monitoring, and much more. 
This project aims to provide comprehensive, community-driven API specifications to make integrations with Oh Dear! a breeze.

## Looking for `permafrost-dev`? 

This is the new home for the original `permafrost-dev` repository.

## Specifications

### OpenAPI

The OpenAPI specification follows the [v3.1.0 standard](https://spec.openapis.org/oas/v3.1.0). It provides a detailed, machine-readable description of the Oh Dear! API. This makes it easy to:

- Generate client SDKs for various programming languages.
- Create server stubs for testing and development.
- Produce interactive API documentation.
- Import into tools like Postman for easy API exploration.

It is particularly useful for building robust [third-party integrations](https://ohdear.app/docs/integrations/3rd-party-integrations-of-oh-dear) and [SDKs](https://ohdear.app/docs/integrations/the-oh-dear-php-sdk).

### Arazzo

The Arazzo specification follows the [v1.0.1 standard](https://github.com/OAI/Arazzo-Specification/blob/main/versions/1.0.1.md) and 
defines how the API works in real-world scenarios. It describes workflows that require one or more steps with request/response 
series. A primary benefit is providing the ability to programmatically test the OpenAPI specification and ensure that API workflows 
behave as expected.

## Repository Structure

A quick overview of the most relevant files and directories:

```text
├── specs/
│   ├── oh-dear.openapi.yaml   # Primary OpenAPI 3.1.2 specification
│   └── oh-dear.arazzo.yaml    # OpenAPI Arazzo 1.0.1 workflow specification
├── config/
│   ├── .markdownlint.yaml     # markdownlint configuration for linting Markdown files
│   ├── .spectral.yaml         # Spectral configuration for linting the OpenAPI spec
│   └── vacuum-ruleset.yaml    # Custom ruleset used for additional quality linting
├── assets/                    # Logos and images used in the README/docs
├── package.json               # Lint scripts and dependencies (Spectral & Redocly CLI)
├── CHANGELOG.md               # Changelog for tracking notable changes
├── LICENSE.md                 # MIT License
└── README.md
```

- `specs/` contains the source-of-truth API contract files. Any change here should ideally be validated via the lint commands and—where applicable—reflected in downstream SDKs or docs.
- `config/.spectral.yaml` configures [Spectral](https://github.com/stoplightio/spectral) for linting the OpenAPI specification.
- `config/.markdownlint.yaml` configures [markdownlint](https://github.com/igorshubovych/markdownlint-cli) for linting Markdown files.
- `config/vacuum.yaml` is an extended quality ruleset (Vacuum) enforcing style, documentation completeness, schema rigor, and structural integrity on top of default linters.
- `assets/` stores project branding and visual resources.

<img src="./assets/chart-project-files.png" alt="Project Structure" width="100%" style="align: center; width:100%;"/>

## Usage

To use these specifications, you'll need Node.js and npm installed. First, clone the repository and install the dependencies:

```bash
git clone https://github.com/permafrost-dev/oh-dear-openapi-spec.git
cd oh-dear-openapi-spec
npm install
```

### Generating Documentation

You can generate beautiful, interactive API documentation from the OpenAPI specification using tools like [Redocly](https://redocly.com/):

```bash
npx @redocly/cli build-docs specs/oh-dear.openapi.yaml
```

This will generate a `redoc-static.html` file in the root of the project.

### Generating SDKs

A wide range of tools can generate client SDKs from OpenAPI specifications. One popular option is the [OpenAPI Generator CLI](https://openapi-generator.tech/).

For example, to generate a TypeScript SDK:

```bash
npx @openapitools/openapi-generator-cli generate \
  -i specs/oh-dear.openapi.yaml -g typescript-axios \
  -o ./generated-sdk/ts
```

## Validation

This project uses multiple layers of validation to ensure specification quality:

1. **Spectral** (`@stoplight/spectral-cli`) – General OpenAPI best practice and style conformance.
2. **Redocly CLI** (`@redocly/cli`) – Structural validation and bundling consistency.
3. [**Vacuum**](https://github.com/daveshanley/vacuum) – An ultra-super-fast, lightweight OpenAPI linter and quality checking tool. 
4. Enhanced quality checks: description presence & duplication, schema typing, path ambiguity, enum uniqueness, security-related 
5. markdown hygiene, and naming conventions.

Run all standard validations:

```bash
npm run lint
```

Run individually:

```bash
npm run lint:open-api   # Lints specs/oh-dear.openapi.yaml via Spectral
npm run lint:arazzo     # Lints specs/oh-dear.arazzo.yaml via Redocly CLI
```

Or lint the OpenAPI specification using individual npm scripts:

```bash
npm run lint:open-api:spectral  # Lints specs/oh-dear.openapi.yaml via Spectral
npm run lint:open-api:vacuum    # Lints specs/oh-dear.openapi.yaml via Vacuum
npm run lint:open-api           # Lints using Spectral CLI only.
```

## Development

The OpenAPI specification file was created using [Stoplight Studio](https://stoplight.io/studio/), GPT-5, and manual 
editing in VSCode. The official Oh Dear! documentation and PHP SDK source code were treated as the source of truth.

The Arazzo specification was initially generated by `@redocly/cli` from the OpenAPI specification, then manually edited 
in VSCode to ensure accuracy and completeness.

## Status

The specifications are a work in progress. The OpenAPI specification is approximately 95% complete, while the Arazzo specification
is around 70% complete. Keep an eye on the [CHANGELOG](CHANGELOG.md) and the issue tracker for updates.

## Contributing

Contributions are welcome! Please feel free to submit a pull request or create an issue for any bugs, suggestions, or questions you
may have. See the [issue tracker](https://github.com/permafrost-dev/oh-dear-openapi-spec/issues) for a list of open items.

When contributing changes to the specs:

- Run `npm run lint` before opening a PR.
- Keep descriptions meaningful—avoid duplication.
- Favor reusable components where appropriate (consider refactoring repeated schema fragments).
- If adding workflows, ensure Arazzo steps align with existing operationIds.

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information on what has changed recently.

## Security Vulnerabilities

Please review our [security policy](https://github.com/permafrost-dev/oh-dear-openapi-spec/security/policy) on how to report security vulnerabilities.

## Credits

- [Patrick Organ](https://github.com/patinthehat)
- [Mattias Geniar](https://github.com/mattiasgeniar)
- [All Contributors](https://github.com/permafrost-dev/oh-dear-openapi-spec/graphs/contributors)

## License

This project is licensed under the [MIT License](./LICENSE.md).
