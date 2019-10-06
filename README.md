## Ansible - MySQL installation-Playbook 


#### Lets go through the ansible-playbook for this repo.

Playbooks are a completely different way to use ansible than in ad-hoc task execution mode, and are particularly powerful.Each playbook is composed of one or more 'plays' in a list.   

You can  access playbook of our repo from **_[here](https://github.com/JUZZINN/Ansible---Mariadb-installation-Playbook-/blob/master/mariadb-installation.yml.txt)_**
 
 #####  To prompt the user for certain input, and can do so with the ‘vars_prompt’ section.
 
 - The user input is hidden by default but it can be made visible by setting ***private:no***


```
  vars_prompt:
    - name: mysql_user
      private: no
      prompt: "Enter User name"

    - name: mysql_password
      private: yes
      prompt: "Enter User Password"

    - name: mysql_database
      private: no
      prompt: "Enter Database Name"

    - name: mysql_root
      private: yes
      prompt: "Enter root password"
```

<br>

----------------------------

<br>

## TASKS

- **Installing Mariadb** 


```

    - name: "Installing Mariadb"
      yum:
        name:
          - mariadb-server
          - MySQL-python

        state: present

```

- **Restarting/ Enabling service**

```
    - name: "Restarting/ enabling service"
      service:
        name: mariadb
        state: restarted
        enabled: true
```


- **MariaDB - reset Root Passsword**

```
      ignore_errors: yes
      mysql_user:
        login_user: root
        login_password: ''
        user: root
        password: "{{ mysql_root }}"
        host_all: yes
```

- **MariaDB - Deleting Anonymous user**  

```
    - name: "MariaDB - Deleting Anonymous user"
      mysql_user:
        login_user: root
        login_password: "{{ mysql_root }}"
        user: ''
        host_all: yes
        state: absent
         
 ```
        
- **Mariadb - Creating Extra Database** 

```
    - name: "Mariadb -Creating extra Database"
      mysql_db:
        login_user: root
        login_password: "{{ mysql_root }}"
        name: "{{ mysql_database }}"
        state: present

```

- **Mariadb - Creating /Granting extra use**  

```
    - name: "Mariadb - Creating /granting extra user"
      mysql_user:
        login_user: root
        login_password: "{{ mysql_root }}"
        name: "{{ mysql_user }}"
        state: present
        password: "{{ mysql_password }}"
        host: localhost
        priv: "{{ mysql_database }}.*:ALL"
```
