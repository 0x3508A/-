# Google Cloud Platform = `gcp`

## Storage Policy

<https://cloud.google.com/storage/docs/bucket-policy-only?hl=en_US>

## Cloud Store Go Client Library in Golang

<https://cloud.google.com/storage/docs/reference/libraries#client-libraries-install-go>

## Service Account Roles

<https://cloud.google.com/iam/docs/understanding-roles>

## Default Credentials configuration for WebApps

<https://cloud.google.com/docs/authentication/production>

## Role for Storage Admin

`roles/storage.admin`

## Access the Service Accounts

<https://console.cloud.google.com/iam-admin/serviceaccounts?>

## Key File Creation Commands with `gcloud`

```sh
gcloud iam service-accounts create [NAME]

gcloud projects add-iam-policy-binding [PROJECT_ID] --member "serviceAccount:[NAME]@[PROJECT_ID].iam.gserviceaccount.com" --role "roles/storage.admin"

gcloud iam service-accounts keys create [FILE_NAME].json --iam-account [NAME]@[PROJECT_ID].iam.gserviceaccount.com
```

## Actual Commands to operate `gcloud`

```sh
gcloud iam service-accounts create store-example-appengine-github

gcloud projects add-iam-policy-binding golangblr \
--member "serviceAccount:store-example-appengine-github@golangblr.iam.gserviceaccount.com" \
--role "roles/storage.admin"

gcloud iam service-accounts keys create store-example-appengine-github.json \
--iam-account store-example-appengine-github@golangblr.iam.gserviceaccount.com
```

## Extensions Needed

Need to install following extensions:

```sh
core
app-engine-go
gcd-emulator
cloud-firestore-emulator
pubsub-emulator
bq
cloud-datastore-emulator
gsutil
```

Commands:

```sh
gcloud components update --quiet
gcloud components install core\
 app-engine-go\
 gcd-emulator\
 cloud-firestore-emulator\
 pubsub-emulator\
 bq\
 cloud-datastore-emulator\
 gsutil\
 --quiet


docker run \
 -p 8080:8080 \
 -p 8000:8000 \
 -v '$(pwd)'app.yaml:/apps google/cloud-sdk dev_appserver.py \
 --host=0.0.0.0
 --admin_host=0.0.0.0
 /apps/app.yaml

```

----
<!-- Footer Begins Here -->
## Links

- [Back to Misc. Hub](./README.md)
- [Back to Root Document](../README.md)
