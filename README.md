# Creating a Virtual Machine with Ansible in Proxmox

Bu Ansible rolü, Proxmox üzerinde sanal makinelerin otomatik olarak oluşturulması ve yönetilmesi için kullanılır. Proxmox Virtual Environment (PVE) platformunu kullanarak VM'lerin kolayca dağıtılmasını ve konfigürasyonunun yapılmasını sağlar.

## Özellikler

- Ubuntu 22.04.4 LTS (Jammy Jellyfish) 
- Debian 11  (bullseye)

## Önkoşullar

- **SSH** Proxmox sunucunuza ssh keyinizi yüklemeniz
````bash
ssh-copy-id -i ~/.ssh/mykey root@192.168.1.10
````

## Kullanım

1. **Envanterinizi Yapılandırın**
   - `inventory` dizininde ki `cluster_inventory.yml` düzenlemeniz gerekmektedir. Cloud_init bölümü sunucunuzun alacağı id, parola ve network bilgileri buradan çekecektir.
   - Ayrıca `tasks` dizininden template ve vm'in alacağı `VM ID` düzenleyebilirsiniz. Ayrıca vm ile ilgili ram, cpu, disk gibi özellikleri `*.vm.yml` dosyasından düzenleyebilirsiniz.
   

````yml
hypervisor:
  hosts:
    proxmox:
      ansible_host: 192.168.1.10
      ansible_user: 'root'
      ansible_password: 'admin'
      ansible_connection: 'ssh'

  vars:
    proxmox:
    cloud_init_user: murat
    cloud_init_password: murat
    cloud_init_ip: '192.168.1.135/24'
    cloud_init_gw: '192.168.1.1'
````


2. **Playbook'u Çalıştır**: Aşağıdaki komut ile Ansible playbook'unu çalıştırın:

```bash
ansible-playbook -i inventory/cluster_inventory.yml site.yml
```


````bash
ansible-role-vm-proxmox/
├── ansible.cfg
├── collections
│   └── requirements.yml
├── hosts
├── inventory
│   └── server_groups.yml
├── LICENSE
├── playbooks
│   └── roles
│       └── create-vm
│           ├── files
│           │   └── ssh_key.pub
│           ├── handlers
│           ├── meta
│           ├── output
│           ├── tasks
│           │   ├── 01_ubuntu_temp.yml
│           │   ├── 01_ubuntu_vm.yml
│           │   ├── 02_debian_temp.yml
│           │   ├── 02_debian_vm.yml
│           │   └── main.yml
│           ├── templates
│           └── vars
│               └── main.yml
├── README.md
└── site.yml

12 directories, 14 files
````