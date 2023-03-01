### Micronaut app that exercises GCP object storage functionality

You'll need a GCP account.

The commands below will create a project, service account, roles, bucket and test the app.

Assume you're running this from the root of this app so key file ends up in correct place to be referenced by the app.

You may hit a problem with billing not being associated with the account like:

_ERROR: (gcloud.storage.buckets.create) HTTPError 403: The billing account for the owning project is disabled in state absent_

I had to go to the web console Billing section and fix this.

Create the project:

`gcloud projects create winged-spotted-zebra`

Select the project:

`gcloud config set project winged-spotted-zebra`

Create the service account:

`gcloud iam service-accounts create sa-testing --description="Service account for testing purposes" --display-name="sa-testing"`

Add relevant storage role:

`gcloud projects add-iam-policy-binding winged-spotted-zebra --member="serviceAccount:sa-testing@winged-spotted-zebra.iam.gserviceaccount.com" --role="roles/owner"`

Create the service account private key:

`gcloud iam service-accounts keys create ./src/main/resources/sa-testing-pk.json --iam-account="sa-testing@winged-spotted-zebra.iam.gserviceaccount.com"`

Create the bucket:

`gcloud storage buckets create gs://zebra-object-storage`

Upload profile.jpg included with project:

`curl -i -F 'fileUpload=@profile.jpg' http://localhost:8080/pictures/mike`

Check image was uploaded:

`gcloud storage ls --recursive gs://zebra-object-storage`

Download the image:

`curl http://localhost:8080/pictures/mike -O -J`

Delete the image:

`curl -X DELETE http://localhost:8080/pictures/mike`

Check image has gone from GCP:

`gcloud storage ls --recursive gs://zebra-object-storage`