

## Servidor Apache
~~~
 asir_apache:
    image: httpd:latest
    networks:
       apanet:
        ipv4_address: 192.168.22.4
    ports:
      - 8081:80
    volumes:
      - apache-data:/usr/local/apache2/htdocs
~~~
* Puerto:8081:80.
* Ponemos un volumen, apache data.
* Asigranción de la ip de la red: 192.168.22.4


~~~
asir_bind9:
    image: internetsystemsconsortium/bind9:9.16
    networks:
       apanet:
        ipv4_address: 192.168.22.5
    ports:
      - 53:53
    volumes:
      - conf:/etc/bind
      - options:/var/cache/bind
      - secondaryzones:/var/lib/bind
      - logfiles:/var/log
~~~
* El puerto es el 53:53.
* Ponemos como volumen bind.
* Asignamos la ip 192.168.22.5
~~~

## Cliente

 asir_cliente:
    image: kasmweb/desktop:1.10.0-rolling
    networks:
      - apanet
    dns:
      - 192.168.22.5
    ports:
      - 6901:6901
    stdin_open: true  # docker run -i
    tty: true         # docker run -t
     environment:
    - VNC_PASSWORD=password 

~~~
* Su puerto será el 6901:6901.
* Asignamos una contraseña que será 'password' en string.
~~~
## Red

networks:
  apanet:
    external: true

~~~
## Volumenes

    volumes:
    apache-data:
    external: true
     conf:
      external: true
        options:
      secondaryzones:
       logfiles:
~~~
wireshark:
    image: lscr.io/linuxserver/wireshark
    container_name: wireshark
    cap_add:
      - NET_ADMIN
    network_mode: host
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
    volumes:
      - /path/to/config:/config
    ports:
      - 3000:3000 #optional
    restart: unless-stopped

