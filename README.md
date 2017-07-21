ec2_ebs_mounting
-------

[![Build Status](https://travis-ci.org/mikhailadvani/ec2_ebs_mounting.svg?branch=master)](https://travis-ci.org/mikhailadvani/ec2_ebs_mounting) [![Galaxy](https://img.shields.io/badge/ansible--galaxy-mikhailadvani.ec2_ebs_mounting-blue.svg)](https://galaxy.ansible.com/mikhailadvani/ec2_ebs_mounting)


Ansible role to mount EBS block devices as data or swap volumes

### Required variables

Setup the following vars and include them in the playbook before execution

    instance:
      volumes:                                                                  
        - EBSSpecifications:                                                    
            volume_size: 8                                                      #The size of the volume
            volume_type: gp2                                                    #The type of the volume
            device_name: /dev/sda1                                              #The device id of the volume
            delete_on_termination: yes                                          #Whether the volume needs to be retained after instance termination
          MountSpecifications:
            mountpoint: /                                                       #Root volume            
        - EBSSpecifications:
            volume_size: 2
            volume_type: gp2
            device_name: /dev/xvdf
            delete_on_termination: yes
          MountSpecifications:
            mountpoint: swap                                                    #To be mounted as a swap volume
        - EBSSpecifications:
            volume_size: 20
            volume_type: gp2
            device_name: /dev/xvdg
            delete_on_termination: yes
          MountSpecifications:
            volume_label: "postgresdata"
            mountpoint: /var/lib/pgsql                                          #Data volume                                  
            owner:                                                              #Owner of data volume
              name: postgres
              uid: 2000
              home_dir: /home/postgres
    
- Root volume is identified by `mountpoint` value being `/`
- Swap volume is identified by `mountpoint` value being `swap`
- Data volume is identified by `mountpoint` value not being `/` or `swap`
- Data volumes can have an optional `owner` defined
    - If owner is defined, one of `name` is mandatory
    - The owner will be created if doesn't exist already
    - `uid` and `home_dir` are optional

### Example Playbook

    - hosts: localhost
      roles:
         - { role: mikhailadvani.ec2_ebs_mounting }

License
-------

Apache    
