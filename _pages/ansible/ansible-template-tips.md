## Managing Templated Files

Include comments in Ansible Deployed Templates.

Use the “Ansible managed” string set in the ansible_managed directive. The `ansible_managed` directive is set in the `ansible.cfg` file:

`ansible_managed = Ansible managed`

To include the ansible_managed string inside a Jinja2 template, use the following syntax:

`{{ ansible_managed }}`