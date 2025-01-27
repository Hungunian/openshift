### Objective: Create a ResourceQuota for the `neptune` project
#### Task details
- Create a ResourceQuota named `neptune-quota` in the `neptune` project with the following constraints:
    - Total memory consumption across all containers: 3Gi maximum
    - Total CPU usage across all containers: 2 cores maximum
    - Total memory requests across all containers: 1Gi maximum
    - Maximum number of replication controllers: 3
    - Maximum number of pods: 5
    - Maximum number of services: 3

0. Login as the cluster administrator

1. Change to the correct project

        oc project neptune

2. Create the ResourceQuota with the specified constraints
    1. You can use the `-h` flag to get example usage for any oc command
       
            oc create quota -h #shows usage and example
    2. Generate a yaml definition file for the quota
            
            oc create quota neptune-quota --hard=cpu=2,memory=3G,pods=5,services=3,replicationcontrollers=3 --dry-run=client -o yaml > neptune-quota.yaml
    3. Make the necessary changes on the generated yaml, lines 8-10
    4. create the resource

            oc create -f neptune-quota.yaml