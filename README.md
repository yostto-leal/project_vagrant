# project_vagrant
criaçãode uma vm com o vagrantfile com  virtualbox,utilizando box linux oficial, criar uma automação simples com ansible, atualizar os pacotes (upgrade)

Certo, aqui está um exemplo de como criar uma VM com Vagrant utilizando a box Linux oficial e automatizar a instalação do Nginx e a exibição de "Hello, World!" usando Ansible:

1. Primeiro, vamos criar o Vagrantfile:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
  end
end
```

Este Vagrantfile define a caixa (box) Ubuntu 20.04 (Focal Fossa) e encaminha a porta 80 da VM para a porta 8080 do host.

2. Agora, vamos criar o playbook Ansible `playbook.yml`:

```yaml
- hosts: all
  become: true
  tasks:
    - name: Update package repositories
      apt:
        update_cache: yes
        
    - name: Upgrade packages
      apt:
        upgrade: dist
        
    - name: Install Nginx
      apt:
        name: nginx
        state: present
        
    - name: Create index.html
      copy:
        content: "Hello, World!"
        dest: /var/www/html/index.html
```

Este playbook Ansible realiza as seguintes tarefas:

- Atualiza os repositórios de pacotes
- Atualiza todos os pacotes instalados
- Instala o pacote Nginx
- Cria o arquivo `index.html` com o conteúdo "Hello, World!" na raiz do servidor web

3. Agora, basta executar os seguintes comandos:

```
vagrant up
```

Esse comando criará a VM usando o Vagrantfile e, em seguida, executará o playbook Ansible para realizar as tarefas de atualização e instalação do Nginx.

Após a conclusão da execução, você pode acessar a página "Hello, World!" em `http://localhost:8080`.

Aqui está um resumo do que este exemplo faz:

1. O Vagrantfile define a caixa (box) Linux a ser usada e provisiona a VM com o playbook Ansible.
2. O playbook Ansible atualiza os pacotes do sistema, instala o Nginx e cria o arquivo `index.html` com o conteúdo "Hello, World!".
3. Ao executar `vagrant up`, o Vagrant cria a VM e o Ansible provisiona a automação.
4. Você pode acessar a página "Hello, World!" em `http://localhost:8080`.




   
 






