eucalyptus-configure
====================

Configure all the services in order to have all user services ready and working.

** *WARNING !!!* **
*This role has to be used in the deployment of Eucalyptus cloud !*

Requirements
------------

hosts:

- clc | Cloud Controller

- ufs | User Facing Services

- cc | Cluster Controllers

- sc | Storage Controllers

- nc | Node Controllers

- walrus | S3 builtin-backend

Role Variables
--------------

| Name | Default | Description | Note
|--- |--- |--- |---
| networking_mode | None | Eucalyptus networking mode | Must be the same for all roles
| osg_provider_name | Walrus | S3 backend services. Values: walrus,riakcs,s3 | Need the backend-system to be setup
| osg_properties | [{}] (list of objects) | List of key-value sets describing the backend storage | Back-End system already existing
| sc_clusters | [{}] | List of sets to configure the backend for EBS services | See examples for SAN  
| edge_network_config_file | /tmp/network.json | Path the JSON configuration for EDGE networking | None
| euca_admin_creds_path | /var/tmp/creds | Default folder where will be extracted the Eucalyptus admin credentials | None
| euca_admin_zip_file | /var/tmp/admin.zip | Default ZIP file location with the Eucalyptus admin credentials | None

Dependencies
------------

- JohnPreston.eucalyptus-register

- JohnPreston.eucalyptus-start

Example Playbook
----------------

```

- hosts: clc
  roles: 
  - JohnPreston.eucalyptus-start
  - JohnPreston.eucalyptus-configure
  vars:
  osg_provider: riakcs
  osg_properties:
  - {'name': 'objectstorage.s3provider.s3endpoint', 'value': 'http://RIACKS-URL'}
  - {'name': 'objectstorage.s3provider.s3accesskey', 'value': 'riak_akey'}
  - {'name': 'objectstorage.s3provider.s3secretkey', 'value': 'riak_skey'}
  sc_clusters:
  - {'partition': 'az-01', 'mode': 'das', 'properties': [ {'name': 'dasdevice', 'value': 'vg01'}]}
  - {'partition': 'az-02', 'mode': 'netapp', 'properties': [ {'name':'sanhost', 'value': '1.2.3.4'}, {'name': 'sanuser', 'value': 'root'}, {'name': 'sanpassword', 'value': 'N3t4pp'} ]}
  euca_admin_zip_file: /root/euca-admin.zip

```

License
-------

Apache

Author Information
------------------

John Preston [John Mille]
