# aspects_yum_cron

Install and configure yum-cron to run updates on RedHat family OS's.

# Requirements
Set ```hash_behaviour=merge``` in your ansible.cfg file.

Not a requirement, but aspects_cron would be useful if you want to adjust when the updates occur.
# Role Variables
See templates/etc-sysconfig-yum-cron.j2 for what vars are valid.

See defaults/main.yml for default values.

# Example Playbook

    - hosts: servers
      roles:
         - { role: aspects_yum_cron }
      vars:
        aspects_yum_cron_config:
          MAILTO: "admin2@example.com"

# License

MIT