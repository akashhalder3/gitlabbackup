
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash

sudo EXTERNAL_URL="https://envoy.coderepo.online/" sudo apt install gitlab-ce=17.5.0-ce.0

sudo gitlab-ctl reconfigure

sudo cp /etc/gitlab/gitlab.rb /var/opt/gitlab/backups

sudo cp /etc/gitlab/gitlab-secrets.json /var/opt/gitlab/backups

sudo cp -r /var/opt/gitlab/git-data /var/opt/gitlab/backups

sudo cp -r /etc/gitlab/ssl /var/opt/gitlab/backups

scp /var/opt/gitlab/backups/1735037577_2024_12_24_17.5.0_gitlab_backup.tar root@91.108.110.239:/backup/location/root/gitlab

scp /var/opt/gitlab/backups/gitlab-secrets.json root@91.108.110.239:/root/gitlab

scp /var/opt/gitlab/backups/gitlab.rb root@91.108.110.239:/root/gitlab

scp -r /var/opt/gitlab/backups/ssl root@91.108.110.239:/root/gitlab

scp -r /var/opt/gitlab/backups/git-data root@91.108.110.239:/root/gitlab

