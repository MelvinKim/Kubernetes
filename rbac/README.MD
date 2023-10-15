#### Role vs ClusterRole
- Role - defines allowed operations on namespaced resources eg pods, deployments.
- ClusterRole - defines allowed operations across more than one namespaces.
- RoleBinding can be used with a Role or ClusterRole
- ClusterRoleBinding can only be used with a ClusterRole
#### RoleBinding vs ClusterRoleBinding
- RoleBinding - defines the subject that can do what is defined in a Role.
- ClusterRoleBinding - defines the subject that can do what is defined in a ClusterRole.

#### NB:
- Users and Groups are not namespaced objects: considered cluster-wide resources