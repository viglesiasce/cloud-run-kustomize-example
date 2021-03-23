# Multi-environment Config Management for Cloud Code with Skaffold and Kustomize

## Intro

## Tutorial

1. Create 2 projects, one for development and another for production.

    ```shell
    export DEV_PROJECT=${USER}-cloud-run-kustomize-dev
    export PROD_PROJECT=${USER}-cloud-run-kustomize-prod
    gcloud projects create ${DEV_PROJECT}
    gcloud services enable cloudbuild.googleapis.com \
                           run.googleapis.com \
                           containerregistry.googleapis.com \
                           --project ${DEV_PROJECT}
    gcloud projects create ${PROD_PROJECT}
    gcloud services enable cloudbuild.googleapis.com \
                           run.googleapis.com \
                           containerregistry.googleapis.com \
                           --project ${DEV_PROJECT}
    ```

1. Build the development version of the app and run it. Do this as many times as you need to feel comfortable that your changes are correct.

    ```shell
    skaffold render --default-repo=gcr.io/${DEV_PROJECT} \
                    --add-skaffold-labels=false --loud \
                    -o service-dev.yaml
    gcloud beta run services replace service-dev.yaml --region us-west2 --project ${DEV_PROJECT}
    ```

1. Deploy to the production project when you've got things just how you like them.

    ```shell
    skaffold render --default-repo=gcr.io/${PROD_PROJECT} \
                   --add-skaffold-labels=false --loud \
                   --profile prod -o service-prod.yaml
    gcloud beta run services replace service-prod.yaml --region us-west2 --project ${PROD_PROJECT}
    ```
