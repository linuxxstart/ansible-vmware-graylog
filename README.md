# Ansible VMware

Criando uma maquina virtual no VMware e instalando o graylog server.

# Pré requisitos
Matrix de compatibilidade dos sistemas operacionais das vms.

http://partnerweb.vmware.com/programs/guestOS/guest-os-customization-matrix.pdf

Exemplo que estou usando é o ubuntu-18.04.

Instalar o ansible.

https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html

Instalar o pip do python para instalar a API de conexão com o vCenter.

```
sudo apt install python-pip
sudo pip install pyvmomi
```
# 1 Criando variáveis

Depois de clonar o repositório edite o arquivo.
```
$ vim group_vars/all
```
Encripitar as senhas dos usuários user@vcenter.domain e root. Vai pedir para definir uma senha, pode ser qualquer uma, ela será pedida na exeção do playbook.
```
$ ansible-vault encrypt_string 'senhadoroot' --name 'qualquernome'
New Vault password: 
Confirm New Vault password: 
qualquernome: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          31396333636539343661313062396232633261306435646562383064383866333865363566356138
          3937656432313439336338653565323836356137333338330a346134653864623337323430313366
          34363835653337383039653166396161316662616534336133626538663537653235653139386438
          6665376565363934300a643065613139663462343236613138616465363038343130633833613365
          3234

Copiar a parte: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          31396333636539343661313062396232633261306435646562383064383866333865363566356138
          3937656432313439336338653565323836356137333338330a346134653864623337323430313366
          34363835653337383039653166396161316662616534336133626538663537653235653139386438
          6665376565363934300a643065613139663462343236613138616465363038343130633833613365
          3234
E cole na variavel de senha de root. Repita o processo com a senha do usuário user@vcenter.domain
```


# 2 Executar o playbook
```
$ ansible-playbook -i vms_deploy main.yml --ask-vault-pass
```

# 3 Deletando as VMs
```
$ ansible-playbook -i vms_deploy delete.yml --ask-vault-pass
```

# 4 Usando tags específicas 
```
$ ansible-playbook -i vms_deploy main.yml --tags "add-repo-graylog,install-graylog,config-graylog,start-graylog" --ask-vault-pass
```

# 5 Acessando o Graylog

Usar http://ip_do_servidor com usuário: admin e senha: senha, cadastrada na role do graylog na task "Configurar Graylog".