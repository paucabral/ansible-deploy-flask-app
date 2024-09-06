# Ansible Playbook for Flask App Deployments

Ansible Playbook for deploying a Flask web applications with database.

## App Criteria
1. App is monolithic deployed in either single server configuration (both Flask App and DB Server in a single node) or two tier (Flask App and DB Server on separate nodes).
2. Uses `app.py` as main app entry point.
3. App uses either SQLite or PostgreSQL as the target databse.

## Prerequsites
1. Ansible installed on the control node *(Note: control node requires GNU/Linux or Unix based operating system, e.g. Linux, MacOS)*.
2. SSH Pass package installed on the control node if using SSH username and password in reaching managed nodes.
3. Ansible Collections: Install using the following commands.
    ```
    $ ansible-galaxy collection install community.postgresql
    $ ansible-galaxy collection install ansible.posix
    ```

## Usage
1. Load the necessary environment variables (as needed) for authentication (if using SSH username and password). *You may refer to the contents of `.env.sample`.*
2. Setup the `inventory.ini` file using `inventory.ini.sample` as the base. Host groupings are set to `db` and `app` as reflected in the plays that can be viewed in `playbook.yaml`.
3. Create a `config.yaml` file using `config.yaml.sample` as the base. Modify the configurations and `playbook.yaml` according to preference.
4. Run the playbook.
    ```bash
    $ ansible-playbook playbook.yaml
    ```

## Tested Environment
This playbook is developed primarily using **Centos 9 Stream** as the targe environment. Specifically using [bento/centos-stream-9 Vagrant box](https://app.vagrantup.com/bento/boxes/centos-stream-9). As such, roles are expected (but **not guaranteed**) to work on `RedHat` OS Family.

*Checkout this [automation demo for 2-tier deployment in Virtualbox](https://github.com/paucabral/local-iac-virtualbox-demo/tree/2-tier-sampler) to automate the setup of test environment. For the demo application used, checkout this repository for the [demo Flask app](https://github.com/paucabral/flask-fullstack-crud-with-auth).*

## Known Bugs
1. Error on `TASK [flask-web-server : Run Flask DB commands (init, migrate, upgrade)]`.

    Problem on reading environment variable used for database URI (environment variables are loaded system-wide on task `TASK [flask-web-server : Set environment variables]`). Checking if VM performance issue. Will provide updates on the role.

    *Temporary Resolution: Re-run the playbook a couple of times within minute intervals until it runs successfully.*