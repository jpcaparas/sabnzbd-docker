## SABnzbd as a docker service

Repo that dockerizes the [SABnzbd](https://sabnzbd.org/) application.

### Commands

    docker-compose up -d

### Override defaults

1. Copy the `.env` template  
    
    ```
    cp -r .env.example .env
    ```
    
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

### OpenVPN conflict

Networking issues usually arise when trying to run `docker-compose up` with OpenVPN running, because both will be fighting for the same network.

In order to get past that, append the below lines to the `docker-compose.yml` or, alternatively, create a `docker-compose-override.conf` file:

    networks:
        default:
            external:
            name: localdev

Now run this:

    docker network create localdev --subnet 10.0.1.0/24
    
If you want the network to be created every time the systemd service runs/shuts down, add the following items on the `[Service]` section:

    # Create temp network before running docker-compose
    ExecStartPre=/usr/bin/docker network create localdev --subnet 10.0.1.0/24
    
    # Delete temp network after running docker-compose
    ExecStopPost=/usr/bin/docker network rm localdev

