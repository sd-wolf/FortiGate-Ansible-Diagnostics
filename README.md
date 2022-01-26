# FortiGate-Ansible-Diagnostics

A collection of Ansible playbooks for automating the collection of information from FortiGates, in order to accelerate investigations and engagement with Fortinet TAC. It enables engineers with little or no experience with FortiGates to capture required technical information.

These playbooks use the Ansible raw module to:
1. Create a script of the diagnostic commands.
2. SSH onto the FortiGate and run the commands.
3. Email the results of the diagnostic commands to a specified email address.

The current list of diagnostics are:
* CPU and Memory
* Flow Debugs
* Generating TAC report
* High Availability (HA)
* IPS
* Routing
* Switch / Switch Controller
* VPN

Note: Still fairly new to Ansible, so this may not be (is probably not) written in the optimal manner. :)
## Before use:
* Edit the FortiGates inventory file as necessary.
* Modify the email variables at the top of the playbook.

## To use:
Simply call the Ansible playbook as necessary. Some playbooks have extra parameters that can be configured (especially the Flow Debug playbook).

### cli_cpu_memory.yml extra variables
* top_iterations - The number of times to run diagnose sys top (default is 15 times).
* top_delay - How often to run diagnose system top (default is 2 seconds).

### cli_flow_debug.yml extra variables
* debug_length - How long to run the flow debug for (default is 60 seconds).
* flow_count - How many packets to capture (default is 100 packets).
* iprope - Enables extra detail to flow debug. True/False input (default is False).
* function_name - Enables extra detail to flow debug. True/False input (default is False).
* vdom - Index of the VDOM to filter packets on. Integer input. Get the VDOM index from running "diagnose sys vd list" in global context.
* negate_vdom - Negates the VDOM field, so the filter ignores packets from that VDOM. True/False input (default is False).
* protocol - Protocol type to filter packets with. Integer input. 6 = TCP, 17 = UDP.
* negate_protocol - Negates the protocol field, so the filter ignores packets of that protocol type. True/False input (default is False).
* address - IP address to filter packets with (either in source or destination.
* negate_address - Negates the address field, so the filter ignores packets with that IP address. True/False input (default is False).
* src_address - Source IP address to filter packets with.
* negate_src_address - Negates the src_address field, so the filter ignores packets with that source IP address. True/False input (default is False).
* dst_address - Destination IP address to filter packets with.
* negate_dst_address - Negates the dst_address field, so the filter ignores packets with that destination IP address. True/False input (default is False).
* port - Port to filter packets with (either in source or destination).
* negate_port - Negates the port field, so the filter ignores packets with that port. True/False input (default is False).
* src_port - Source port to filter packets with.
* negate_src_port - Negates the src_port field, so the filter ignores packets with that source port. True/False input (default is False).
* dst_port - Destination port to filter packets with.
* negate_dst_port - Negates the dst_port field, so the filter ignores packets with that destination port. True/False input (default is False).

### cli_ha.yml extra variables
* top_iterations - The number of times to run diagnose sys top (default is 6 times).
* top_delay - How often to run diagnose system top (default is 5 seconds).
* app_length - How long to capture the debug output of the HA Talk and HA Sync processes.

## Current issues:
1. Uses password logon to the FortiGates. Need to resolve to use certificates / store the password in Ansible Vault.
2. Passwords for logon are currently stored in the inventory. This is in cleartext (BAD!)

## References
These playbooks are heavily inspired by: 
* https://ansible-galaxy-fortios-docs.readthedocs.io/en/latest/faq.html#how-to-work-with-raw-fotios-cli
* https://blog.boll.ch/wp-content/uploads/2020/12/CheatSheet-FortiOS-6.4-v1.1.pdf
