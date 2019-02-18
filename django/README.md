# Django Container

This container is designed to power a Django based Python project using a combination of uWSGI and Nginx.

## Usage

* Place this Dockerfile definition (with supporting docker folder) in to the root workspace of your application
* Build the container using build argument `PROJECT_REFERENCE` to correlate with the project path that gets created with the Django project
* Once built, run the container, it will expose itself on port 80 by default
