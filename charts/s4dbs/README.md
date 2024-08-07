# Speedgain for Databases Helm Chart
This Helm Chart can be used to deploy Speedgain for Databases (S4DBs) via Helm. 

Some adjustments have to be made to the values.yml and some objects have to be created before running this chart.

## Adjustments within the values.yml
### storageClassName
The storage class name needs to be defined if no PVC was created up front. This name will be used to create a PV and PVC during helm deployment. See last chapter in this readme to create on PVC for the repository database on your own.

## Prereqs
In order to run successfully, some things have to be upn front:

### 1. Create a Namespace / poject if not done yet

Use kubectl or oc or openshift web frontend to create or request a new project to deploy speedgain for database into.

### 2. Adding Persistent Volume Claims
The helm chart does create PVC / Persistenc Volume Claims if you choose a valid storage class name. You may want to create one up front the deployment to adjust some more parameters. The chart will recognize a pre-existing PVC and not create a new one / overwrite the existing one. If you are not allowed to create a PVC on your own, ask your administrator to do it for you in your project. Storage size is just a first step - can be adjusted based on your needs.

````
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: s4dbs-postgres-pv-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 100Gi
---

````

## Open Speedgain for Databases Web Frontend
The Helm Chart should have created a route and/or load balancer four you. Look for the load balancer or route adress in your openshift or kubernetes frontend / console to open Speedgain for Databases.
In an Openshift environment this is typically:

`http://s4dbs-<Projectname>.apps.<OpenShiftDNSName>`
