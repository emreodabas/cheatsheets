
## clone all project under gitlab group  

for repo in $(curl "https://#{gitlab_host}/api/v4/groups/528?private_token=${your_token}" | jq .projects[].ssh_url_to_repo | tr -d '"'); do git clone $repo; done;

## git password reminder
 
git config --global credential.helper store
