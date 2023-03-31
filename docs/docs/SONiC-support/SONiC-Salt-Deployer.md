# SONiC Salt deployer

This tool deploys and configures Salt-minions on SONiC devices.

It includes:

* DNS server configuration
* Salt minion PEX
* Update grains script
* Systemd services / timers

You can run it regularly. There will be no impact on already deployed devices. Only the needed changes will be made.

# Prepare your environment

```
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements/base.txt
```

# How to use it

## Settings

See [settings.env](https://github.com/criteo/sonic-salt-deployer/blob/main/settings.env).

There is also an example provided [here](https://github.com/criteo/sonic-salt-deployer/blob/main/settings_example.env).

## Usage

```
pip install -r requirements/base.txt
python ./start.py
```

Or build the PEX via `tox -e bundle` and run the executable.

You can use this systemd [service](https://github.com/criteo/sonic-salt-deployer/blob/main/systemd/sonic-salt-deployer.service) and its [timer](https://github.com/criteo/sonic-salt-deployer/blob/main/systemd/sonic-salt-deployer.timer).
