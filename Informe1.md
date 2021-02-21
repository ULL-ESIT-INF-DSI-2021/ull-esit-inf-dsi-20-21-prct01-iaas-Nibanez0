# Informe Práctica 1: Configuración de máquina virtual en el Iaas

## Objetivo
El objetivo principal de esta práctica es configurar la máquina virtual del servicio Iaas de la ULL, además de la instalación y configuración de todas las herramientas necesarias para comenzar a trabajar en la asignatura "Desarrollo de Sistemas Informáticos".

## Pasos previos
Antes de comenzar, debe realizar algunos pasos previos:

1. Configurar el servicio VPN de la ULL en el caso de que estemos tratando de utilizar el servicio Iaas desde fuera de la red universitaria. Para ello, accederá a la [documentación de configuración de la VPN de la ULL](https://www.ull.es/servicios/stic/2020/12/01/servicio-de-vpn-de-la-ull/) y seguirá las instrucciones correspondientes a su sistema operativo.

2. En caso de que no disponer de cuenta en GitHub, creación de una. Se recomienda utilizar el correo institucional (@ull.edu.es).

## Configuración de la máquina virtual en el Iaas
Una vez conectado a la VPN, deberá acceder al [Servicio Iaas de la ULL](https://iaas.ull.es/ovirt-engine/sso/login.html) e introducir sus credenciales ULL. A continuación, inicie la máquina virtual llamada DSI.

Haga clic en el nombre de la máquina virtual y, en la parte derecha de la interfaz, donde se indica *Interfaces de red*, podrá conocer la IP. Acto seguido y, conociendo la IP, conéctese por SSH a su máquina virtual desde la terminal como se muestra:

```bash
ssh usuario@10.6.XXX.XXX
```

Tenga en cuenta que siempre debe estar conectado a la VPN de la ULL para trabajar con una máquina virtual del Iaas.

Introduzca ` yes ` y pulse intro ante la siguiente pregunta:

```bash
The authenticity of host '10.6.XXX.XXX (10.6.XXX.XXX)' can't be established.
ECDSA key fingerprint is SHA256:1Xm4M66FeBUSiykP7SqJgObwjmVs2gEouBhy1PTWDV4.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

La contraseña que deberá introducir ahora es ` usuario `.
Una vez introducida dicha contraseña, se le indicará que modifique la contraseña y para ello tendrá que repetir la contraseña actual (` usuario `) e introducir la nueva por duplicado. Una vez realice lo anterior, tendrá que volver a iniciar una conexión SSH con su máquina, pero esta vez deberá usar su nueva contraseña para acceder.

Ahora modificaremos el nombre de host de la máquina virtual para llamarla ` iaas-dsi2 `, tal y como se muestra seguidamente.

```bash
usuario@ubuntu:~$ cat /etc/hostname
ubuntu
usuario@ubuntu:~$ sudo vi /etc/hostname
usuario@ubuntu:~$ cat /etc/hostname
iaas-dsi2
```

Además, cambiaremos el antiguo nombre de host ` ubuntu ` por ` iaas-dsi2 ` en el siguiente fichero:

```bash
usuario@ubuntu:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	ubuntu
...

usuario@ubuntu:~$ sudo vi /etc/hosts

usuario@ubuntu:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	iaas-dsi2
...
```

Una vez finaliza estos cambios, actualice el software de la misma:

```bash
usuario@ubuntu:~$ sudo apt update
...
usuario@ubuntu:~$ sudo apt upgrade
...
```

Reinicie la máquina virtual:
```bash
usuario@ubuntu:~$ sudo reboot
Connection to 10.6.XXX.XXX closed by remote host.
Connection to 10.6.XXX.XXX closed.
```

Cuando la máquina virtual se está reiniciando, vamos a editar el archivo de host en la máquina local e incluir información de la conexión a la máquina virtual para no tener que recordar la IP de la máquina virtual:

```bash
noelia@noelia-IdeaPad-Gaming-3-15IMH05:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	noelia-IdeaPad-Gaming-3-15IMH05
...

noelia@noelia-IdeaPad-Gaming-3-15IMH05:~$ sudo vi /etc/hosts

noelia@noelia-IdeaPad-Gaming-3-15IMH05:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	noelia-IdeaPad-Gaming-3-15IMH05
10.6.XXX.XXX   iaas-dsi2
...
```

Ahora toca configurar, en la máquina local, la infraestructura de clave pública-privada.
Paara comprobar si ese paso lo hizo ya, ejecute:
```bash
noelia@noelia-IdeaPad-Gaming-3-15IMH05:~$ cat .ssh/id_rsa.pub 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC2viBKziXT9ssB/i88+GakFSUlPhDsjxBVWVLYeXeH5LkgJ+BTSjWwu/2EwtctLf8VwkoHU7XPPCJlxnP+FE134ZpSOM+Shy1bqLI7z4NN776kwXP4XI6ldHdSeE//+pK/acBwbDI7gpLHYQjwJUUOVTNua6IF7hu7x6KYXf/Tc4yM2LHKnEtjwZeIwHAeqnm1f7+woCJI5vs3PK6OCRqzzpjOyYTRqvnnYFg/I6UomAZMZLY4NUPPZ+WTyPmv5Y8/0O1mF8bug3TdTkd5QsFF12AY7n34mLVqVZxRUKrjbl+21SxWpTej5Y1PHjedjkUZVqTrRQiMTTFr/qv6/I0Nj4gaNarqgw5r8tfpL1IwPShIgs18CZ+qTwwa2XZdoBLA5T8b2vB8Sp9Rr8WemIY7aA+ilPOPNspHM4JqHrNe3U8XSbUY4UU= noelia@noelia-IdeaPad-Gaming-3-15IMH05
```

En caso de que dicho fichera no exista, ejecute el siguiente comando:
```bash
noelia@noelia-IdeaPad-Gaming-3-15IMH05:~$ ssh-keygen
```

Generalmente, puede usar las opciones predeterminadas en cada paso del script. Es importante no ingresar ninguna *passphrase* asociada al par de claves pública-privada. Después de generar la clave, ejecute el siguiente comando, que le permitirá copiar la clave pública desde la computadora local a la máquina virtual:

```bash
noelia@noelia-IdeaPad-Gaming-3-15IMH05:~$ ssh-copy-id usuario@iaas-dsi2
The authenticity of host 'iaas-dsi2 (10.6.XXX.XXX)' can't be established.
ECDSA key fingerprint is SHA256:1Xm4M66FeBUSiykP7SqJgObwjmVs2gEouBhy1PTWDV4.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
usuario@iaas-dsi2's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'usuario@iaas-dsi2'"
and check to make sure that only the key(s) you wanted were added.
```

Iniciamos sesión en la máquina virtual ejecutando:
```bash
noelia@noelia-IdeaPad-Gaming-3-15IMH05:~$ ssh usuario@iaas-dsi2
Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 4.15.0-135-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

...

Last login: Wed Feb 17 14:21:23 2021 from 10.20.52.163
usuario@iaas-dsi2:~$
```

Como hemos podido comprobar, de esta manera conseguimos acceder a la máquina virtual sin necesidad de contraseña. Además, aparece el nuevo nombre de host.

También vamos a cambiar el nombre de usuario (` usuario@ `) de la máquina virtual a la hora de conectarse vía SSH, de este modo podremos utilizar solo el nombre. Para ello haremos lo siguiente desde la máquina local:

```bash
noelia@noelia-IdeaPad-Gaming-3-15IMH05:~$ touch ~/.ssh/config
noelia@noelia-IdeaPad-Gaming-3-15IMH05:~$ vi ~/.ssh/config
noelia@noelia-IdeaPad-Gaming-3-15IMH05:~$ cat ~/.ssh/config
Host iaas-dsi2
  HostName iaas-dsi2
  User usuario
```

Ahora podrá iniciar una conexión SSH utilizando:
```bash
noelia@noelia-IdeaPad-Gaming-3-15IMH05:~$ ssh iaas-dsi2
```

Genere las claves público-privada en la máquina virtual siguiendo los mismos pasos que en la máquina local:
```bash
usuario@iaas-dsi2:~$ ssh-keygen 
Generating public/private rsa key pair.
Enter file in which to save the key (/home/usuario/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/usuario/.ssh/id_rsa.
Your public key has been saved in /home/usuario/.ssh/id_rsa.pub.
The key fingerprint is:
...

usuario@iaas-dsi2:~$ cat .ssh/id_rsa.pub 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC2viBKziXT9ssB/i88+GakFSUlPhDsjxBVWVLYeXeH5LkgJ+BTSjWwu/2Ewt+Shy1bqLI7z4NN776kwXP4XI6ldHdSeE//+pK/ewqqVWCODm5Ix97YzwvBacBwbDI7gpLHYQjwJUUOVTNua6IF7hu7x6KYXf/Tc4yM2LNUPPZ+WTyPmv5Y8/0O1mF8bsRxUWc9Kkug3TdTkd5QsFF12AY7n34mLVqVZxRUKrjbl+21SxWpTej5Y1PHjedjkUZVqTrRQiMTTFr/qv6/I0Nj4gaNarqgw5r8tfpL1IwPShIgs18CZ+qTwwa2A0HXw6QI4bM2VhlCOdg2dk7CsB3nMC5+ilPOPNspHM4JqHrNe3U8XSbUY4UU= noelia@noelia-IdeaPad-Gaming-3-15IMH05
```

## Instalación de git y Node.js en la máquina virtual del IaaS
Para instalar git en la máquina virtual, efectue el siguiente comando:

```bash
usuario@iaas-dsi2:~$ sudo apt install git
Leyendo lista de paquetes... Hecho
Creando árbol de dependencias       
Leyendo la información de estado... Hecho
git ya está en su versión más reciente (1:2.25.1-1ubuntu3).
0 actualizados, 0 nuevos se instalarán, 0 para eliminar y 0 no actualizados.
```
Para configurar git en la máquina virtual por primera vez, haga lo siguiente:

```bash
usuario@iaas-dsi2:~$ git config --global user.name "Noelia Ibanez"
usuario@iaas-dsi2:~$ git config --global user.email alu0101225555@ull.edu.es
usuario@iaas-dsi2:~$ git config --list
user.name=Noelia Ibanez
user.email=alu0101225555@ull.edu.es
```
A continuación, configure el prompt de la terminal para que aparezca la rama actual en la que nos encontramos cuando accedemos a algún directorio que resulta estar asociado a un repositorio git. Para ello,  siga los siguientes pasos:

```bash
usuario@iaas-dsi2:~$ touch .git-prompt.sh
usuario@iaas-dsi2:~$ vi .git-prompt.sh
usuario@iaas-dsi2:~$ vi .bashrc
usuario@iaas-dsi2:~$ mv .git-prompt.sh .git-prompt.sh
...
source ~/.git-prompt.sh
PS1='\[\033]0;\u@\h:\w\007\]\[\033[0;34m\][\[\033[0;31m\]\w\[\033[0;32m\]($(git branch 2>/dev/null | sed -n "s/\* \(.*\)/\1/p"))\[\033[0;34m\]]$'

usuario@iaas-dsi2:~$ exec bash -l
[~()]$
```

Básicamente, lo que hacemos es: creamos *.git-prompt.sh*, copiamos dentro de ese fichero el código que aparece en [git-prompt](https://github.com/git/git/blob/master/contrib/completion/git-prompt.sh) y añadimos en *.bashrc* las líneas que aparecen a continuación:

```bash
...
source ~/.git-prompt.sh
PS1='\[\033]0;\u@\h:\w\007\]\[\033[0;34m\][\[\033[0;31m\]\w\[\033[0;32m\]($(git branch 2>/dev/null | sed -n "s/\* \(.*\)/\1/p"))\[\033[0;34m\]]$'
```

No obstante, para comprobar si realmente el prompt muestra lo que deseamos, que no es otra cosa que la rama actual de trabajo cuando accedemos a un directorio asociado a un repositorio, tendremos que añadir la clave pública de la máquina virtual en la configuración de las claves de nuestra cuenta de GitHub. En primer lugar, copie la clave pública de su máquina virtual:

```bash
[~()]$cat ~/.ssh/id_rsa.pub
```

Una vez copiada, acceda a la configuración de su cuenta de GitHub (account settings), y en la sección SSH and GPG keys, pulse sobre el botón New SSH key. En el formulario añada un título para la clave (por ejemplo: usuario@iaas-dsi2) y pegue la clave pública de su máquina virtual en el campo de texto key. Por último, pulse sobre el botón Add SSH key. Si funciona correctamente, ahora podría ejecutar el siguiente comando desde la máquina virtual para clonar un repositorio:

```bash
[~()]$git clone git@github.com:ULL-ESIT-INF-DSI-2021/prct01-iaas-vscode.git
Clonando en 'prct01-iaas-vscode'...
remote: Enumerating objects: 100, done.
remote: Counting objects: 100% (100/100), done.
remote: Compressing objects: 100% (79/79), done.
remote: Total 100 (delta 32), reused 50 (delta 16), pack-reused 0
Recibiendo objetos: 100% (100/100), 341.75 KiB | 1.63 MiB/s, listo.
Resolviendo deltas: 100% (32/32), listo.
[~()]$ls
prct01-iaas-vscode
[~()]$cd prct01-iaas-vscode/
[~/prct01-iaas-vscode(main)]$
```
Observe como se ha clonado el repositorio almacenado en GitHub de manera satisfactoria sin necesidad de introducir ningún tipo de credencial. Además, al acceder al directorio asociado al repositorio git, observe como el prompt del sistema indica, entre paréntesis, la rama actual de trabajo (main).

Por último, procederemos a instalar Node Version Manager (nvm), el gestor de versiones de Node.js, como se muestra a continuación:

```bash
[~()]$wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
[~()]$exec bash -l
[~()]$nvm --version
0.37.2
```
Una vez hecho lo anterior, vamos a proceder a instalar la versión más reciente de Node.js:

```bash
[~()]$nvm install node
Downloading and installing node v15.8.0...
Downloading https://nodejs.org/dist/v15.8.0/node-v15.8.0-linux-x64.tar.xz...
####################################################################################################################################################################################### 100,0%
Computing checksum with sha256sum
Checksums matched!
Now using node v15.8.0 (npm v7.5.1)
Creating default alias: default -> node (-> v15.8.0)
[~()]$node --version
v15.8.0
[~()]$npm --version
7.5.1
```

Al instalar Node.js, se puede observar como se ha instalado la última versión (15.8.0), además de la última versión de Node Package Manager (npm), el gestor de paquetes de Node.js.

## Conclusión
En conclusión, para llevar a cabo esta práctica se utilizaron las instrucciones dadas por el profesor en el guión, la guía de [Markdown](https://guides.github.com/features/mastering-markdown/) y tutoriales de git.

Respecto a las dificultades encontradas durante la ejecución de la práctica, solo hubo problemas para entender correctamente la primera parte de *Instalación de git y Node.js en la máquina virtual del IaaS*. Esto se resolvió de la manera que se muentra en dicho apartado de este informe.

Una vez completado el guión y resueltos los problemas ya tenemos configurada la máquina virtual en el Iaas y las herramientas necesarias.