# Nooba_backup

This repository will be used to store an automation to backup Nooba Database.

## Prerequisites

- We assume that you already have OCS or ODF installed and running within the Openshift Cluster 4.6+.  

- Clone the Nooba_Backup repository:  
`git clone git@github.com:filipe-santos-rh/Nooba_backup.git`  

### Installation

*The following automation will create a configMap within the `openshift-storage` namespace and a cronJob that will run on an hourly schedule.*  


##### Step 1

*You will need to create a service account within the openshift-storage namespace*  
`oc create sa snapshot-sa -n openshift-storage`  

*You will need to grant access to the newly created service account to allow it to create and run Cronjobs, VolumeSnapshot and retrieve cluster details. For ease of the process will grant admin access to the namespace, **Not recommended for the Production environment**.*  

`oc policy add-role-to-user admin -z snapshot-sa -n openshift-storage`  


*Confirm that the newly created service account could create VolumeSnapshots*  

`oc policy who-can create VolumeSnapshot`  


##### Step 2

*Create the Cronjob within the openshift-storage namespace*  
`oc apply -f ./Nooba_backup/CronJob_Noobaa_Snapshot.yaml`  

*Confirm that the cronjob was properly created*  


### Review

- *Confirm that the Cronjob was created succesfully*  
`oc get cronjobs -n openshift-storage`

- *If the cronjob executed, review the logs of the cronjob to confirm if there are any errors*  

- *If the cronjob executed successfully, review the volumeSnapshot and confirm it's in ready state*  