# Interface Specifications

## `composition.toml`

This file defines this package.

- `name`: The package name
- `description`: Brief description of the package
<!-- - `base_image`: Array of base images/OSes that are supported by this package. -->
- `version`: The version of the package
<!-- - `requirements`: Dictionary of packages needed by this package and their compatible versions. -->
- `images`: Array of images to be merged. Note that this list is ordered. The first images listed
    (e.g. `images[0]`) will overwrite files in later images. Keep this in mind as this is the means
    of handling merge conflicts.

## `composition.lock`

This is a generated file, which will contain the exact versions and hashes of all images used.

## Docker Image Labels

Resulting images will have labels applied in the following format:

`docker-composition.base-images`: A JSON formatted array of images (with hashes used to generate
this image)
