# What is this?

This is a simple stack to provide a simple upload/download interface using a web based file browser and a ftp server.

By default (in)secure downloads/uploads are enabled.


# What is used?
* [File Browser 2.16.1](https://github.com/filebrowser/filebrowser)
  * [Docker image](https://hub.docker.com/r/filebrowser/filebrowser)
* [SFTP](https://github.com/atmoz/sftp)
  * [Docker image (Alpine)](https://hub.docker.com/r/atmoz/sftp/)
* [NGinx](https://www.nginx.com/)
  * [Docker image](https://hub.docker.com/_/nginx)

# How to setup
## Ports
Make sure that all used ports are open, by default SFTP will use 22 and NGinx will use 80 and 443.

## SFTP
If your server has an application using port 22, you can change SFTP port at **docker-compose.yml**
### User with password
Create users following the below pattern at **sftp/users.conf**:

    <user>:<password>:<id>:<groupd>:<home>

### User with key
To log in with an ssh key, your user should look like:

    <user>::<password>:<groupd>:<home>
Remember: you should mount your key at /home/\<user>/.ssh/keys/

    -v $PWD/id_rsa.pub:/home/<user>/.ssh/keys/id_rsa.pub:ro \

Create your keys at **sftp/ssh/**

    ssh-keygen -t ed25519 -f ssh_host_ed25519_key < /dev/null

    ssh-keygen -t rsa -b 4096 -f ssh_host_rsa_key < /dev/null

## NGinx
Generate an ssl certificate with lets encrypt, using provided hooks.

    sudo certbot certonly --standalone --agree-tos \
    --pre-hook ~/stack/hooks/pre.sh \
    --post-hook ~/stack/hooks/post.sh \
    --preferred-challenges http -d <domain>

Remember: change **\<domain>** to your domain. You might need to change pre and post hook paths

Inside **nginx/nginx.conf**, at line 30 and 31, change **\<host>** to your domain.

## File browser
After starting the stack, your login at file browser is admin/admin, you can change it.