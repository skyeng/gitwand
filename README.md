# Управление Github-ом

## Краткое описание
На Github-е лежат репозитории всех проектов компании. Туда имеют доступ все команды разработки, все разработчики, тестировщики, аналитики и другие люди. В репозитории gitwand живёт код для управления всем этим хозяйством.

## Ответственные

- Code owner: Игорь Плехов
- Product owner: Артем Науменко
- Команда: Infra Operations



Manage your organization at Github:
* **Pointedly**: only that will be changed what has to be changed.
* **Fast**: as it does not do anything excessive.
* **Simply**: all configs are in YAML.
* Say *enough!* to cryptic nicknames in your configs.  Here you can use **real names** of your fellows.

## Organization
Describe all your organizations in `vars/github/orgs/`, one per file.  Typically you have only one organization, say `my_org`.
Then place it in `my_org.yml` like this:
```yaml
my_org:
  name: My Organization
  admin:
    - admin1
    - admin2
  member:
    - dude3
    - dude4
```
* `name` (required) is a name of you organization at Github.
* All other parameters are optional.
* **Notice** these lists are closed.  If you place here someone, they will be added to your organization.
  If you remove someone, they will be removed.

## Organization members
Place them all in `vars/users/`, one per file:
```yaml
john.smith:
  github: johnny-smith # his nickname at Github
```

## Teams
Place all your teams in `vars/github/teams/`, one per file:
```yaml
my_team:
  privacy: closed / secret
  parent: parent_team
  maintainer:
    - admin1
    - admin2
  member:
    - dude3
    - dude4
```
* `privacy` (required) determines [visibility](https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/about-teams#team-visibility) of the team.
* All other parameters are optional.
* **Notice** teams members lists are closed and operate the same way as a list of organization's members.

## Repositories
Here is where the real work is going.  Describe your repositories in `vars/github/repos`, one per file:
```yaml
my_repo:
  archived: true / false # this is one direction road: once archived the repo can be unarchived via web only
  default_branch: master
  description: >-
    Brief description.  Try to fit it in one line.  As linefeeds are not allowed here.
  has_issues: true / false
  has_wiki: true / false
  homepage: https://example.com
  language: PHP
  private: true / false
  teams:
    admin:
      ## List of admin teams.  Any member of an admin team will have admin access to this repo.
      - team1
      - team2
    push:
      ## List of teams with push access.
      - all
    pull:
      ## List of teams who can only read this repo.
      - team3
      - team4
  collaborators:
    direct:
      ## List of scattered admins.
      - admin0
    outside:
      ## Not members of your organization.  Think about freelancers.  They'll get push access to this repo.
      - freelancer
```
* All parameters are optional.
* If you have a large number of repositories it's okay to have here only few.
  We're not going to touch anything else, what's not described here.
* Those lists of teams and collaborators are closed as in other places.

# How to use it
So far so good?  Aren't you tired with describing all your staff?  Let's now play with it. :-)

To apply changes in your organization members run:
```
ansible-playbook playbooks/gitwand.yml -e github_members__state=present
```

Have changed only one user?  Would like to update just him or her?  Do like following.  And don't forget,
we use real names (not their own nicknames at Github) here as described in `vars/users/`.
```
ansible-playbook playbooks/gitwand.yml -e github_members__state=present -e github_members__include=john.smith
```

Update all your teams:
```
ansible-playbook playbooks/gitwand.yml -e github_teams__state=present
```

Or better just one:
```
ansible-playbook playbooks/gitwand.yml -e github_teams__state=present -e github_teams__include=my_team
```

Dump all your teams:
```
ansible-playbook playbooks/gitwand.yml -e github_teams__state=dump
```

And sure, you can dump info about only one team as well:
```
ansible-playbook playbooks/gitwand.yml -e github_teams__state=dump -e github_teams__include=my_team
```

The same about repos.  Update all your repositories at once:
```
ansible-playbook playbooks/gitwand.yml -e github_repos__state=present
```

Or better update only one repo:
```
ansible-playbook playbooks/gitwand.yml -e github_repos__state=present -e github_repos__include=my_repo
```

Dump all repos:
```
ansible-playbook playbooks/gitwand.yml -e github_repos__state=dump
```

Or dump only one repo:
```
ansible-playbook playbooks/gitwand.yml -e github_repos__state=dump -e github_repos__include=my_repo
```
