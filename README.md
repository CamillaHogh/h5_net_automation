# Ansible Collection - sde.h5

Documentation for the collection.

## Roles
get_ios_vers - Collects information about IOS Version
get_run_config - Collects information about running configuration
get_start_config - Collects information about startup configuration
get_vlan_dat - Collects information about VLAN database
save_config - Saves the configuration collected to a file
update_ios - Updates the IOS image on the device

## Notes

### Init Ansible
```
sudo apt install ansible

# Creates folder structure for new collection
ansible-galaxy collection init namespace.collection

# Creates folder structure for new role
cd roles
ansible-galaxy roles init my_role
```

