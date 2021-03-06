# Ansible & Vagrant examples - DevOps

### 1- Criar Pasta/Path onde será a base.

### 2- Editar [Vagrantfile]:
```rb
Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/trusty64"

  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
  end

  config.vm.define "wordpress" do |m|
    m.vm.network "private_network", ip: "172.17.177.40"
  end

end
```

### 3- Editar [hosts]:
```rb
[wordpress]
172.17.177.40
```

### 4- Criar a VM:
`vagrant up`

### 5- Testar conexão:
`vagrant ssh`

### 6- Testar comandos em modo verbose diretamente no grupo especificado:
`ansible -vvvv wordpress -u vagrant --private-key .vagrant/machines/wordpress/virtualbox/private_key -i hosts -m shell -a 'echo Hello Ansible!'`

-------------------------------------------
## Executando um playbook no Ansible

### 1- Criar `provisioning.yml`
```yml
---
- hosts: all
  tasks:
    - name: 'Instala o PHP 5'
      apt:
        name: php5
        state: latest
      become: yes
    - name: 'Instala o Apache2'
      apt:
        name: apache2
        state: latest
      become: yes 
``` 

### 2- Rodar o playbook na VM
`ansible-playbook provisioning.yml -u vagrant -i hosts --private-key .vagrant/machines/wordpress/virtualbox/private_key`

