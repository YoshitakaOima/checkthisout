## Cómo crear workflows que echan a andar proyectos hechos con docker a partir de repos de github

### Hazte un token si no tienes uno

Vas a necesitar de antemano un token de github, para ello ve a "Tu Perfil" > "Settings" > "Developer" > "Personal access tokens" y generas un token. Este token te servirá para cualquier mierda que quieras hacer con la API de Github

### Primero que todo

**No clones este repo**. Solo sigue las instrucciones y yap

Llévate el archivo "skel.yml" (dentro de la carpeta .github/workflows) y escoge un repo que se usa con Docker (tener un Dockerfile que indique correr el py que quieres que se ejecute) y toma nota de las variables de entorno que usa. El repo puede ser privado o público, lo que importa es que TU tengas acceso al repo

### Crea 3 gists

Si no sabes lo que es un gist, googlea "github gists" y te va a salir para crear uno o más

En este repo hay 3 scripts:
- El que jala el repo: https://github.com/AniS3kai/wf-s3/blob/main/repo-getter.sh
- El que hace las cosas de docker: https://github.com/AniS3kai/wf-s3/blob/main/docker-build-n-run.sh
- El que repite el workflow: https://github.com/AniS3kai/wf-s3/blob/main/workflow-repeater.sh

1 - Haz un gist para cada uno. Tienes que copiar el contenido de cada script a cada gist

2 - En el caso del script que hace las cosas con docker, lo tienes que modificar según el repo escogido y ponerle las env vars que pide en el comando de docker run que tiene. Los otros gists te servirán para cualqueir otro repo tal y como están, no hay que modificarlos

3 - Llévate el enlace de descarga en el botón que pone "RAW" de cada uno (o sea, tocas el boton que dice RAW y el enlace de esa web lo copias, pero espera, porque tienes que editarlo. Ese enlace ubica la parte que dice /raw/ y borra todo lo que esta despues de eso hasta el proximo / y te deberia quedar .../raw/workflow-repeater.sh)

### El nuevo repo 

Tienes que crear un nuevo repo donde estará el workflow que echará a andar el repo escogido. Este nuevo repo debe ser público

Al crear el repo:

1 - Ve a "El nuevo repo" > "Actions" > Escoge el "Simple workflow" y le das al botón "Configure" para editarlo

2 - Copia el contenido de "skel.yml" en este nuevo workflow. Se recomienda llamar al archivo "skel.yml" también, pero puedes cambiar el nombre de etiqueta del workflow en la variable "name" al principio

3 - Ahora ve a "El nuevo repo" > "Settings" > "Secrets" > "Actions" para crear los secrets

### Los secrets del nuevo repo

Crea los siguientes secrets:
- GH_TOKEN = El token de Github
- GH_R_OWNER = El nombre del dueño del repo
- GH_R_NAME = El nombre del repo
- MY_SCRIPT1 = La URL del gist del script para jala el repo
- MY_SCRIPT2 = La URL del gist del script de docker (el que se debe modificar según el repo escogido)
- MY_SCRIPT3 = La URL del gist del script que repite el workflow

### Encender o apagar el workflow

Para encenderlo: Vas a "El nuevo repo" > "Actions" > "El workflow" y le das al botón que pone "Run Workflow"

Para apagarlo: Vas a la corrida del workflow, le das al menu en las "..." y te va a salir para cancelar

### El workflow SKEL

Dentro de este workflow hay 2 variables que se pueden modificar:
- La variable "WORKFLOW" es el nombre del archivo workflow, si lo cambias, debes poner el nombre aquí
- La variable "BRANCH" que es la rama a recoger del repo seleccionado, por defecto se trata de jalar la rama "main", pero hay repos que todavía usan "master" o el código que sirve está en otra rama cualquiera, etc...

### ¿Puedo meter más workflows en este mismo repo?

Por cada repo que quieras agregar, es un nuevo workflow que deriva de "skel.yml", un nuevo gist del script de docker para el repo que quieres agregar, y nuevos secrets para las variables de dueño del repo y nombre del repo que vas a tener que modificar en el nuevo workflow
