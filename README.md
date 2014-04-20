Usage
----------

To configure this image, create a `config` directory, with a `downloadurl` file inside it.  You will need to place the **direct download link** to a TeamSpeak 3 `.tar.gz` file.  Note that because TeamSpeak is proprietary software, I can't distribute it in the Docker container directly.

This `config` directory will also be used to store the TeamSpeak database, so ensure that it is kept in a permanent location.

To run this image:

    /usr/bin/docker run -p 9987:9987/udp -p 10011:10011 -p 30033:30033 -p 41144:41144 -v /path/to/config:/config --name=teamspeak hachque/teamspeak

What do these parameters do?

    -p 9987:9987/udp = forward the host's 9987 UDP port to TeamSpeak (for voice)
    -p 10011:10011 = forward the host's 10011 port to TeamSpeak (for query)
    -p 30033:30033 = forward the host's 10011 port to TeamSpeak (for file manager)
    -p 41144:41144 = forward the host's 10011 port to TeamSpeak (for TSDNS)
    -v /path/to/config:/config = map the storage directory for TeamSpeak
    --name teamspeak = the name of the container
    hachque/teamspeak = the name of the image

This image is intended to be used in such a way that a new container is created each time it is started, instead of starting and stopping a pre-existing container from this image.  You should configure your service startup so that the container is stopped and removed each time.  A systemd configuration file may look like:

    [Unit]
    Description=TeamSpeak
    Requires=docker.service
    
    [Service]
    ExecStart=<command to start instance, see above>
    ExecStop=/usr/bin/docker stop teamspeak
    ExecStop=/usr/bin/docker rm teamspeak
    Restart=always
    RestartSec=5s
    
    [Install]
    WantedBy=multi-user.target

SSH / Login
--------------

**Username:** root

**Password:** linux

