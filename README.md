### Micronaut app that exercises GCP object storage functionality

I'm specifying the service account to use in application.yml because I have multiple GCP accounts and the one I want to use for this app is different to the account I have configured with the environment variabLe GOOGLE_APPLICATION_CREDENTIALS.

Create the bucket:

`gcloud storage buckets create gs://mw-object-storage`

Upload profile.jpg included with project:

`curl -i -F 'fileUpload=@profile.jpg' http://localhost:8080/pictures/mike`

Check image was uploaded:

`gcloud storage ls --recursive gs://mw-object-storage`

Download the image:

`curl http://localhost:8080/pictures/mike -O -J`

Delete the image:

`curl -X DELETE http://localhost:8080/pictures/mike`

Check image has gone from GCP:

`gcloud storage ls --recursive gs://micronaut-guide-object-storage`