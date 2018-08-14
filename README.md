# aspects_yum_cron

Install and configure yum-cron to run updates on RedHat family OS's.

# Requirements
Set ```hash_behaviour=merge``` in your ansible.cfg file.

Not a requirement, but aspects_cron would be useful if you want to adjust when the updates occur.
# Role Variables

### `aspects_yum_cron_enabled`
Boolean. Default `False`. Set to `True` if you want the tasks in this role to run.

### Others
See templates/etc-sysconfig-yum-cron.j2 for what other variables are valid.

See defaults/main.yml for default values.

# Example Playbook

```yaml
- hosts:
  - aspects_yum_cron
  #  - aspectscentos7
  #  - aspectstrusty
  #  - aspectsxenial
  #  - aspectsstretch
  #  - aspectsoraclelinux7
  #  - aspectbionic
  vars:
    aspects_packages_enabled: True
    aspects_yum_cron_enabled: True
    aspects_yum_cron_config:
      #  YUM_PARAMETER:
      CHECK_ONLY: no
      CHECK_FIRST: yes
      DOWNLOAD_ONLY: no
      ERROR_LEVEL: 0
      DEBUG_LEVEL: 1
      RANDOMWAIT: "62"
      #  MAILTO:
      #  SYSTEMNAME: ""
      DAYS_OF_WEEK: "0123456"
      CLEANDAY: "0"
      SERVICE_WAITS: yes
      SERVICE_WAIT_TIME: 300
  roles:
  - aspects_yum_cron
```

# License

MIT