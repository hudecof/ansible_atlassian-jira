Role Name
=========

Role tp help install or update the Atlassian Jira.

Requirements
------------

For running Atlassian Jira you need a little bit more

- java
- database
- init script

*java* is out of scope for this playbook. I can't force you to install 
any version of java on your server. Use any existing java roles to do that.
I have my own role `hudecof.java` to do that.

You could preffer another  *database* as me. So this is out of scope too.  

The *tar.gz* version do not have startup script. I use `supervisord` to do this job.
I will generate template for `supervisord` and `init.d` and put it into *instalation dorectory* directory.

If you are updating, shutdown you old instance manually. This role do not handle this!.
It will just setup your new instance with your customizations.

Role Variables
--------------

`atlassian_jira_version` is the verion you want to install. Thi sis the only one varialble you need to change, the others are optional.

`atlassian_jira_type` is small hack to support *jira-software* and *jira-core*. If you are instlalling jira before version *7.0.0*, leave this variable as is. For version *7.0.0* and above, choose `core`  or `software`.
 
`atlassian_jira_baseurl` is the URL where you can find the *tar.gz* files. If you have your own mirror, change it.

`atlassian_jira_basedir` is path where to download nad extract the *tar.gz* file, defaults to `/opt/atlassian`.

`atlassian_jira_home` is the `jira.home`, aka you data directory.

`atlassian_jira_user`, `atlassian_jira_uid`, `atlassian_jira_group`, `atlassian_jira_gid` are variables to setup dedicated user to run the instance 

`atlassian_jira_server_xml` is list of changes to `server.xml` It ueses XPath to edit/add/remoce exiting properties.

    atlassian_jira_server_xml:
    - xpath: /Server/Service/Connector
      ensure: present
      attribute: proxyPort
      value: 443
    - xpath: /Server/Service/Connector
      ensure: present
      attribute: scheme
      value: https

`atlassian_jira_jvm_opts` is the list of custom *JVM_OPTS* properties. At this moment you can't change the existing one ;(

For *CrowdSSO* see `CrowdSSO.md`

Dependencies
------------

This role depens on the `cmprescott.xml` role/library.

Example Playbook
----------------

    - hosts: atlassian
      roles:
         - cmprescott.xml
         - hudecof.atlassian-jira

License
-------

BSD

Author Information
------------------

Peter Hudec
CNC, a.s.
