# Dynamically build Ansible inventory

This is a dynamic alternative to Ansible [static inventory file](http://docs.ansible.com/ansible/intro_inventory.html)

Instead of writing a static `hosts` file with groups and hosts, it simply reads the YAML files in `host_vars/*.yml` and creates a dynamic inventory based on them.

The dynamic inventory script is called [inventory](inventory) in this example and simply by running it you should get some json output:

    ~/ansible-inventory (master) $ ./inventory.py | python -m json.tool
    {
        "databases": {
            "hosts": [
                "db1.example.com"
            ]
        },
        "dc_atlanta": {
            "hosts": [
                "db1.example.com",
                "web1.example.com",
                "worker4.example.com"
            ]
        },
        "dc_london": {
            "hosts": [
                "web2.example.com"
            ]
        },
        "webservers": {
            "hosts": [
                "web1.example.com",
                "web2.example.com"
            ]
        },
        "workers": {
            "hosts": [
                "worker4.example.com"
            ]
        }
    }

You also need to set the inventory script in [ansible.cfg](ansible.cfg):

    ~/ansible-inventory (master) $ cat ansible.cfg
    [defaults]
    inventory = ./inventory

And you are good to go. When running `ansible` and `ansible-playbook` it will read the output from the dynamic inventory. Adding/removing a host is as simple as adding/removing a file in `host_vars/`. Just make sure that `host_fqdn` and `host_groups` is set.

## More info

* http://docs.ansible.com/ansible/intro_dynamic_inventory.html
* http://docs.ansible.com/ansible/dev_guide/developing_inventory.html

