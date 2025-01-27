### Objective: Create a custom role and manage user permissions
#### Task details
- Create a custom `viewnagas` role in the `nagas` project
- The role should provide read-only access to pods, deployments, and imagestreams
- The role should permit retrueving individual resources, listing all resources, and watching for updates
- Assign `viewnagas` role to `john`

0. Login as the cluster administrator

1. Create the custom `viewnagas` with the specific permissions

        oc create role viewnagas --verb=get,list,watch --resource=pods,deployments,imagestreams -n nagas

2. Assign the custom role to `john`

        oc adm policy add-role-to-user viewnagas john --role-namespace=nagas -n nagas

3. Verify that the role has been correctly assigned to `john`
    1. Verify the rolebinding entry for `john`

            oc get rolebinding -n nagas
            oc describe rolebindings viewnagas -n nagas
    2. Create a deployment to test the permissions
    Note: Alice is a member of the heroes group, which has administrative rights for the Naga's project.
            
            oc login -u alice
            oc project nagas
            oc create deployment test-deployment --image=bitnami/nginx
            oc login -u john
            oc get pods -n nagas
            pc get imagestreams -n nagas
            oc create deployment another-test --image=bitnami/nginx #this should fail