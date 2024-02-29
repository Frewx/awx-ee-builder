# awx-ee-builder
This project is using ansible-builder to build AWX execution environments

If the default [execution environment](https://quay.io/repository/ansible/awx-ee) is not enough for you, you can add some of the system,python or galaxy packages to your execution environments.

## Prerequisites

* Python3 (for ansible-builder v3 you will need python3.9)
* Docker
* Ansible Builder

After installing docker and python3, you can install ansible-builder with:
````
python3 -m pip install ansible-builder
````

Based on your python3 version, ansible-builder v1 or v3 will be installed. If you have python3.9 or upper ansible-builder v3 will be installed and if you have lower python3 version, ansible-builder v1 will be installed.

## Usage

After you are done with the prerequisites, you can begin to work with the customization. This repository contains folders/files that can help you customize your image. 
Under builder/dependencies folder there are 3 different files with the following purpose:
- **bindep.txt:** Is used for RPM packages that needs to be installed on the image
- **requirements.txt:** Is used for python packages/libraries that needs to be installed on the image
- **requirements.yml:** Is used for ansible galaxy modules that needs to be installed on the image

After modifying requirements files, you need to modify your main file which is **execution-environment.yml**.

This repository includes examples for both ansible-builder v1 and v3, respectively
* execution-environment_v1.yml
* execution-environment_v3.yml

**NOTE:**
*You can use and edit these files based on your ansible-builder version, and change it's name to **execution-environment.yml***

After you modified the above files based on your need, you can create your EE with below with:

```
ansible-builder build --tag <your_registry_name>/awx/ee:<your_base_ee_version>-custom --container-runtime docker --verbosity 3
```

* When tagging the image, make sure that you are tagging with the value of your public/private image registry. (e.g docker.io)
* Make sure to tag your image with base image version to know about the base image

To use your image, you need to push the image to an image registry. After that, you can create an execution environment from AWX, get the image from registry to AWX.

```
docker push <your_registry_name>/awx/ee:<your_base_ee_version>-custom
```

**NOTE:** If your registry needs authentication, you need to use *docker login* first. 

**NOTE:** There is caching when creating an image, if your build fails, cached steps can be used again. But if you don't want to use that cache, you can remove it with clearing the folder **context**, in your folder structure. 