Helm Releases
=========

Create, update, upgrade an delete Helm releases using Ansible and the Helm CLI  
This role aims to mimic the ansible helm module (not maintained for now)  
and to be as much as idempotent as possible.  
Feel free to report any issue !

Requirements
------------

Helm 2.x installed and configured on the target host.  
The role is not yet tested for Helm 3

Role Variables
--------------

The role mimic a part of the module parameters, but with prefix

```yaml
hr_chart:
  name: "" # chart's name
  version: "" # chart's expected version.
              # By default, helm installs the latest version for first install
              # keep to found version for values updates unless hr_state is upgraded
  source:
    type: "" # repo.
             # git NOT SUPPORTED YET
    name: "" # repo's name
    path: "" # path of the chart in a git repository (optionnal)
    update_url: "{{ hr_reuse_values }}" # update the repo's URL if the URL doesn't match
hr_name: "" # the release's name
hr_namespace: "" # namespace for the release
hr_state: present # present | absent | purged | upgraded
hr_values: {}  # chart's custom values
               # If hr_state = present but the values are differents from an existing release, it will be updated with this values.
hr_reuse_values: false  # merge new values with existing values from an existing release
hr_global_flags: [] # list of global flags supported by the helm CLI
```


Example Playbook
----------------

```yaml
    - hosts: helm_cli_host
      tasks:
         - name: Install cert-manager
           include_role: 
             name: aalaesar.helm_release
      vars:
        hr_name: cert-manager
        hr_namespace: cert-manager
        hr_chart:
          version:
          name: cert-manager
          source:
            type: repo
            name: jetstack
            location: "https://charts.jetstack.io"
            # update_url: true
        hr_state: present
        hr_values: 
          image:
            tag: "v0.12.0"
          extraArgs:
            - "--dns01-recursive-nameservers=8.8.8.8:53,1.1.1.1:53"

```

License
-------

BSD 3
