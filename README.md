# Multi-environment Config Management for Cloud Code with Skaffold and Kustomize

## Intro

## Tutorial

CREATE 2 PROJECTS

ENABLE SERVICES

cloudbuild.googleapis.com
run.googleapis.com
containerregistry.googleapis.com

$ export DEV_PROJECT=vic-cloud-run-kustomize-dev
$ skaffold render --default-repo=gcr.io/${DEV_PROJECT} \
                  --add-skaffold-labels=false --loud \
                  -o service-dev.yaml 
$ gcloud beta run services replace service-dev.yaml --region us-west2 --project ${DEV_PROJECT}

$ export PROD_PROJECT=vic-cloud-run-kustomize-prod
$ skaffold render --default-repo=gcr.io/${PROD_PROJECT} \
                  --add-skaffold-labels=false \
                  --profile dev -o service-prod.yaml 
$ gcloud beta run services replace service-prod.yaml --region us-west2 --project ${PROD_PROJECT}
