# project_vagrant
criaçãode uma vm com o vagrantfile com  virtualbox,utilizando box linux oficial, criar uma automação simples com ansible, atualizar os pacotes (upgrade)

PARTE !
1: Criar uma VM com o Vagrant e VirtualBox
Instalar o Vagrant e o VirtualBox:

Baixe e instale o Vagrant e o VirtualBox.
2 Criar diretorio para projeto
3 inicializar o Vagrant
 vagrant init debian/jessie
4 configure o VAgrantfgile

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.network "private_network", type: "dhcp"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end
end
5 inicia a VM
 vagrant up
6 acessa a VM
 depois que a VM estiver em exercução, voçe poode acessar via ssh
 vagrant ssh

 PARTE 2
 Criar uma automação simples com Ansible para atualizar os pacotes
 1 Instalar o Ansible:
  sudo apt update
  sudo apt install ansible
  
 2 Configure o inventário:
Crie um arquivo de inventário (hosts) para definir os servidores que você deseja gerenciar.  

 3 Crie o playbook do Ansible:

Escreva um playbook para atualizar os pacotes. Crie um arquivo chamado update.yml 
---
- hosts: servidores
  become: yes
  tasks:
    - name: Atualizar pacotes do sistema
      apt:
        update_cache: yes
        upgrade: dist
        autoremove: yes
        autoclean: yes

      Execute o playbook:

 4 Execute o playbook usando o comando abaixo:
 ansible-playbook -i host update.yml
 
 5 instalacao do nginx e colocar hello world na  tela
 Certo, aqui está um exemplo de playbook Ansible para instalar o Nginx e configurá-lo para exibir "Hello, World!" na tela:

```yaml
- hosts: webservers
  become: true
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present
        update_cache: yes

    - name: Create default Nginx configuration
      template:
        src: default.conf.j2
        dest: /etc/nginx/conf.d/default.conf
      notify:
        - restart nginx

    - name: Start and enable Nginx
      service:
        name: nginx
        state: started
        enabled: yes

  handlers:
    - name: restart nginx
      service:
        name: nginx
        state: restarted
```

Vamos explicar o que cada parte do playbook faz:

1. `- hosts: webservers`: Define os hosts-alvo para a execução do playbook.
2. `become: true`: Concede privilégios de superusuário para as tarefas.
3. `- name: Install Nginx`: Instala o pacote Nginx.
4. `- name: Create default Nginx configuration`: Cria o arquivo de configuração padrão do Nginx usando um template.
   - `src: default.conf.j2`: Especifica o template a ser usado. Veja abaixo.
   - `dest: /etc/nginx/conf.d/default.conf`: Define o caminho do arquivo de configuração.
   - `notify: - restart nginx`: Notifica o manipulador "restart nginx" para reiniciar o Nginx após a alteração da configuração.
5. `- name: Start and enable Nginx`: Inicia e habilita o serviço Nginx para iniciar automaticamente após o boot.
6. `handlers:`: Define os manipuladores que podem ser acionados por tarefas.
   - `- name: restart nginx`: Reinicia o serviço Nginx.

O template `default.conf.j2` deve conter o seguinte conteúdo:

```nginx
server {
    listen 80;
    server_name _;

    location / {
        root /var/www/html;
        index index.html index.htm;
        return 200 'Hello, World!';
    }
}
```

Este template define um bloco de servidor Nginx que escuta na porta 80 (HTTP) e retorna a mensagem "Hello, World!" na raiz do site.

Para usar este playbook, siga estes passos:

1. Certifique-se de ter o Ansible instalado.
2. Crie o diretório do projeto e os arquivos `hosts` e `playbook.yml` conforme mostrado acima.
3. Crie o template `default.conf.j2` na mesma pasta do playbook.
4. Execute o playbook com o comando `ansible-playbook -i hosts playbook.yml`.

Após a execução, você deve ser capaz de acessar a máquina via HTTP e ver a mensagem "Hello, World!" exibida.

   
 






