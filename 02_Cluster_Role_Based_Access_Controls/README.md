### Objective: Manage user roles and permissions in OpenShift
#### Task details
- Configure the following permissions for these users
    - mariam: full cluster administration rights
    - jane: able to create new projects
    - john: not able to create new projects
    - ian: view access to all projects in the cluster
    - alice: not able to modify the cluster
- Ensure that mariam is the only user with permission to modify the cluster configuration


0. Login as an admin to the Openshift Cluster

1. Grant `mariam` the `cluster-admin` role

        oc adm policy add-cluster-role-to-user cluster-admin mariam
    Note: The `cluster-admin` role is the highest level of access on an OpenShift Cluster

2. Grant `jane` the `self-provisioner` role

        oc adm policy add-cluster-role-to-user self-provisioner jane
    Notes: The `self-provisioner` role allows users to create new projects in the cluster.

3. Remove `self-provisioner` role from user `john`

        oc adm policy remove-cluster-role-from-user self-provisioner john
    
    1. This will return an error because user `john` was not explicitly granted the `self-provisioner` role.
    2. All OAuth authenticated users have by default the `self-provisioner` role.
    3. To prevent user `john` from creating new projects, we must disable the role from the `system:authenticated:oauth` group
    
            oc adm policy remove-cluster-role-from-group self-provisioner system:authenticated:oauth
        - This change will not persist after the master node reboots
        - Change the `rbac.authorization.kubernetes.io/autoupdate` to false. For the change to persist
        - Use the following command
        
                oc edit ClusterRole/self-provisioner
    4. Verify that only `jane` can create projects

            oc get clusterrolebinding self-provisioner -o yaml

4. Grant `ian` the `cluster-reader` role

        oc adm policy add-cluster-role-to-user cluster-reader ian

5. `alice` is a newly created user and has no `cluster-admin` permissions

        oc adm policy remove-cluster-role-from-user cluster-admin alice
    - This will return an error

6. `admin` is the only other user besides `mariam` that has `cluster-admin` permissions. Delete the `admin` user

    1. Get all users with with `cluster-admin` permissions

            oc get clusterrolebinding | grep ^cluster-admin
            oc describe clusterrolebinding cluster-admin-0
    2. Login as the `mariam` user and delete the other cluster admins

            oc login -u mariam -p ephesos
            oc delete user admin
            oc delete clusterrolebinding admin