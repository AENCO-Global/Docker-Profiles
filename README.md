# Aenco Docker Collection

This repository contains a collection of all the Docker images used throughout Aenco; take note that many of these images will need to located in path of the project they are related to in order to successfully build

## directory

* **Block Explorer** - A simple Alpine Linux image with Nginx present used to host static content. In current form, it is mainly used as an intermediary store for later building of the full server
* **Full Server** - Rolling all of the current functionality in to a single Docker container. This image uses monit as a process manager in order to run the aen server, block explorer, and the REST gateway, including MongoDB as a support service
* **REST Gateway** - Used to provide an intermediary interface between the blockchain network and fixed infrastructure. Currently using NodeJS but, action being taken to replace with a Go equivalent
* **Server** - The base build of the blockchain application with no extra facilities
* **Toolchain** - Used for providing an environment in which the server can be compiled
