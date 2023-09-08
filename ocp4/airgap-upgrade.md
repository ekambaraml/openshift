# Upgrading OpenShift 4x in AirGap/Restricted network environment

* [ ] Mirroring images
* [ ] Updating Image Content Source Policy
* [ ] Upgrading OpenShift
* [ ] Upgrading ODF Storage
* [ ] TroubleShooting


## Setup Quay registry server
https://access.redhat.com/documentation/en-us/red_hat_quay/3.9/html/deploy_red_hat_quay_for_proof-of-concept_non-production_purposes/poc-getting-started

```
mkdir /data/quay
export QUAY=/data/quay
mkdir -p $QUAY/postgres-quay
setfacl -m u:26:-wx $QUAY/postgres-quay

sudo podman run -d --rm --name postgresql-quay \
-e POSTGRESQL_USER=quayuser \
-e POSTGRESQL_PASSWORD=quaypass \
-e POSTGRESQL_DATABASE=quay \
-e POSTGRESQL_ADMIN_PASSWORD=adminpass \
-p 5432:5432 \
-v $QUAY/postgres-quay:/var/lib/pgsql/data:Z \
registry.redhat.io/rhel8/postgresql-13:1-109


sudo podman exec -it postgresql-quay /bin/bash -c 'echo "CREATE EXTENSION IF NOT EXISTS pg_trgm" | psql -d quay -U postgres'
```

## Image Mirroring

* OpenShift

oc mirror
  
* ODF

* 
* Monitoring
* Maximo
  

## TroubleShooting

'''
skopeo inspect docker://
