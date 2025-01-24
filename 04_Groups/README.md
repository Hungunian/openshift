### Objective: Set up groups and assign permissions
#### Task details
- Create the following groups
    - heroes
        - member: alice
    - wizards
        - member: ian
- heroes should have admin rights for projects:
        - neptune
        - siren
        - triton
        - nagas
- wizards should have view access for projects:
        - atlantis
- wizards should have edit permissions for projects:
        - siren

0. Login as the cluster administrator

1. Create the `heroes` group and add `alice`

        oc adm groups new heroes
        oc adm groups add-users heroes alice

2. Create the `wizards` group and add `ian`

        oc adm groups new wizards
        oc adm add-users wizards ian

2. Grant admin rights to the `heroes` group for the required projects

        for project in neptune siren triton nagas; do
          oc adm policy add-role-to-group admin heroes -n $project
        done

3. Grant view permission to the `wizards` group for the required projects

        oc adm policy add-role-to-group view wizards -n atlantis

4. Grant edit permission to the `wizards` group for the required projects

        oc adm policy add-role-to-group edit wizards -n siren