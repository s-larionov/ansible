## Requirements

### Step 1

Configure inventory/k8s.yml for k8s hosts. Set hosts for masters, nodes and etcd.

Also, you can change settings for kubespray values in inventory/group_vars.

After that you can run kubespray playbook for setup k8s cluster:

```shell script
git submodule init
git submodule update
ansible-playbook kubespray/cluster.yml --become --become-user=root
```

### Step 2

For next step we have to set a secret with token to API of DigitalOcean.

`kubectl apply -f digitalocean-token.yml`
```yaml
apiVersion: v1
data:
  token: ...
kind: Secret
metadata:
  name: digitalocean-token
  namespace: cert-manager
type: Opaque
```

### Step 3

Run post-install roles for k8s.

```shell script
ansible-playbook playbooks/k8s.yml 
```

*WARNING!* Be careful with running this playbook. It will reset configuration for prometheus, grafana, etc.
