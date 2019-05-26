## SABnzbd as a docker service

Repo that dockerizes the SABnzbd application.

### Commands

docker-compose up -d

### Override defaults

1. Copy the `.env` template

    cp -r .env.example .env

1. Change values on the `.env` file

### Systemd 

There is a `systemd` service file available to symlink so you can start the docker container as a service; more specifically a startup service.

Example usage would be:

    # Create the symlink (or copy it, whatever)
    ln -s $REPODIR/systemd/sabnzbd.service /etc/systemd/system/sabnzbd.service

    # Start the service
    sudo systemctl start sabnzbd.service

    # Stop the service
    sudo systemctl stop sabnzbd.service

    # Enable the service on startup
    sudo systemctl enable sabnzbd.service

    # Watch the logs
    sudo journalctl -b -f -u sabnzbd.service

