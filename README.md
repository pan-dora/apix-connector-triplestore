Api-X Messaging
===================================

### Image on Docker Hub
`pandorasystems/messaging`
-----------------
[![](https://images.microbadger.com/badges/image/pandorasystems/messaging.svg)](https://microbadger.com/images/pandorasystems/messaging "pandorasystems/messaging")[![](https://images.microbadger.com/badges/version/pandorasystems/messaging.svg)](https://microbadger.com/images/pandorasystems/messaging "pandorasystems/messaging")


This contains messaging extensions for API-X.
These are OSGi services that extend the functionality of a [Fedora4](https://wiki.duraspace.org/display/FF/Fedora+Repository+Home) repository.

Extensions
----------
* [`exts-connector-triplestore`](exts-connector-triplestore): An triplestore indexing services

Building
--------

Create OSGI bundles
```sh 
    $ gradle install
```
Copy bundles from local Maven repository to Docker Build directory
```sh      
    $ gradle copyTask
```
Build Docker image
```sh 
    $ gradle docker
```
Execute Docker Compose
 ```sh     
    $ docker-compose up
  ```   

More information
----------------
See [pandora-demo](https://github.com/pan-dora/pandora-demo) for implementation example.


