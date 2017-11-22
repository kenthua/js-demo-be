# base files / pipeline
https://github.com/christophersanson/js-demo-be

# spin up a default GKE cluster

# create keyring and key
gcloud kms keyrings create js-demo-be --location=global
gcloud kms keys create github-token --keyring=js-demo-be --purpose encryption --location=global

# need this so cloudbuilder can do kms encrypt/decrypt
gcloud kms keys add-iam-policy-binding github-token --location=global --keyring=js-demo-be --member=serviceAccount:665770853622@cloudbuild.gserviceaccount.com --role=roles/cloudkms.cryptoKeyEncrypterDecrypter

# need this access for cloudbuilder to execute the k8s apply
gcloud kms keys add-iam-policy-binding github-token --location=global --keyring=js-demo-be --member=serviceAccount:665770853622@cloudbuild.gserviceaccount.com --role=roles/container.developer

# acquire github token from github account developer settings - personal access tokens, need role of "repo"
export TOKEN=

gcloud kms encrypt \
> --plaintext-file=- \
> --ciphertext-file=- \
> --location=global \
> --keyring=js-demo-be

# place the resulting key into your cloudbuild.yml GITHUB_TOKEN secret

# modify cloudbuild.yml and k8s.yml accordingly to your gcloud / gke project

# setup local git repo or fork
git init
git add *
git add .gitignore
git add .dockerignore
git remote add ..
git push origin master

# mirror your github repo with gcsr

# setup your GCB trigger

# Cloud builder custom variable _IMAGE_NAME=js-demo-be

# need to prebuild image prior to executing the cloudbuild trigger.  So setup another trigger to build via dockerfile, otherwise build locally and gcr docker -- push


https://github.com/kelseyhightower/pipeline/blob/master/labs/prerequisites.md
