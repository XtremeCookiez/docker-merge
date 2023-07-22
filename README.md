# docker-merge

Overall goal of this program is to allow composition of docker images.

## Problem

When building a lot of docker images, there is often overlap between different images that don't really get captured well.

### Example

You have a c++ project you wish to build using Conan. Let's say this project also needs sqlite. Across your previous work you have a few docker images already setup:

1. A python image that has all your standard python configurations (custom pip configurations and so forth)

    ```Dockerfile
    FROM ubuntu:22.04
    RUN apt-get update && apt-get install python, python3-pip
    RUN pip install poetry
    RUN echo "Copy config file or something"
    ```

2. An image with your desired version of GCC (maybe for cross compilation)

    ```Dockerfile
    FROM ubuntu:22.04
    RUN apt-get update && apt-get install -y software-properties-common gcc gdb
    ```

3. An SQLite image:

    ```Dockerfile
    FROM ubuntu:22.04
    RUN apt-get update && apt-get install sqlite3
    ```

Traditionally, you have 2 options for combining all of these things.

1. Use one of your existing images as a base (e.g. `FROM my_python:3.8`) then install gcc, sqlite, and conan on top of it.
2. Start from scratch (like from an ubuntu or centos image)
   
Neither of these solutions are ideal. In both cases, you are effectively reimplementing at least 2 fo these dockerfiles.

## Solutions

I have 2 concepts for solving this.

1. Do it at the Dockerfile level. The tool would replace the "IMPORT" sections with snippets of dockerfiles from a registry. Then, use `docker build` to build the resulting dockerfile.

    ```Dockerfile
    FROM ubuntu:22.04
    {{ IMPORT my_python }}
    {{ IMPORT my_compiler }}
    RUN echo "Do project specific stuff"
    ```

2. Do it at the docker image level. Instead of dealing with dockerfiles, we deal with the images directly. Basically we take 2 images file systems and overlay them on top of each other.