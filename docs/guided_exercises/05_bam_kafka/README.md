# {{ product. short }} + Kafka Workshop

A set of guided labs to get you up started on how to:
- 
### Running locally

Have docker or podman running locally:

    ~~~shell
    docker run -it --rm -p 8080:8080 -v $(pwd):/app-data -e CONTENT_URL_PREFIX="file:///app-data" -e WORKSHOPS_URLS="file:///app-data/_bam_kafka_workshop.yml" -e LOG_TO_STDOUT=true quay.io/osevg/workshopper
    ~~~

### Running on ocp

Login on ocp and:

    ~~~shell
    oc create -f support/ocp-provisioning.yml
    ~~~
