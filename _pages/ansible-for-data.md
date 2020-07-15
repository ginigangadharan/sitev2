---
layout: post
title: Formatting Data in Ansible
author: gini
categories: [ ansible ]
image: "assets/images/2020/photo-1456428746267-a1756408f782-@clintadair.jpg"
tags: [network lab, network automation, ansible network automation]
permalink: ansible-for-data
featured: false
hidden: false
---

# Process JSON Data

```
  tasks: 
    - name: Display the JSON file and collect data
      shell: cat {{ cve_json_file }}
      register: result
 
    - name: Save the Json data to a Variable
      set_fact:
         jsondata: "{{ result.stdout | from_json }}"

    - name: setDomainName
      set_fact:
          domain_name: "{{ jsondata | json_query(jmesquery) }}"
      vars:
          jmesquery: 'domain.name'

    - name: Server Names
      set_fact:  
        servernames:  "{{ jsondata | json_query(jmesquery) }}"
      vars:
        jmesquery: '*.servers[*].name'
```

Ref:
- [Ansible read JSON file – JSON file Parsing](https://www.middlewareinventory.com/blog/ansible-playbook-read-json-file/)
- [JSON Query Filter](https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters.html#json-query-filter)