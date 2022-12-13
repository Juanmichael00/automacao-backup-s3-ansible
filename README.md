# Automação de backup com Ansible, S3CMD, KMS e GPG para Bucket S3:
## Author deste projeto: Juan Michael
### Arquivos: automacao-backup-s3-ansible/...
### Sobre o Projeto:
##### Este Projeto visa trazer uma automação no envio e download de objetos para o s3 da AWS utilizando as seguintes ferramentas: Ansible e S3CMD, com foco de segurança dos dados em repouso "KMS" e trânsito "GPG e HTTPS"

- 1°Primeiros Passos:
Criar chave KMS assíncrona

- 2°Segundo Passo:
Criar um Bucket S3 na AWS habilitando a criptografia em repouso (KMS) e atachando policy para bloqueio contra exclusão

- 3°Terceiro Passo:
Criar usuário de acesso programático na AWS com policy s3fullacess atachada ao usuário. (ideal criar 2 usuários diferentes para put e get com as devidas permissões)

- 4°Quarto Passo:
Instalação do Ansible na máquina que executará o Playbook:
   sudo apt-get install python3.9
   sudo apt install ansible

- 5° Quinto Passo:
Adicionar sua chave SSH pública da máquina que está executando o Ansible na máquina que você irá acessar:

Exemplo: copiar chave da máquina local: cat /root/.ssh/id_rsa.pub e colar no o servidor de destino vim /home/ubuntu/.ssh/authorized_keys

Pronto, agora você tem acesso a máquina de destino logando com o user Ubuntu > Root 

- 6° Sexto Passo:
Em upload_backup_s3/templates/user_put_s3 e download_backup_s3/templates/user_get_s3, adicionar os seguintes valores:

###### access_key =
###### secret_key = 
###### bucket_location = region
###### host_base = s3.amazonaws.com
###### host_bucket = nome-bucket.s3.amazonaws.com
###### gpg_passphrase = Adicionar uma chave (senha) ou gerar com "openssl rand -base64 30"

- 7° Sétimo Passo:
Editar o arquivo hosts do ansible e adicionar o IP Público da máquina que está o backup:
ansible_host=ipadress

- 8° Oitavo Passo:
Fazer as devidas alterações no main.yml
"caminho do backup" e "nome do bucket"

- 9° Nono passo
acessar o diretório do playbook e rodar o comando:

    ansible servidores -i hosts -m ping           Para teste de conexão
    ansible-playbook site.yml -i hosts            Para executar o playbook

### Documentação:

- Doc Ansible Modules: https://docs.ansible.com/ansible/latest/collections/ansible/builtin/apt_module.html
- Doc KMS:             https://docs.ansible.com/ansible/latest/collections/ansible/builtin/apt_module.html
- Doc S3CMD:           https://s3tools.org/usage
- Doc AWS S3:          https://aws.amazon.com/pt/s3/

