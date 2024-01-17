# python

## pyenv

- `brew install pyenv`
- list available versions `pyenv install --list`
- install specific version(s) `pyenv install <version>` e.g. `pyenv install 3.10.13`
- set global version `pyenv global <version>` e.g. `pyenv global 3.10.13`
- install pyenv virtual env `brew install pyenv-virtualenv`
- add this to your `.zshr`:
```shell
eval "$(pyenv init --path)"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

## virtual environment

- create virtual environment `pyenv virtualenv <version> <env_name>` e.g. `pyenv virtualenv 3.10.13 test-project`
- virtual environment is created in `~/.pyenv/versions/<version>/envs/<env_name>`
- activate virtual environment `pyenv activate <env_name>` e.g. `pyenv activate test-project`

