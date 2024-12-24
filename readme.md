The warning you see is expected behavior. The `gitlab-backup` command only backs up repositories, database contents, and uploads but does **not** include configuration files (`gitlab.rb` and `gitlab-secrets.json`) due to security concerns.

To create a complete backup, you must manually back up these configuration files. Here's how to ensure a complete backup:

---

### **Complete Backup Process**
1. **Run the GitLab Backup Command**
   ```bash
   sudo gitlab-backup create
   ```
   This will generate a backup tar file in the default backup directory (`/var/opt/gitlab/backups/`).

2. **Manually Back Up Configuration Files**
   - Copy the `gitlab.rb` and `gitlab-secrets.json` files:
     ```bash
     sudo cp /etc/gitlab/gitlab.rb /var/opt/gitlab/backups/
     sudo cp /etc/gitlab/gitlab-secrets.json /var/opt/gitlab/backups/
     ```

3. **Package the Backup**
   - Include everything (backup tar file, `gitlab.rb`, and `gitlab-secrets.json`) in a compressed archive:
     ```bash
     cd /var/opt/gitlab/backups/
     sudo tar -czvf gitlab_complete_backup_$(date +%F).tar.gz *.tar gitlab.rb gitlab-secrets.json
     ```

4. **Store the Backup Securely**
   - Transfer the compressed file to a secure location:
     ```bash
     scp /var/opt/gitlab/backups/gitlab_complete_backup_$(date +%F).tar.gz user@backup-server:/backup/location/
     ```

---

### **Why Back Up `gitlab.rb` and `gitlab-secrets.json`?**
- **`gitlab.rb`**: Contains your GitLab instance’s configuration (e.g., external URLs, SMTP settings, etc.).
- **`gitlab-secrets.json`**: Stores encryption keys for sensitive data like database secrets, secure tokens, and CI/CD variables.

Without these files, you **cannot fully restore** your GitLab instance to its previous state.

---

### **Automate Configuration File Backups**
To automate this process, append the following to a cron job:

1. Open the cron editor:
   ```bash
   sudo crontab -e
   ```

2. Add the following job:
   ```bash
   0 2 * * * sudo gitlab-backup create && sudo cp /etc/gitlab/gitlab.rb /etc/gitlab/gitlab-secrets.json /var/opt/gitlab/backups/ && cd /var/opt/gitlab/backups/ && sudo tar -czvf gitlab_complete_backup_$(date +%F).tar.gz *.tar gitlab.rb gitlab-secrets.json
   ```

This will create a complete backup daily at 2 AM.

---

Let me know if you need further clarification or help setting this up!