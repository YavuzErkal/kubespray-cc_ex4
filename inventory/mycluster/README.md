## Selam Türkos

Linkteki GitHub Reposunu clonelayın. git clone commandini aşağıya yazdım bire bir kullanabilirsiniz.
https://github.com/kubernetes-sigs/kubespray
```
git clone git@github.com:kubernetes-sigs/kubespray.git
```

v2.23.1 versiyonua geçin.
```
git fetch --tags
git checkout v2.23.1
```

inventory dosyasını tanımlamak için `inventory/sample` dosyasını `inventory/mycluster` olarak kopyalayın. 
```
cp -rfp inventory/sample inventory/mycluster
```

Kopyalanan `inventory/mycluster` içinde `inventory.ini` dosyası oluşturun. Zaten varsa sadece içeriğini değiştirebilirsiniz. Dosyanın içeriği aşağıdaki gibi olmalı.
Bazı değerler değişken, onları şu şekilde belirttim `${variable}`:
```
[all:vars]
#### ansible_user=yavuzerkal benimki böyle sizde farklı olabilir.
ansible_user=${VM_USERNAME}

ansible_become=True

#### ansible_private_key_file=~/.ssh/id_rsa_cc benimki böyle sizde farklı olabilir
ansible_private_key_file=${ssh_key_path}

[all]
node1 ansible_host=${VM1_IP_ADDRESS}
node2 ansible_host={VM2_IP_ADDRESS}
node3 ansible_host={VM3_IP_ADDRESS}

[kube_control_plane]
node1
node2
node3

[etcd]
node1
node2
node3

[kube_node]
node1
node2
node3

[calico_rr]

[k8s_cluster:children]
kube_control_plane
kube_node
calico_rr
```

Sonra aşağıdaki command ile k8s cluster oluşturuluyor.
```
ansible-playbook -i inventory/mycluster/inventory.ini  --become --become-user=root cluster.yml
```
