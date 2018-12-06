# pipeline-demo
DevOps exercise using polled github project to control build.

This repository contains project source code and Jenkins build control file.
Once installed as a Jenkins project, build will automatically launch if either is changed (polled every minute).

Polling was used instead of webhook as this project was developed on a secured network and external IP for the development machine could not be provided.
