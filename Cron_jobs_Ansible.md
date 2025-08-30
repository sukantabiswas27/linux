
# Automation: Cron Jobs & Ansible

## 1. Cron Jobs

### What is a Cron Job?
- A **cron job** is a scheduled task in Unix/Linux systems.
- It allows you to run scripts or commands automatically at **specific intervals** (e.g., every day at 2 AM, every Monday, every 5 minutes).

### How Cron Works
- **Cron daemon (`crond`)** runs in the background and checks the schedule defined in cron tables (**crontabs**).
- Each user can have their **own crontab**.
- **Crontab file format** (5 fields + command):
  
```
*    *    *    *    *  command_to_run
-    -    -    -    -
|    |    |    |    |
|    |    |    |    +----- Day of the week (0-7) (Sunday=0 or 7)
|    |    |    +------- Month (1-12)
|    |    +--------- Day of month (1-31)
|    +----------- Hour (0-23)
+------------- Minute (0-59)
```

### Examples of Cron Jobs

1. **Run a script every day at 2 AM**:
```bash
0 2 * * * /home/user/backup.sh
```

2. **Run a script every 15 minutes**:
```bash
*/15 * * * * /home/user/check_logs.sh
```

3. **Run a script every Monday at 6 AM**:
```bash
0 6 * * 1 /home/user/weekly_report.sh
```

### Crontab Commands

- **Edit crontab**:
```bash
crontab -e
```

- **List crontab**:
```bash
crontab -l
```

- **Remove crontab**:
```bash
crontab -r
```

- **Check cron logs**:
```bash
tail -f /var/log/cron
```

> Tip: Always use **absolute paths** in cron commands because the cron environment has a limited PATH.

---

## 2. Ansible

### What is Ansible?
- **Ansible** is an **open-source IT automation tool**.
- It automates tasks like:
  - Configuration management
  - Application deployment
  - Orchestration of multiple systems
- **Agentless**: It doesn’t require software to be installed on target machines. It uses **SSH**.

### Core Concepts of Ansible

1. **Inventory**
   - A file listing all the servers you want to manage.
   - Example `hosts.ini`:
   ```ini
   [webservers]
   web1.example.com
   web2.example.com

   [dbservers]
   db1.example.com
   ```

2. **Playbook**
   - A YAML file defining tasks to run on servers.
   - Example `deploy.yml`:
   ```yaml
   - name: Deploy a web application
     hosts: webservers
     become: yes
     tasks:
       - name: Install Nginx
         apt:
           name: nginx
           state: present

       - name: Start Nginx
         service:
           name: nginx
           state: started
   ```

3. **Modules**
   - Ansible has built-in modules to perform tasks like installing packages, copying files, restarting services, etc.
   - Examples: `apt`, `yum`, `copy`, `service`, `git`.

4. **Roles**
   - Predefined reusable sets of tasks and configurations.
   - Helps in organizing complex deployments.

### Running Ansible

- **Ping servers**:
```bash
ansible all -m ping -i hosts.ini
```

- **Run a playbook**:
```bash
ansible-playbook -i hosts.ini deploy.yml
```

- **Check dry run** (simulate changes):
```bash
ansible-playbook -i hosts.ini deploy.yml --check
```

### Cron + Ansible Together
- You can **manage cron jobs on multiple servers** using Ansible.
- Example task in Ansible playbook to create a cron job:
```yaml
- name: Ensure backup cron job exists
  cron:
    name: "Daily backup"
    minute: "0"
    hour: "2"
    job: "/usr/local/bin/backup.sh"
```
- This will deploy the cron job across all targeted servers, ensuring consistency.

### Benefits

| Automation Tool | Use Case | Benefits |
|-----------------|----------|---------|
| **Cron**        | Schedule recurring tasks on a single server | Simple, lightweight, runs locally, great for repetitive tasks |
| **Ansible**     | Deploy apps, manage configurations on multiple servers | Agentless, scalable, consistent, integrates with CI/CD pipelines |

---

### Summary
- **Cron Jobs** → Local task scheduling (minutes, hours, days).
- **Ansible** → Multi-server automation (deployment, config, orchestration).
- **Combined** → Use Ansible to deploy cron jobs across many servers easily.
