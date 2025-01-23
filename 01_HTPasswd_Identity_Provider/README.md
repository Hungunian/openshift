### Objective: Configure HTPasswd Identity Provider in OpenShift
#### Task details
- Configure an HTPasswd Identity Provider named 'local-users'
- Create a secret named 'ex280-htpass-secret' to store the user credentials
- Add the following user accounts to the HTPasswd file:
    - john with password doe123
    - jane with password doe123
    - ian with password manchester
    - mariam with password ephesos
    - alice with password wonderland

0. Install HTPasswd utility, *if necessary*

        sudo dnf install httpd-tools

1. Create users
        
        htpasswd -c -B -b /tmp/htpasswd john doe123
        htpasswd -B -b /tmp/htpasswd jane doe123
        htpasswd -B -b /tmp/htpasswd ian manchester
        htpasswd -B -b /tmp/htpasswd mariam ephesos
        htpasswd -b /tmp/htpasswd alice wonderland
    Notes
    - `-c` flag to create the htpasswd file
    - `-B` flag to use the bcrypt hashing algorithm
    - Default hasing algorithm is MD5, Eg. user alice

2. Create a secret from the `/tmp/htpasswd` file

        oc create secret generic ex280-htpass-secret --from-file htpasswd=/tmp/htpasswd -n openshift-config

3. Export the existing OAuth resource to a file named oauth.yaml

        oc get oauth cluster -o yaml > /tmp/oauth.yaml

4. Edit the oauth configuration file
    1. `oauth-new.yaml` is the edited file
    2. line 11 is the name of the secret created on step 2
    3. line 13 is the name of the HTPasswd Identity Provider that will appear on Openshift

5. Apply the custom resource that was defined in the previous step.

        oc replace -f /tmp/oauth.yaml

6. Wait for the authentication changes to take effect, it will take some time for the pod to restart
        
        watch oc get pods -n openshift-authentication

7. Test the new configuration

        oc login -u john -p doe123