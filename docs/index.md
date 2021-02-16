# Práctica 1 - Configuración de máquina virtual en el IaaS

En esta práctica llevaremos a cabo la configuración de la máquina virtual que tienen disponible a través
del servicio IaaS de la ULL, además de la instalación y configuración de todas las herramientas necesarias
para comenzar a trabajar en la asignatura.

Tendrán que llevar a cabo un informe acerca de todos los pasos que han seguido, además de enumerar
los diferentes problemas a los que se han enfrentado con sus respectivas soluciones.

## Algunas tareas previas

1. Cumplimente la [encuesta de elección de grupo de trabajo](https://campusingenieriaytecnologia.ull.es/mod/choicegroup/view.php?id=281122).
2. Cumplimente la [encuesta sobre expectativas y conocimientos previos](https://campusingenieriaytecnologia.ull.es/mod/feedback/view.php?id=281123).
3. Dese de alta en el aula [Google Classroom](https://classroom.google.com/c/MjMxMDkxNzcyMTY5?hl=es&cjc=pyq2dp7) de la asignatura.
4. Partiremos de la idea de que ya posee una cuenta [GitHub](https://github.com/) asociada a su correo institucional
(aluXXXXXXXXXX@ull.edu.es). En caso contrario, créela.
5. Solicite los [beneficios de estudiantes de GitHub Education](https://education.github.com/discount_requests/student_application).
6. Si no lo ha hecho antes, dese de alta en [GitHub Classroom](https://classroom.github.com/) haciendo uso de su cuenta de GitHub, preferiblemente, aquella asociada a su correo institucional.
7. Acepte la [asignación de GitHub Classroom](https://classroom.github.com/a/ckIr4G7P) asociada a esta práctica.
8. Lea esta breve [introducción a Markdown](https://guides.github.com/features/mastering-markdown/). Deberá usar Markdown como lenguaje para escribir sus informes de prácticas. Lea el siguiente recurso sobre [GitHub Pages](https://docs.github.com/en/github/working-with-github-pages). Habilite GitHub Pages en el repositorio asociado a esta práctica y desarrolle el informe de la misma como una GitHub Page haciendo uso de Markdown. GitHub Pages utiliza Jekyll, una herramienta que transforma ficheros de texto en sitios Web. Para más información, visite el sitio Web de [Jekyll](https://jekyllrb.com/). Los siguientes cursos de [GitHub Learning Lab](https://lab.github.com/) también le podrían ser de ayuda para acometer esta tarea:
    * [GitHub Pages](https://lab.github.com/githubtraining/github-pages)
    * [Communicating using Markdown](https://lab.github.com/githubtraining/communicating-using-markdown)

## Configuración de la máquina virtual en el IaaS

Lo primero que debemos hacer es configurar el servicio VPN de la ULL en el caso de que estemos tratando de utilizar el servicio IaaS desde
fuera de la red universitaria. Para ello, debemos acceder a la
[documentación de configuración de la VPN de la ULL](https://www.ull.es/servicios/stic/2020/12/01/servicio-de-vpn-de-la-ull/)
y descargar las instrucciones correspondientes a nuestro sistema operativo. Siga dichas instrucciones y trate de conectarse a la VPN de la ULL.

Una vez conectado a la VPN, deberá acceder al [Servicio IaaS de la ULL](https://iaas.ull.es/) e introducir sus credenciales ULL. A continuación, se le mostrará un panel de control con las diferentes máquinas virtuales que tiene asociadas a su cuenta. Si no lo ha hecho antes,
deberá iniciar la máquina virtual que se llama *DSI*. Al arrancar dicha máquina, se le asignará una máquina virtual del pool de máquinas virtuales
de la asignatura. Sabrá que lo anterior ha sucedido debido a que su máquina pasará a tener un número como sufijo, por ejemplo *DSI-76*.

Haga clic en el nombre de su máquina virtual y, en la parte derecha de la interfaz, donde se indica *Interfaces de red*, podrá conocer
la IP asignada a la interfaz de su máquina. Conociendo dicha IP, ya podrá conectarse por SSH a su máquina. Es importante recordar que
siempre tiene que estar conectado a la VPN de la ULL para trabajar con una máquina virtual del IaaS. Para conectarse por SSH a su
máquina virtual, abra una terminal y teclee lo siguiente:

```bash
ssh usuario@10.6.XXX.XXX
```

Introduzca `yes` y pulse intro ante la siguiente pregunta:

```bash
The authenticity of host '10.6.XXX.XXX (10.6.XXX.XXX)' can't be established.
ECDSA key fingerprint is SHA256:1Xm4M66FeBUSiykP7SqJgObwjmVs2gEouBhy1PTWDV4.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

La contraseña que debe introducir ahora es `usuario` (las credenciales por defecto son `usuario`, `usuario` para el nombre de usuario y
contraseña, respectivamente). Una vez introducida dicha contraseña, el propio sistema le indicará que la modifique. Para ello, deberá
introducir su contraseña actual (`usuario`) y, luego, su nueva contraseña por duplicado. Trate de recordar su nueva contraseña
o apúntela en un lugar seguro. Una vez hecho lo anterior, tendrá que volver a iniciar una conexión SSH con su máquina, pero esta vez
deberá usar su nueva contraseña para acceder.

Ahora lo que podemos hacer es modificar el nombre de host de la máquina virtual:

```bash
usuario@ubuntu:~$ cat /etc/hostname
ubuntu
usuario@ubuntu:~$ sudo vi /etc/hostname
usuario@ubuntu:~$ cat /etc/hostname
iaas-dsi2
```

Tal y como puede observar, he llamado a la máquina virtual `iaas-dsi2`. También debemos modificar ciertos parámetros en
el siguiente fichero:

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

En este caso, he cambiado el antiguo nombre de host `ubuntu`, por el nombre de host `iaas-dsi2`. Antes de proceder a reiniciar la máquina
virtual para que todos los cambios tengan efecto, actualice el software de la misma:

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

Aprovechando que su máquina virtual está reiniciando, puede editar su fichero de hosts de la máquina local para incluir información
de conexión a la máquina virtual. De ese modo, no tendrá que recordar la IP de la máquina virtual:

```bash
edusegre@lluvia:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	lluvia
...

edusegre@lluvia:~$ sudo vi /etc/hosts

edusegre@lluvia:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	lluvia

10.6.XXX.XXX	iaas-dsi2
...
```

Ahora toca configurar, en la máquina local, la infraestructura de clave pública-privada, si es que no lo había hecho
anteriormente: 

```bash
edusegre@lluvia:~$ cat .ssh/id_rsa.pub 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDJhRJAXKxxJTOrS/2A1vB5p7BgFlM2rAr6qvmsye5madY3dYFw8JooM2axmShEEvepw6jGf/y9Nj3hK3i8VQabFS0r1kJMvWP1/IpDWjY8qKrAoM7gATyHSm3v66ZV6GzyTEUhfAjYIDrnL+O+W3nE3csCY9rbwE6N40wgGf/kCsJAzsk8RM2bG3jHOhjrIovY/sSjf2ZnRaawSES0LXHJu9M1Cokd+KjIqpQWvexNROJBgz/QbDKp8k2HghBtJ5LlnjxJTOe7UxLqf2xlf5Ahvs1jQuFXMRaX9S3yL1+eAvZH4pH8JFxLL1hn76q7vocmrnv/+jOBCUp1+9FE8y7P edusegre@lluvia
```

Lo anterior demuestra que yo ya había generado mi par de claves pública-privada. En caso de que dicho fichero no exista, ejecute el
siguiente comando:

```bash
edusegre@lluvia:~$ ssh-keygen
```

Por lo general, puede tomar las opciones por defecto en cada paso del script de generación de claves. Lo que si es importante es no introducir
ninguna *passphrase* asociada al par de claves pública-privada. Una vez generadas las claves, ejecute el siguiente comando, lo cual permitirá
copiar su clave pública desde la máquina local a la máquina virtual:

```bash
edusegre@lluvia:~$ ssh-copy-id usuario@iaas-dsi2
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

Y efectivamente, tratamos de hacer inicio de sesion en la máquina virtual ejecutando el siguiente comando:

```bash
edusegre@lluvia:~$ ssh usuario@iaas-dsi2
Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 4.15.0-135-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Fri Feb  5 10:16:46 WET 2021

...

Last login: Fri Feb  5 10:14:25 2021 from 10.20.53.243
usuario@iaas-dsi2:~$
```

Tal y como puede observarse, hemos sido capaces de acceder a la máquina virtual sin necesidad de introducir ninguna
contraseña. Además de lo anterior, en el prompt de la máquina virtual, debería aparecer el nuevo nombre de host
configurado previamente.

Si tampoco quisiera utilizar el nombre de usuario (`usuario`) de la máquina virtual a la hora de conectarse vía
SSH, puede configurar el siguiente fichero en su máquina local:

```bash
edusegre@lluvia:~$ touch ~/.ssh/config 
edusegre@lluvia:~$ vi ~/.ssh/config 
edusegre@lluvia:~$ cat ~/.ssh/config 
Host iaas-dsi2
  HostName iaas-dsi2
  User usuario
```

Ahora podrá iniciar una conexión SSH simplemente indicando el nombre de la máquina virtual:

```bash
edusegre@lluvia:~$ ssh iaas-dsi2
```

Genere las claves pública-privada en su máquina virtual también, siguiendo los pasos que siguió anteriormente en su
máquina local:

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
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDfCKDPGn7qLhwmjKCYaCBeOZVObmdHQ/GFOYALTU1Lmnjb108HGSr7aDaFZunI1TtwAKh1qGdo6LYCPECY+y90LA+vtRTdQnoPqGzsTctillZkRJoMv7beLhpHVKFadXNc/DKlwCU83uHwTRGmqb3OY5246rSIA+/blFpDEBpK088oXvTailphaCeZRHV+Qg12LJu5Q2uKBTjckU0yebz4Xx2wXjZQkpohX8PSOJpKy6QlNmG8j3DDY+qrRmy+LScRGyWHlqQIVR2YrejuHqs2mG1b8FSGNwUUCp20rc0TWV22ggjQxEmjCRAHIofRsZ7zN752aChLqWGXcDJLTI8d usuario@iaas-dsi2
```

## Instalación de git y Node.js en la máquina virtual del IaaS

Instale git en la máquina virtual, aunque suele venir preinstalado con el sistema operativo:

```bash
usuario@iaas-dsi2:~$ sudo apt install git
Leyendo lista de paquetes... Hecho
Creando árbol de dependencias       
Leyendo la información de estado... Hecho
git ya está en su versión más reciente (1:2.25.1-1ubuntu3).
0 actualizados, 0 nuevos se instalarán, 0 para eliminar y 0 no actualizados.
```

Si necesita refrescar sus conocimientos sobre git, por favor, acceda al libro
["Pro Git" de Scott Chacon y Ben Straub](https://git-scm.com/book/es/v2). Lo que si es importante hacer es seguir las instrucciones
disponibles en dicho libro para [configurar git por primera vez](https://git-scm.com/book/es/v2/Inicio---Sobre-el-Control-de-Versiones-Configurando-Git-por-primera-vez) en la máquina virtual. Básicamente, la configuración
se reduce a ejecutar los siguientes comandos:

```bash
usuario@iaas-dsi2:~$ git config --global user.name "Eduardo Segredo"
usuario@iaas-dsi2:~$ git config --global user.email esegredo@ull.edu.es
usuario@iaas-dsi2:~$ git config --list
user.name=Eduardo Segredo
user.email=esegredo@ull.edu.es
```

Ahora, configuremos el prompt de la terminal para que aparezca la rama actual en la que nos encontramos cuando
accedemos a algún directorio que resulta estar asociado a un repositorio git. Para ello, descargue el script
[git prompt](https://github.com/git/git/blob/master/contrib/completion/git-prompt.sh) y eche un vistazo a su contenido
para aprender a configurarlo en su máquina virtual. Los pasos que doy a continuación serían para una `bash`:

```bash
usuario@iaas-dsi2:~$ mv git-prompt.sh .git-prompt.sh
usuario@iaas-dsi2:~$ vi .bashrc
usuario@iaas-dsi2:~$ tail .bashrc
...
source ~/.git-prompt.sh
PS1='\[\033]0;\u@\h:\w\007\]\[\033[0;34m\][\[\033[0;31m\]\w\[\033[0;32m\]($(git branch 2>/dev/null | sed -n "s/\* \(.*\)/\1/p"))\[\033[0;34m\]]$'

usuario@iaas-dsi2:~$ exec bash -l
[~()]$
```

Tal y como puede observarse, los pasos básicos a seguir son, por un lado, la descarga del script y, por el otro, modificar el fichero
`~/.bashrc`, incluyendo al final del mismo las dos líneas que aparecen en el código anterior. En caso de utilizar otro tipo de terminal,
lea la documentación incluida en el propio script `git-prompt.sh`. El último comando permite reiniciar la terminal. Podremos observar que
el formato del prompt ha cambiado.

No obstante, para comprobar si realmente el prompt muestra lo que deseamos, que no es otra cosa que la rama actual de trabajo cuando
accedemos a un directorio asociado a un repositorio, tendremos que añadir la clave pública de la máquina virtual en la configuración
de las claves de nuestra cuenta de GitHub, de modo que nos sea mucho más fácil trabajar con repositorios remotos, y así poder también
clonar alguno de los repositorios para hacer la prueba. En primer lugar, copie la clave pública de su máquina virtual:

```bash
[~()]$cat ~/.ssh/id_rsa.pub
```

Una vez copiada, acceda a la configuración de su cuenta de GitHub (*account settings*), y en la sección *SSH and GPG keys*, pulse sobre
el botón *New SSH key*. En el formulario añada un título para la clave (yo tengo *usuario@iaas-dsi2*) y pegue la clave pública de su
máquina virtual en el campo de texto *key*. Por último, pulse sobre el botón *Add SSH key*. Si todo ha ido bien, ahora podría ejecutar
el siguiente comando desde la máquina virtual para clonar un repositorio:

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

Observe como se ha clonado el repositorio almacenado en GitHub de manera satisfactoria sin necesidad de introducir ningún tipo de credencial.
Además, al acceder al directorio asociado al repositorio git, observe como el prompt del sistema indica, entre paréntesis, la rama actual de
trabajo (`main`). Puede llevar a cabo la anterior comprobación con cualquiera de sus repositorios remotos.

Si desea refrescar un poco sus conocimientos sobre GitHub, le recomiendo este
[curso de introducción](https://lab.github.com/githubtraining/introduction-to-github). En general, en el
[GitHub Learning Lab](https://lab.github.com/) podrá encontrar multitud de cursos con los que aprender diferentes
aspectos del ecosistema GitHub. Un curso muy recomendable es
[*First Week on GitHub*](https://lab.github.com/githubtraining/first-week-on-github).

Ahora vamos a proceder a instalar [Node Version Manager (nvm)](https://github.com/nvm-sh/nvm), el gestor de versiones de Node.js.
[Node.js](https://nodejs.org/en/) es un entorno que permite la ejecución de código desarrollado en JavaScript y variantes, como
por ejemplo, TypeScript.

```bash
[~()]$wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
[~()]$exec bash -l
[~()]$nvm --version
0.37.2
```

Puede observar como hemos instalado nvm satisfactoriamente. Una vez hecho lo anterior, vamos a proceder a instalar la versión más
reciente de Node.js:

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

Al instalar Node.js, se puede observar como se ha instalado la última versión (15.8.0), además de la última versión de
[Node Package Manager (npm)](https://www.npmjs.com/), el gestor de paquetes de Node.js. Más adelante, aprenderemos más
acerca de estas herramientas.

Para instalar una versión concreta de Node.js, podemos hacer lo siguiente:

```bash
[~()]$nvm install 12.0.0
Downloading and installing node v12.0.0...
Downloading https://nodejs.org/dist/v12.0.0/node-v12.0.0-linux-x64.tar.xz...
####################################################################################################################################################################################### 100,0%
Computing checksum with sha256sum
Checksums matched!
Now using node v12.0.0 (npm v6.9.0)
[~()]$node --version
v12.0.0
[~()]$npm --version
6.9.0
```

Por último, para cambiar entre versiones, podemos ejecutar los siguientes comandos:

```bash
[~()]$nvm list
->      v12.0.0
        v15.8.0
default -> node (-> v15.8.0)
iojs -> N/A (default)
unstable -> N/A (default)
node -> stable (-> v15.8.0) (default)
stable -> 15.8 (-> v15.8.0) (default)
lts/* -> lts/fermium (-> N/A)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.17.0 (-> N/A)
lts/dubnium -> v10.23.2 (-> N/A)
lts/erbium -> v12.20.1 (-> N/A)
lts/fermium -> v14.15.4 (-> N/A)
[~()]$nvm use v15.8.0 
Now using node v15.8.0 (npm v7.5.1)
[~()]$node --version
v15.8.0
[~()]$npm --version
7.5.1
```

Recuerde que debe cumplimentar un informe, a modo de GitHub Page, con todos los pasos que ha llevado a cabo para el desarrollo de esta práctica.
El repositorio debe ser el que se haya creado automáticamente tras haber aceptado la asignación de GitHub Classroom.
En el aula virtual de la asignatura, deberá entregar el enlace a dicho repositorio en la tarea correspondiente.
