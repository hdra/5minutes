# 5Minutes

Base server setup. Forked from [this](https://github.com/chhantyal/5minutes) which is based on [this](https://plusbryan.com/my-first-5-minutes-on-a-server-or-essential-security-for-linux-servers) with some changes.

## Install

Assuming the server is created with `root` ssh access.

On the local machine with `pipenv` installed, run `pipenv install`

## Usage

1. Clone the repo

2. Change the `hosts` file with the server information

3. Configure the variables in `vars.yml`, or override it when running:

```
pipenv run ansible-playbook 5minutes.yml -u <ssh_username> \
  --extra-vars "server_user_name=<username> server_user_password=$(pipenv run python generate_password.py)"
```

* Run `pipenv run python generate_password.py |> pbcopy` to generate a password to be copied into `vars.yml`

### What it does:

It setups:

- Connects to server using SSH
- Updates APT cache
- Performs APT upgrade
- Adds user specified in variable `server_user_name` which has sudo permission
- Adds specified public key in variable `user_public_keys` in ssh authorized_keys.
- Disables root SSH access. Yes, from next time you need to use new user to access server.
- Disables password authentication. Again you will need to use new user with SSH public key auth method.
- Installs `ufw` as firewall, `fail2ban` to ban IPs that show malicious signs, `logwatch` to analyze and report logs.
- It also installs `unattended-upgrades` to enable automatic security updates.


### Variables

Configurable params in the `vars.yml`

- `server_user_name`: default `username`
- `server_user_password`: Can use the `generate_password.py` script.
- `logwatch_email`: default `devops@example.com`, you won't get report email from `logwatch` if you don't change.
- `user_public_keys`: default `~/.ssh/id_rsa.pub`, if you use different key pair name, you need to change this path
 to public key file.

Specify the the python interpreter path on each server in the `hosts` file
