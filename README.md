Ansible Automation Platform Daily Demo for Linux
=========
A demo designed to showcase many of the use cases that people are looking for.  We are using the workflow visualizer to show how the various building blocks are put together and enable the delivery on demand of a custom website.  The playbooks call roles, the roles allow for ease of sharing the code and also allow for documentation of the various things needed in each role. The demo is designed to be integrated with an IT Service Management (ITSM) system.  Everything will be documented in ITSM system via the skillfull use of automation.  Check out the video below to see that "the art of the possible."

# The workflow

![alt text](https://github.com/ericcames/aap.dailydemo.linux/blob/main/images/DDLWF1.png "Start of workflow")
![alt text](https://github.com/ericcames/aap.dailydemo.linux/blob/main/images/DDLWF2.png "Middle of Workflow")
![alt text](https://github.com/ericcames/aap.dailydemo.linux/blob/main/images/DDLWF3.png "End of Workflow")

**The playbooks**

[1. Create our network container](https://github.com/ericcames/aap.dailydemo.linux/blob/main/playbooks/create_vpc_01.yml "create_vpc_01.yml") <br>
[2. Create our virtual machine](https://github.com/ericcames/aap.dailydemo.linux/blob/main/playbooks/create_instance_02.yml "create_instance_02.yml")<br>
[3. Update our inventory](https://github.com/ericcames/aap.dailydemo.linux/blob/main/playbooks/add_inventory_03.yml "add_inventory_03.yml")<br>

Ansible Controller Credential<br>
Input configuration
```
fields:
  - id: url
    type: string
    label: Controller URL
  - id: user
    type: string
    label: Controller Username
  - id: password
    type: string
    label: Controller Password
    secret: true
required:
  - url
  - user
  - password
```
Injector configuration
```
extra_vars:
  controller_url: '{{url}}'
  controller_user: '{{user}}'
  controller_passwd: '{{password}}'
```
[4. Gather instance information](https://github.com/ericcames/aap.dailydemo.linux/blob/main/playbooks/get_instance_info_04.yml "get_instance_info_04.yml")<br>
[5. Red Hat Registration](https://github.com/ericcames/aap.dailydemo.linux/blob/main/playbooks/redhat_subscription_manager_05.yml "redhat_subscription_manager_05.yml")<br>

Red Hat Customer Portal Credential<br>
Input configuration
```
fields:
  - id: user
    type: string
    label: Red Hat Customer Portal Username (access.redhat.com)
  - id: password
    type: string
    label: Red Hat Customer Portal Password
    secret: true
required:
  - user
  - password
```
Injector configuration
```
extra_vars:
  customer_portal_password: '{{password}}'
  customer_portal_username: '{{user}}'
```

[6. Configuration Management](https://github.com/ericcames/aap.dailydemo.linux/blob/main/playbooks/post_install_06.yml "post_install_06.yml")<br>
[7. User access](https://github.com/ericcames/aap.dailydemo.linux/blob/main/playbooks/provision_user_access_07.yml "provision_user_access_07.yml")<br>
[8. Application install](https://github.com/ericcames/aap.dailydemo.linux/blob/main/playbooks/lamp_setup_08.yml "lamp_setup_08.yml")<br>
[9. Website deployment](https://github.com/ericcames/aap.dailydemo.linux/blob/main/playbooks/website_deployment_09.yml "website_deployment_09.yml")<br>
[10. Reporting](https://github.com/ericcames/aap.dailydemo.linux/blob/main/playbooks/reporting.yml "reporting.yml")<br>
[11. Send notification that the website is ready](https://github.com/ericcames/aap.dailydemo.linux/blob/main/playbooks/sendmail_10.yml "sendmail_10.yml")<br>

Custom Mail Server credential<br>
Input configuration
```
fields:
  - id: smtp_server
    type: string
    label: Mail Server
  - id: smtp_port
    type: string
    label: Mail Server Port
  - id: smtp_username
    type: string
    label: Mail Server Username
  - id: smtp_password
    type: string
    label: Mail Server Password
    secret: true
required:
  - smtp_server
  - smtp_port
  - smtp_username
  - smtp_password
```
Injector configuration
```
extra_vars:
  MAILHOST: '{{smtp_server}}'
  MAILHOST_PORT: '{{smtp_port}}'
  MAILHOST_PASSWORD: '{{smtp_password}}'
  MAILHOST_USERNAME: '{{smtp_username}}'
```
[Site Delete will clean everything up](https://github.com/ericcames/aap.dailydemo.linux/blob/main/playbooks/site_delete.yml "site_delete.yml")<br>

ServiceNow
========

**The playbooks**

[Create a CMDB record](https://github.com/ericcames/aap.dailydemo.linux/blob/main/playbooks/servicenow/create_ci.yml "create_ci.yml") <br>
[Create a CMDB relationship](https://github.com/ericcames/aap.dailydemo.linux/blob/main/playbooks/servicenow/create_cmdb_relationship.yml "create_cmdb_relationship.yml") <br>

![alt text](https://github.com/ericcames/aap.dailydemo.linux/blob/main/images/DDLWF2.png "Middle of Workflow")

[Create incident ticket](https://github.com/ericcames/aap.dailydemo.linux/blob/main/playbooks/servicenow/incident_create.yml "incident_create.yml") <br>

![alt text](https://github.com/ericcames/aap.dailydemo.linux/blob/main/images/DDLWF1.png "Start of workflow")

[Update requested item ticket](https://github.com/ericcames/aap.dailydemo.linux/blob/main/playbooks/servicenow/update_sn_req_itm.yml "update_sn_req_itm.yml") <br>

ServiceNow credential<br>
Input configuration
```
fields:
  - id: instance
    type: string
    label: Instance
  - id: username
    type: string
    label: username
  - id: password
    type: string
    label: password
    secret: true
required:
  - instance
  - username
  - password
```
Injector configuration
```
env:
  SN_HOST: '{{instance}}'
  SN_PASSWORD: '{{password}}'
  SN_USERNAME: '{{username}}'
```
- ServiceNow Ansible spoke setup

[Ansible spoke setup - Alex Dworjan](https://github.com/shadowman-lab/Ansible-SNOW/tree/master/SNOWSetup#servicenowaap-integration-instructions-using-ansible-spoke "Ansible spoke setup - Alex") <br>
[Ansible spoke youtube - Alex Dworjan](https://www.youtube.com/watch?v=DmPXiRHjgRY "Ansible spoke youtube - Alex Dworjan") <br>

- ServiceNow Ansible spoke setup additional Ansible controllers
```
Flow Designer -> Connections -> Add Connection

Connection Name: ericamesAAPalias
Connection URL: https://ericames.ddns.net
Credential Name: Eric Ames AAP Spoke Credentials
Application Registry Name: Eric Ames Spoke Registry
OAuth Client ID: %SECRETID%
OAuth Client Secret: %SECRETGOESHERE%
Oauth Entity Profile Name: Eric Ames Spoke Registry default_profile
OAuth Entity Scope: write
Authorization URL: https://ericames.ddns.net/api/o/authorize/
Token URL: https://ericames.ddns.net/api/o/token/
OAuth Redirect URL: https://ven05433.service-now.com/api/sn_ansible_spoke/ansible_oauth_redirect

```
- Automated incident management example

[Example Error Handling in support of incident enrichment](https://github.com/ericcames/aap.dailydemo.linux/blob/main/roles/instance_create_aws/tasks/main.yml "Example Error Handling") <br>

```
- name: Adding incident management error handling
  block:

    PUT YOUR TASKS HERE

  rescue:

    - name: Capture the error message
      register: my_error
      ansible.builtin.set_stats:
        data:
          my_error: "{{ ansible_failed_result.msg }}"

    - name: Capture the Job ID
      register: my_job_id
      ansible.builtin.set_stats:
        data:
          my_job_id: "{{ tower_job_id }}"

    - name: Capture the Job Template name
      register: my_job_template_name
      ansible.builtin.set_stats:
        data:
          my_job_template_name: "{{ tower_job_template_name }}"

    - name: Fail the job even though the rescue worked
      ansible.builtin.fail:
        msg: failing so we create the incident ticket
```

# The website

![alt text](https://github.com/ericcames/aap.dailydemo.linux/blob/main/images/Web01.png "Webtop")
![alt text](https://github.com/ericcames/aap.dailydemo.linux/blob/main/images/Web02.png "Webbottom")

- Patching Report

![alt text](https://github.com/ericcames/aap.dailydemo.linux/blob/main/images/Patchingreport.png "zjleblanc.reporting collection")

# A youtube video of the demo

- [AAP Daily Demo Linux](https://youtu.be/Wvf7-CZmAHI?si=oxqDRAWsfqpXTnD- "AAP Daily Demo Linux")

# Cockpit the Administrators GUI

![alt text](https://github.com/ericcames/aap.dailydemo.linux/blob/main/images/cockpit.png "cockpit")

# The command line; there's no place like home :-)

![alt text](https://github.com/ericcames/aap.dailydemo.linux/blob/main/images/cli.png "The command line")

Looking for other Daily Demos?
=========

- [AAP Daily Demo Windows](https://github.com/ericcames/aap.dailydemo.windows "AAP Daily Demo Windows")
- [AAP Daily Demo Linux](https://github.com/ericcames/aap.dailydemo.linux "AAP Daily Demo Linux")
- [AAP Daily Demo F5](https://github.com/ericcames/aap.dailydemo.F5 "AAP Daily Demo F5")
- [AAP Daily Demo Panos](https://github.com/ericcames/aap.dailydemo.Panos "AAP Daily Demo Panos")
- [AAP Daily Demo Satellite](https://github.com/ericcames/aap.dailydemo.satellite "AAP Daily Demo Satellite")
