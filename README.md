# ansible-homework-1
part 1 of the ansible homework assignments

[routers] (connects to router's address, sets the hostname, and sets the interface descriptions
router1 ansible_host=192.168.100.100 device_hostname=Core-Router-East interface_description="Link to Management Network - East Side"
router2 ansible_host=192.168.100.101 device_hostname=Core-Router-West interface_description="Link to Management Network - West Side"

[routers:vars] (connection and CLI information for the routers)
ansible_connection=network_cli
ansible_network_os=cisco.ios.ios
ansible_user=cisco
ansible_password=cisco
ansible_ssh_common_args='-o StrictHostKeyChecking=no'


---
- name: Test Connectivity and Add Loopback to Routers (forgot to change this from the last lab)
  hosts: routers
  gather_facts: no

  tasks:
    - name: Configure hostname
      ios_config:
        lines:
          - hostname {{ device_hostname }} sets the hostname to what is configured in the inventory.ini variable device_hostname)

    - name: Configure description on Ethernet0/0
      ios_config:
        parents:
          - interface Ethernet0/0
        lines:
          - description {{ interface_description }} sets the interface description to what is configured in the inventory.ini variable interface_description)

(the rest shows the command output of the hostname and interface description changes) 

    - name: Verify hostname
      ios_command:
        commands:
          - show running-config | include hostname
      register: hostname_output

    - name: Show hostname output
      debug:
        var: hostname_output.stdout_lines

    - name: Verify Ethernet0/0 description
      ios_command:
        commands:
          - show running-config interface Ethernet0/0
      register: int_output

    - name: Show interface Ethernet0/0 config
      debug:
        var: int_output.stdout_lines    
