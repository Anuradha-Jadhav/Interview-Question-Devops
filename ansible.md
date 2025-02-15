# Ansible Overview

Ansible is an open-source automation tool used for configuration management, application deployment, and task automation. It simplifies IT orchestration by allowing infrastructure as code (IaC) management with human-readable YAML scripts.

## **Architecture**
Ansible follows an agentless architecture, meaning it does not require any software to be installed on managed nodes. It communicates over SSH (Linux) or WinRM (Windows).

### **Components**
1. **Control Node**: The machine where Ansible is installed and from where automation is executed.
2. **Managed Nodes**: The remote machines controlled by Ansible.
3. **Inventory**: A file that lists the target machines.
4. **Modules**: Predefined tasks that Ansible executes.
5. **Handlers**: Special tasks triggered by other tasks.
6. **Roles**: A structured way to group tasks, handlers, variables, and templates for reusability.
7. **Plugins**: Extend Ansible functionalities (e.g., connection, lookup, cache).
8. **Variables and Facts**: Dynamic values that can be used within playbooks.
9. **Templates**: Jinja2-based files for dynamic configuration generation.
10. **Ansible Vault**: Encrypts sensitive data in playbooks.

## **Workflow**
1. Install Ansible on the control node.
2. Define managed nodes in an inventory file.
3. Execute ad-hoc commands or structured automation tasks using playbooks.
4. Use modules, variables, and templates for dynamic automation.
5. Apply roles for modular automation.

## **Modules in Ansible**
Modules are small programs that Ansible runs on managed nodes. They are the building blocks for Ansible playbooks and ad-hoc commands.

### **Commonly Used Modules**
1. **System Modules**
   - `user`: Manage user accounts
   - `group`: Manage groups
   - `hostname`: Manage hostnames
   - `cron`: Manage cron jobs

2. **File and Directory Modules**
   - `file`: Set file attributes like ownership and permissions
   - `copy`: Copy files to remote systems
   - `fetch`: Fetch files from remote nodes to the control node
   - `template`: Manage configuration files using Jinja2 templates

3. **Package Management Modules**
   - `yum`: Manage packages on RedHat-based systems
   - `apt`: Manage packages on Debian-based systems
   - `dnf`: Manage packages on Fedora systems
   - `pip`: Manage Python packages

4. **Service Management Modules**
   - `service`: Manage services (start, stop, restart, enable, disable)
   - `systemd`: Manage systemd services
   - `shell`: Execute shell commands on remote hosts

5. **Networking Modules**
   - `ping`: Test connectivity to remote nodes
   - `firewalld`: Manage firewall rules on Linux
   - `iptables`: Manage iptables rules

6. **Cloud Modules**
   - `ec2`: Manage AWS EC2 instances
   - `azure_rm`: Manage Azure resources
   - `gcp_compute`: Manage Google Cloud instances

7. **Database Modules**
   - `mysql_db`: Manage MySQL databases
   - `postgresql_db`: Manage PostgreSQL databases

8. **Security Modules**
   - `authorized_key`: Manage SSH keys for users
   - `ufw`: Manage Uncomplicated Firewall (UFW) settings
   - `seboolean`: Modify SELinux booleans

## **Basic Commands**
```sh
# Check Ansible version
ansible --version

# Ping all managed nodes
ansible all -m ping

# Execute a command on all nodes
ansible all -m command -a "uptime"

# Encrypt a file using Ansible Vault
ansible-vault encrypt secrets.yml

# Decrypt a file using Ansible Vault
ansible-vault decrypt secrets.yml
```

## **Example Playbook**
```yaml
- name: Deploy Web Server
  hosts: webservers
  become: yes
  tasks:
    - name: Install Apache
      yum:
        name: httpd
        state: present

    - name: Start Apache Service
      service:
        name: httpd
        state: started
        enabled: yes

    - name: Copy Index.html File
      copy:
        src: files/index.html
        dest: /var/www/html/index.html
```

## **Roles in Ansible**
Roles allow you to group tasks, handlers, variables, templates, and other resources in a structured manner, promoting reusability.

### **Example Directory Structure of a Role**
```sh
myrole/
├── defaults/
│   └── main.yml
├── files/
├── handlers/
│   └── main.yml
├── meta/
│   └── main.yml
├── tasks/
│   └── main.yml
├── templates/
│   └── config.j2
├── vars/
│   └── main.yml
```

### **Example Role Task File (`tasks/main.yml`)**
```yaml
- name: Install Apache
  yum:
    name: httpd
    state: present

- name: Start Apache Service
  service:
    name: httpd
    state: started
    enabled: yes
```

## **Templates in Ansible**
Templates are used to create dynamic configuration files using Jinja2 syntax.

### **Example Template File (`templates/config.j2`)**
```yaml
server_name {{ server_name }};
listen {{ port }};
root {{ document_root }};
```

### **Example Task Using a Template**
```yaml
- name: Deploy Nginx Configuration
  template:
    src: templates/config.j2
    dest: /etc/nginx/sites-available/default
  notify: Restart Nginx
```

## **Advantages of Ansible**
- **Agentless**: No need to install software on managed nodes.
- **Simple YAML Syntax**: Human-readable automation scripts.
- **Idempotent Execution**: Ensures consistent state without unnecessary changes.
- **Scalability**: Can manage thousands of nodes efficiently.
- **Integration with Cloud Providers**: Works with AWS, Azure, and more.

## **Use Cases**
- Server provisioning
- Configuration management
- Application deployment
- Continuous integration and delivery (CI/CD)
- Security automation

---

For more details, refer to the [official Ansible documentation](https://docs.ansible.com/).

