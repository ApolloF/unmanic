# Unmanic development

The development environment can be configured in 2 ways:

1. Using Docker

2. As a Pip develop installation


Depending on what you are trying to develop, one way may work better than the other.

Regardless of the method you use, you will need to pull in the frontend component and build it.



## Dev env

### Option 1: Docker

Docker is by far the simplest way to develop. You can either pull the latest Docker image, or build
the docker image by following the [Docker documentation](../docker/README.md)

Once you have a Docker image, you can run it using the scripts in the `../devops/` directory.

Examples:
```
# Enable VAAPI
devops/run_docker.sh --debug --hw=vaapi --cpus=1

# Enable NVIDIA
devops/run_docker.sh --debug --hw=nvidia --cpus=1

# Standard dev env
devops/run_docker.sh --debug
```

The following folders are generated in the Docker environment:

  - `/dev_environment/config` - Contains the containers mutable config data
  - `/dev_environment/library` - A library in which media files can be placed for testing
  - `/dev_environment/cache` - The temporary location used by ffmpeg for converting file formats

### Option 2: Pip

You can also just install the module natively in your home directory in "develop" mode.

Start by creating a venv.
```
python3 -m venv venv
echo 'export HOME_DIR=$(readlink -e ${VIRTUAL_ENV}/../)/dev_environment' >> ./venv/bin/activate
source ./venv/bin/activate
```

Then install the dependencies into that venv
```
python3 -m pip install --upgrade pip
python3 -m pip install --upgrade -r ./requirements.txt -r ./requirements-dev.txt
```

Then install the module:

```
python3 ./setup.py develop
```

This creates an egg symlink to the project directory for development.

To later uninstall the development symlink:

```
python3 ./setup.py develop --uninstall
```

You should now be able to run unmanic from the commandline:
```
# In develop mode this should return "UNKNOWN"
unmanic --version
```



## Building the Frontend

The Unmanic frontend UI exists in a submodule.

Start by pulling the latest changes

```
git submodule update --init --recursive 
```

Once you have done this, run the frontend_install.sh script.

```
devops/frontend_install.sh
```

This will install the NPM modules and build the frontend package. The end result will be located in `unmanic/webserver/public`



## Database upgrades

This project uses Peewee migrations for managing the sqlite database.
`devops/migrations.sh` provides a small wrapper for the cli tool. To get started, run:
```
devops/migrations.sh --help
```
