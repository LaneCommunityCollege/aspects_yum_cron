# aspects_yum_cron

Install and configure yum-cron to run updates on CentOS 7.

> Warning: As of 2020-02-18 configuration has been rewritten and old configuration will not 
> work.

# Requirements
Set ```hash_behaviour=merge``` in your ansible.cfg file.

# Role Variables
### `aspects_yum_cron_enabled`
Boolean. Default `False`. Set to `True` if you want the tasks in this role to run.

### yum-cron configuration
There are three dictionaries to configure three levels of yum-cron runs.

* aspects_yum_cron_config_default
* aspects_yum_cron_config_hourly
* aspects_yum_cron_config_security

Each has two sub items.

`enabled` determines if the configuration is used, and if existing configuration should be removed.
`config` is a dictionary with the actual configuration for each config file's template.

The templates check if the key is defined and isn't empty, so you can leave off any values you don't want
to specify.

Available values are:

* `update_cmd`
* `update_messages`
* `download_updates`
* `apply_updates`
* `random_sleep`
* `system_name`
* `emit_via`
* `output_width`
* `email_from`
* `email_to`
* `email_host`
* `group_list`
* `group_package_types`
* `debuglevel`
* `skip_broken`
* `mdpolicy`
* `assumeyes`

What each key configures can be figured out from the template file 
[templates/etc-yum-yum-cron.conf.j2](templates/etc-yum-yum-cron.conf.j2) 
(it contains comments copied from the default CentOS 7 configuration) or you can check `man yum-cron`.

### Defaults
[defaults/main.yml](defaults/main.yml) contains defaults disabling the security and hourly jobs, and
setting the default job to only email root@localhost with a list of available updates.

# Example Playbook

```yaml
- hosts:
  - aspects_yum_cron
  vars:
    aspects_packages_enabled: True
    aspects_yum_cron_enabled: True
    aspects_yum_cron_config_default:
      enabled: True
      config:
        update_cmd: "default"
        update_messages: "yes"
        download_updates: "no"
        apply_updates: "no"
        random_sleep: "1"
        system_name: "{{ ansible_fqdn }}"
        emit_via: "email"
        output_width: "80"
        email_from: "root+yum-cron@{{ ansible_fqdn }}"
        email_to: "root@localhost, you@yourdomain.com"
        email_host: "localhost"
        group_list: "None"
        group_package_types: "mandatory, default"
        debuglevel: "-2"
        mdpolicy: "group:main"
    aspects_yum_cron_config_hourly:
      enabled: False
      config:
        update_cmd: "default"
        update_messages: "no"
        download_updates: "no"
        apply_updates: "no"
        random_sleep: "15"
        system_name: "None"
        emit_via: "stdio"
    aspects_yum_cron_config_security:
      enabled: True
      config:
        update_cmd: "security"
        update_messages: "yes"
        download_updates: "yes"
        apply_updates: "yes"
        random_sleep: "1"
        system_name: "{{ ansible_fqdn }}"
        emit_via: "email"
        output_width: "80"
        email_from: "root+yum-cron-security@{{ ansible_fqdn }}"
        email_to: "root@localhost, you@yourdomain.com"
        email_host: "localhost"
        group_list: "None"
        group_package_types: "mandatory, default"
        debuglevel: "-2"
        mdpolicy: 'group:main'
  roles:
  - aspects_yum_cron
```

# License

MIT