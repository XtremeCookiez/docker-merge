# Interface Specifications

## `composition.toml`

This file defines this package.

- `name`: The package name
- `description`: Brief description of the package
- `base_image`: Array of base images/OSes that are supported by this package.
- `version`: The version of the package
- `requirements`: Dictionary of packages needed by this package and their compatible versions.

## `composition.lock`

This is a generated file, which will contain the exact versions and hashes of all packages used.
