# Speedgain for Databases Helm Chart
This Helm Chart can be used to deploy Speedgain for Databases (S4DBs) via Helm. 

Some adjustments have to be made to the values.yml and some objects have to be created before running this chart.

## Adjustments within the values.yml
### serverHostname / aka public dns adress
You need to add information for the paramter "serverHostname" before deploying the chart. This is the public DNS Name that has to be create after deploying the chart - e.g. the dns name of the route in openshift. The name is typically:
````
<Name of The Route>-<Project/Namespace Name>.apps.ocp.<mydomain>.<myTopLevelDomain>
````
Example:
````
s4dbs-public-s4dbs-rel150.apps.ocp.itgain.de
````

### pdbPass
The dpbPass variable defines the postgresql superuser password. This password will be stored in a secret and will be used by the services connecting to the repository database.

### serverPort
Maybe you will have to adjust the serverPort value (NodePort in Openshift) if it is already taken. 

## Prereqs
In order to run successfully, some things have to be upn front:

### 1. Create a Namespace / poject if not done yet

Use kubectl or oc or openshift web frontend to create or request a new project to deploy speedgain for database into.

### 2. Adding Persistent Volume Claims
The helm chart does not create PVC / Persistenc Volume Claims. If you are not allowed to create a PVC on your own, ask your administrator to do it for you in your project. Storage size is just a first step - can be adjusted based on your needs.

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
