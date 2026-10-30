# Práctica Guiada

*Una vez creada la carpeta, comenzaremos a realizar diferentes pruebas para aprender a utilizar Git.*

**Alumno:** Lucas Alonso Gómez

## 0. Creación del directorio y los archivos

Primero creamos un directorio llamado `prueba_git` utilizando los siguientes comandos:

```bash
mkdir prueba_git
```

*Este comando sirve para crear la carpeta.*

```bash
cd prueba_git
```

*Con este segundo comando accedemos al directorio que acabamos de crear.*

### Creación de los archivos

Ahora crearemos dos archivos dentro de la carpeta:

- `texto.txt`
- `script.sh`

Para ello, abrimos Visual Studio Code desde Git Bash utilizando:

```bash
code .
```

Una vez abierto el editor, creamos ambos archivos.

En el primero, `texto.txt`, escribimos tres palabras, cada una en una línea:

```text
uno
dos
tres
```

En el segundo archivo, `script.sh`, introducimos las siguientes líneas:

```bash
echo Listado completo
ls -l
```

### Ejecución del script

Una vez guardados los archivos, volvemos a Git Bash y ejecutamos los siguientes comandos:

```bash
chmod a+x script.sh
```

*Con este comando damos permisos de ejecución al archivo.*

```bash
./script.sh
```

*Ejecutamos el script y obtenemos el siguiente resultado:*

```text
Listado completo
total 2
-rw-r--r-- 1 Lucas 197121 27 Oct 30 10:40 script.sh
-rw-r--r-- 1 Lucas 197121 14 Oct 30 10:37 texto.txt
```

Como podemos observar, el script muestra primero el mensaje indicado y después realiza un listado detallado de los archivos que contiene el directorio.

## 1. Inicialización (init y status)

Primero creamos un repositorio local nuevo utilizando el comando:

```bash
git init
```

Este comando inicializa Git en el directorio actual, creando una carpeta oculta llamada `.git`, donde se almacenará toda la información necesaria para gestionar las diferentes versiones del proyecto.

A continuación, utilizamos `git status` para consultar el estado del repositorio y comprobar qué archivos están siendo gestionados por Git.

## 2. Añadir archivos (add y commit)

Primero añadimos el archivo `script.sh` al seguimiento de Git mediante:

`git add script.sh`

Durante la ejecución aparece un aviso sobre la conversión de finales de línea (LF a CRLF), relacionado con la configuración de Windows.

A continuación, ejecutamos `git status` para comprobar el estado del repositorio.

El resultado muestra que `script.sh` está preparado para realizar un commit (Staged), mientras que `texto.txt` continúa sin estar rastreado (Untracked).

Esto significa que Git ya tiene preparado el primer archivo para confirmar una versión, pero todavía no se ha guardado ningún commit.

## 3. Modificar archivos

Abrimos el archivo `texto.txt` y añadimos una nueva línea con la palabra `cuatro`.

A continuación, ejecutamos:

```bash
git status
```

Git detecta que el archivo ha sido modificado desde el último commit, pero todavía no está preparado para confirmar los cambios.

Para preparar esta modificación utilizamos:

```bash
git add texto.txt
git status
```

Ahora el archivo aparece en el apartado `Changes to be committed`.

Sin realizar todavía un commit, volvemos a modificar `texto.txt`, añadiendo una quinta línea con la palabra `cinco`.

Si volvemos a ejecutar `git status`, observamos que el mismo archivo aparece en dos estados diferentes:

- Modificado y preparado para el siguiente commit.
- Modificado, pero todavía sin preparar.

Esto demuestra que Git mantiene separados los cambios preparados de aquellos que todavía no se han añadido al área de preparación (*Stage*).

Finalmente, añadimos la última modificación y confirmamos todos los cambios:

```bash
git add texto.txt
git commit -m "Ampliada la explicación del texto"
git status
```

El repositorio vuelve a estar limpio, sin modificaciones pendientes de confirmar.


## 4. Información (git show y git log)

Una vez realizados varios commits, podemos consultar la información de las versiones guardadas y revisar el historial del repositorio.

### 4.1. Consultar la última versión

Para obtener información sobre el último commit realizado utilizamos:

```bash
git show
```

Este comando muestra información sobre los últimos cambios confirmados, incluyendo el identificador del commit, el autor, la fecha y las modificaciones realizadas en los archivos.

### 4.2. Consultar el historial de versiones

Para consultar el historial completo de commits ejecutamos:

```bash
git log
```

Este comando muestra las diferentes versiones que hemos ido guardando en el repositorio.

En cada commit podemos encontrar su identificador único (hash), el autor, la fecha y el mensaje que escribimos al confirmar los cambios.

También aparece `HEAD -> main`, que indica que actualmente estamos trabajando sobre la rama principal `main`.

### 4.3. Consultar un commit concreto

También podemos consultar la información interna de un commit mediante:

```bash
git cat-file HASH -p
```

Sustituyendo `HASH` por una parte del identificador del commit que queremos consultar.

Este comando muestra información sobre el objeto commit, incluyendo el directorio asociado (*tree*) y el commit anterior (*parent*), además del resto de datos almacenados.


## 5. Diferencias (git diff)

Para comprobar las diferencias entre un archivo modificado y su última versión confirmada, utilizamos el comando `git diff`.

Primero abrimos `texto.txt`, eliminamos la línea `tres` y añadimos una nueva línea con la palabra `seis`.

Una vez guardados los cambios, ejecutamos:

```bash
git diff
```

El resultado muestra las diferencias entre ambas versiones del archivo.

Los principales símbolos que aparecen son:

- `-`: indica una línea eliminada respecto a la versión anterior.
- `+`: indica una línea añadida respecto a la versión anterior.
- Las líneas sin símbolos representan el contenido que permanece sin cambios.

La información que comienza por `@@` indica el fragmento del archivo que se está comparando, incluyendo las posiciones y cantidades de líneas.

Después de comprobar las diferencias, confirmamos los cambios mediante:

```bash
git commit -a -m "Probando diff"
```

Este comando permite preparar y confirmar las modificaciones de archivos que ya están siendo rastreados por Git.

Por último, añadimos la palabra `siete` como nueva línea en `texto.txt` y volvemos a ejecutar:

```bash
git diff
```

De esta manera, podemos observar que Git muestra únicamente la última modificación realizada, ya que las anteriores quedaron guardadas en el commit.


## 6. Ignorar archivos (.gitignore)

En ocasiones existen archivos que no nos interesa incluir en el control de versiones, como archivos temporales o archivos compilados.

Para evitar que Git los tenga en cuenta, creamos un archivo llamado `.gitignore` dentro del directorio del proyecto.

Introducimos las siguientes reglas:

```gitignore
# Ignora los archivos terminados en .class
*.class

# Ignora los archivos terminados en ~
*~

# No ignora importante~
!important~
```

El símbolo `#` permite introducir comentarios. El carácter `*` representa cualquier conjunto de caracteres y `!` permite establecer una excepción a una regla de exclusión anterior.

A continuación, creamos tres archivos: `Hola.java`, `temporal~` e `importante~`.

En `Hola.java` introducimos un pequeño programa:

```java
class Hola {
    public static void main(String[] args){
        System.out.println("Welcome to the Java World");
    }
}
```

Los otros dos archivos pueden permanecer vacíos.

Después compilamos el programa Java:

```bash
javac Hola.java
```

Esto genera el archivo `Hola.class`.

Para comprobar todos los archivos presentes en el directorio, incluidos los ocultos, ejecutamos:

```bash
ls -a
```

A continuación, consultamos el estado del repositorio:

```bash
git status
```

Comprobamos que Git ignora `Hola.class` y `temporal~`, mientras que sí muestra `.gitignore`, `Hola.java` e `importante~`.

Para comprobar el funcionamiento de las reglas, eliminamos temporalmente `.gitignore` y volvemos a ejecutar `git status`. Ahora también aparecen los archivos que anteriormente estaban ignorados.

Finalmente, restauramos `.gitignore` y añadimos todos los archivos pendientes:

```bash
git add .
git commit -m "Añadidos fuentes en java y archivos importantes"
```

El punto permite añadir todos los archivos pendientes, incluidos los de subdirectorios, respetando las exclusiones definidas en `.gitignore`.

Comprobamos que todo está correctamente guardado mediante:

```bash
git status
```

El repositorio vuelve a estar limpio, sin cambios pendientes de confirmar.

## 7. Tags y gestión de versiones (git tag y git checkout)

Al consultar el historial mediante `git log`, podemos observar que cada commit tiene un identificador único llamado hash.

Como estos identificadores son bastante largos, Git permite asignar etiquetas (*tags*) para identificar las versiones de una forma más sencilla.

### 7.1. Crear etiquetas

Para asignar una etiqueta a la versión actual utilizamos:

```bash
git tag v0.7
```

También podemos asignar una etiqueta a una versión anterior indicando su identificador:

```bash
git tag v0.3 HASH
```

### 7.2. Consultar las versiones

Para consultar el historial de forma resumida ejecutamos:

```bash
git log --oneline
```

Este comando muestra los commits en una sola línea, incluyendo sus identificadores abreviados y las etiquetas asociadas.

También podemos consultar directamente una versión mediante su etiqueta:

```bash
git show v0.3
```

### 7.3. Cambiar entre versiones

Si queremos consultar una versión anterior del proyecto utilizamos:

```bash
git checkout v0.3
```

Para volver a la versión más reciente de nuestra rama principal ejecutamos:

```bash
git checkout main
```

Es importante tener precaución al cambiar a versiones anteriores, ya que si queremos realizar modificaciones en ellas es recomendable crear primero una nueva rama de trabajo.

Las etiquetas se almacenan dentro del directorio `.git/refs/tags`.

## 8. Ramas (`git branch`)

Las ramas permiten trabajar en paralelo sobre un proyecto, realizar pruebas o desarrollar nuevas funcionalidades sin modificar directamente la rama principal.

### Crear y consultar ramas

```bash
git branch Prueba
git branch
```

El comando `git branch` muestra las ramas existentes. El asterisco indica la rama activa.

### Cambiar de rama

```bash
git switch Prueba
```

También se puede utilizar `git checkout Prueba`.

### Trabajar en una rama independiente

Se modifica `Hola.java` añadiendo una nueva línea:

```java
System.out.println("I have no more branches to commit, said the Ent");
```

Después se guardan los cambios y se crea una etiqueta:

```bash
git commit -a -m "Llegan lod ENTS a Java"
git tag v0.7-Ent-Release
```

### Comprobar la independencia de las ramas

Al volver a la rama principal:

```bash
git switch main
```

Se comprueba que `Hola.java` no contiene la nueva línea y que `main` conserva su versión anterior.

### Fusionar ramas

Si los cambios realizados en la rama de pruebas son satisfactorios, se pueden incorporar a la principal:

```bash
git merge Prueba
```

De esta manera, los cambios de `Prueba` pasan a formar parte de `main`.

**Conclusión:** Las ramas permiten desarrollar y probar cambios de forma independiente, integrándolos posteriormente en la rama principal cuando estén terminados.

## 9. Repositorios remotos y GitHub
