# NCBI BLAST in GCP

BLAST in GCP is a project that aims at making NCBI BLAST available as a
ready-to-use package that can be deployed in a private GCP cloud environment.

This document describes in brief the prerequisites and the sequence of steps
required to install BLAST in your GCP project.


## Before you begin

1. Select or create a GCP project.

   _Note_: If you don't plan to keep the resources you create in this procedure, create a new project instead of selecting an existing project. After you finish following these steps, you can delete the project, removing all resources associated with the project.

1. Make sure that billing is enabled for your Google Cloud Platform project.

   [LEARN HOW TO ENABLE BILLING](https://cloud.google.com/billing/docs/how-to/modify-project)

1. Enable the following APIs:
   - GKE
   - Cloud Memorystore for Redis
   - Cloud Dataproc
   - Cloud Endpoints
   - Stackdriver Logging

1. [Install and initialize the Cloud SDK.](https://cloud.google.com/sdk/docs/)

1. Install `kubectl`.

       gcloud components install kubectl

1. Select or create a GKE cluster. (It likely requires at least three nodes.)

1. Verify that you have access to the GKE cluster. The following command lists the nodes in your container cluster
   and indicates that your container cluster is running and that you have access to it.

       kubectl get nodes

   When you create a cluster with gcloud, authentication is set up automatically for kubectl. If you use the Google Cloud
   Platform Console to create clusters, you can set up authentication by using the `gcloud container clusters get-credentials <CLUSTER-NAME> --zone <CLUSTER-ZONE>` command.

1. The user account that will trigger the deployment must have the *Editor* role in the target project.

1. Select or create a Cloud Storage bucket for use by Cloud Dataproc to stage job dependencies,
   miscellaneous config files, and job driver console output


## Deployment

1. Define shell variables for convenience and to avoid mistakes:

       K8S_CLUSTER=[MY-K8S-CLUSTER]
       REGION=[MY-GCP-REGION]
       ZONE=[MY-K8S-CLUSTER-GCP-ZONE]
       BUCKET=[MY-BUCKET-FOR-CLOUDBLAST]
       SCALE=[MY-SCALE--SEE-BELOW-FOR-VALUES]

   The value of the `ZONE` variable above is the [zone][gcpzone] where
   your Kubernetes cluster is running.

   The value in `SCALE` refers to a file in the `spawner/scales`
   directory. (`SCALE` can be `small` or `nano` at the moment.) Once
   you have downloaded the spawner, you may wish to review the
   parameters stored in the file `spawner/scales/$SCALE.env` because
   they will have an effect on the cost and capabilities of the
   resulting CloudBLAST instance.

1. Download the "spawner" script:

       gsutil cat gs://strides-cloudblast-artifacts/deploy/spawn-heads-production.tgz | tar xz

1. Run the spawner script:

       $ cd spawner
       $ ./spawn.sh -r $REGION -z $ZONE -b $BUCKET -s $SCALE

   The above four are required parameters. The spawner script also accepts the
   following optional parameters:

   - _-n NAMESPACE_ - namespace for this deployment. This will be the
     namespace for the objects created in Kubernetes. This will also be the
     base component of the unique names that will be generated for other GCP
     resources. By default, the current account username is used as the
     basis for the namespace.

[gcpzone]: https://cloud.google.com/compute/docs/regions-zones/