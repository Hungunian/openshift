### Objective: Setup project permissions
#### Task details
- Create the following projects
    - avalon
    - atlantis
    - neptune
    - siren
    - triton
    - garuda
    - nagas
- Configure jane as administrator for the avalon, atlantis, and garuda projects
- john should have view access for Avalon and Siren projects
- alice should be able to modify objects in the avalon project (expect roles and rolebindings)

0. Login as the cluster administrator

1. Create the projects

        for project in avalon atlantis neptune siren triton garuda nagas; do
          oc new-project $project
        done

2. Grant `jane` the admin role for the required projects

        for project in avalon atlantis garuda; do
          oc adm policy add-role-to-user admin jane -n $project
        done

3. Grant `john` view permission to the required projects

        for project in avalon siren; do
          oc adm policy add-role-to-user view john -n $project
        done

4. Grant `alice` edit permission for the avalon project

        oc adm policy add-role-to-user edit alice -n avalon
    Note: The edit rule typically allows users to modify most resources in a project, including creating and editing applications, but it doesn't allow management of roles or role bindings.