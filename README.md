Чтобы применить изменения в членов организации выполняем
```
ansible-playbook playbooks/github.yml -e github_members__state=present
```

Чтобы применить изменения в команде выполняем
```
ansible-playbook playbooks/github.yml -e github_teams__state=present -e github_teams__include=команда
```

Чтобы применить изменения к репе выполняем
```
ansible-playbook playbooks/github.yml -e github_repos__state=present -e github_repos__include=репа
```
