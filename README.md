# CI Repo for ISO

## Description
This CI environment is set up using [bosh-bootloader](https://github.com/cloudfoundry/bosh-bootloader), and deployed on **GCP**. The domain **corgi-poncho.cf** is registered at [freenom](https://my.freenom.com).

## Setting Up Concourse

export the environment variables.
```
export SERVICE_ACCOUNT_NAME=totoro
export PROJECT_ID=corgi-poncho-iso
export ZONE=us-west1-c
export REGION=us-west1
```
1. create a service account key
```
gcloud iam service-accounts create ${SERVICE_ACCOUNT_NAME}
gcloud iam service-accounts keys create --iam-account="${SERVICE_ACCOUNT_NAME}@${PROJECT_ID}.iam.gserviceaccount.com" ${SERVICE_ACCOUNT_NAME}.key.json
gcloud projects add-iam-policy-binding ${PROJECT_ID} --member="serviceAccount:${SERVICE_ACCOUNT_NAME}@${PROJECT_ID}.iam.gserviceaccount.com" --role='roles/editor'
```

2. bbl up a director

bbl uses [terraform tmeplate](https://github.com/cloudfoundry/bosh-bootloader/blob/master/terraform/gcp/templates/bosh_director.tf) to stand up a diretor.

```
bbl up --iaas gcp --gcp-service-account-key ${SERVICE_ACCOUNT_NAME}.key.json --gcp-project-id ${PROJECT_ID} --gcp-zone $ZONE --gcp-region $REGION
```
Note that you will get a 403 when first using the api. Just click on the link provided to enable the api access on gcloud console.

After running the `bbl up` command, a bbl state file is generated. For security purposes the file is saved in the gcp storage.

3. deploying concourse

Follow the instruction [here](https://github.com/cloudfoundry/bosh-bootloader/blob/master/docs/concourse-gcp.md).

Note: that the credentials in the `concourse.yml` file is tempalted out. A `credential.yml` file is saved to a lastpass note. In order to bosh deploy, save the file to the root directory as `credentials.yml` since it has been git ignored, and run the following command.
```
bosh -d concourse deploy concourse.yml -l credentials.yml
```

4. setup DNS
`corgiponcho.cf` is registered for free till 10/12/2018
go to gcp Cloud DNS and set up the subdomain ci.corgiponcho.cf to point to the load balancer

## run bosh commands
target bosh usiing bbl
```
eval "$(bbl print-env)"
```
Now, you are good to go with running bosh commands

## fly CLI
See the [concourse documentation](https://concourse.ci/fly-login.html) for details

set pipelines
```
fly -t corgiponcho sp -p {PIPELINE_NAME} -c pipelines/{PIPELINE_CONFIG_YML} -l credentials.yml
```