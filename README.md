# ovpn-client-and-rutorrent-docker

docker-compose.yml file for both an openvpn client and an rutorrent client with web interface

## VPN
The simple way of using the vpn is having an .ovpn file and putting it in the ./vpn volume. (so in the working directory, Ex. $PWD/vpn/example.ovpn) All ports that need to be local accessable need to be mapped on the vpn container, and not on the service that uses those ports. For other uses as to connect to a remote network using the ovpn-client, look at the documentation referenced below. But it comes down to using docker run ... -v 'server;user;password[;port]' to connect to a remote network.  

#### Volumes
* /vpn: ovpn file location

## Rutorrent
The torrent client will send all trafic through the vpn, meaning that the vpn is the only container with access to the locale network. If the ports below are mapped on the vpn then they will be locally accessable, the port mapped on 8080 will then be the port used to connect to the torrent web interface. (Ex. mapping ports: - '9999:8080' on the vpn will allow you to use the web interface on http://localhost:9999)  
All downloads will go to the /downloads folder on the torrent client so make sure to change the path in volumes if needed.

#### Volumes
* /data: rTorrent / ruTorrent config, session files, log, ...
* /downloads: Downloaded files
* /passwd: Contains htpasswd files for basic auth

> ⚠️ warning Note that the volumes should be owned by the user/group with the specified PUID and PGID. If you don't give the volumes correct permissions, 
> the container may not start.

#### Ports
* 6881 (or RT_DHT_PORT): DHT UDP port (dht.port.set)
* 8000 (or XMLRPC_PORT): XMLRPC port through nginx over SCGI socket
* 8080 (or RUTORRENT_PORT): ruTorrent HTTP port
* 9000 (or WEBDAV_PORT): WebDAV port on completed downloads
* 50000 (or RT_INC_PORT): Incoming connections (network.port_range.set)

> ⚠️ warning Port p+1 defined for XMLRPC_PORT, RUTORRENT_PORT and WEBDAV_PORT will also be reserved for healthcheck. 
> (e.g. if you define RUTORRENT_PORT=8080, port 8081 will be reserved)


## references
#### dperson/openvpn-client:  
https://hub.docker.com/r/dperson/openvpn-client  
https://github.com/dperson/openvpn-client  
https://github.com/dperson/openvpn-client/blob/master/docker-compose.yml  

#### crazymax/rtorrent-rutorrent:  
https://hub.docker.com/r/crazymax/rtorrent-rutorrent  
https://github.com/crazy-max/docker-rtorrent-rutorrent  
