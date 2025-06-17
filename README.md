# ansible

## Disk Monitoring with Alerts

The `disk_monitor.yml` playbook checks a host's disk usage and sends alerts when the usage exceeds a specified threshold. Alerts are delivered by email, Slack notification, and a local log entry.

### Prerequisites
1. Ansible installed on the control machine.
2. A valid inventory of target hosts.
3. SMTP access and a Slack token for notifications.

### Setup
1. **Configure variables** in `disk_monitor.yml`:
   - `disk`: filesystem to check (default `/`).
   - `threshold`: usage percentage that triggers an alert.
   - `email_to` and `email_from`: addresses for email alerts.
   - `smtp_host` and `smtp_port`: mail server settings.
   - `slack_token` and `slack_channel`: Slack credentials.
   - `log_file`: path for log warnings.
2. **Run the playbook**:
   ```bash
   ansible-playbook -i inventory disk_monitor.yml
   ```
3. If disk usage exceeds the threshold, Ansible will:
   - Send an email using the `mail` module.
   - Post a Slack message with `community.general.slack`.
   - Write a warning to the chosen log file.

### Example Inventory
```ini
[servers]
server1.example.com
server2.example.com
```

### Automating with Cron
To check disks every hour, create a cron job:
```bash
0 * * * * ansible-playbook -i /path/to/inventory /path/to/disk_monitor.yml
```

## User Management

The `user_management.yml` playbook creates or removes user accounts and sets up groups, passwords, and SSH keys.

### Usage
1. Edit the variables at the top of `user_management.yml` to define the account name, groups, password, and SSH key.
2. Run the playbook:
   ```bash
   ansible-playbook -i inventory user_management.yml
   ```

## Service Management

The `service_management.yml` playbook starts, stops, enables, or disables
services on target hosts.

### Usage
1. Edit the `services` list in `service_management.yml` to define each
   service name, desired state, and whether it should be enabled at boot.
2. Run the playbook:
   ```bash
   ansible-playbook -i inventory service_management.yml
   ```

Example structure for `services`:
```yaml
services:
  - name: nginx
    state: started
    enabled: true
  - name: sshd
    state: stopped
    enabled: false
```

## System Updates

The `system_update.yml` playbook installs the latest package updates and
security patches on target hosts. It can reboot machines when required.

### Usage
1. Set the `reboot` variable to `true` if you want hosts to reboot after
   applying updates when a reboot is needed.
2. Run the playbook:
   ```bash
   ansible-playbook -i inventory system_update.yml
   ```
