 <h1> ¿Como funciona git? </h1>
También es conocido como  VCS version control system
Es muy parecido al protoco FTP,: Entorno Local ⇋ Aplicación FTP ⇋ Entorno de Producción pero de otra forma, con un Repositorio  entorno local <-> Repositorio <-> Entorno de Prodiccion
Repositorio Origen, el repositorio local es la copia de este
Para sincronizar el código con el repositorio origen, se debe de sincronizar el repositorio origen con el local, se conoce como push (sincronaizer = push)
pull, sincroniza el repositorio origen con el de Producción

<h1> COMANDO BASICOS GIT </h1>
git status -> Comprueba el estado de un repositorio
git add -> Agrega los nuevos archivos al repositorio llevando así un control sobre ellos.
git commit -> Agrega al repositorio los cambios en archivos que se han modificado
git push -> Sude los archivos al repositorio origen en nuestro caso  GitHub.
git pull -> descarga los archivos del repositorio junto con los cambios que otros usuarios hayan realizado en él.
git stash -> deshace los cambios no guardados.
git init -> crea un nuevo repositorio vacío.
git config -> Configurar diversos aspectos de Git.
git remote -> Siver para configurar un repositorio remoto asociado a un repositorio local. 


 <h1> CREAR UN NUEVO REPOSITORIO  DESDE GIT BASH fALLO </h1>

git init
git commit -m 'primer commit'
git remote add origin git@github.com:NombreCuentaUsuario/<nombreRepositorio>.git
git push -u origin master

<h1> AÑADIR NUEVOS FICHEROS AL REPOSITORIO  </h1>
 
Añadir todos los archivos modificados git add .
Añadir un archivo en específico git add nombreArchivo.extension
Añadir solo 2 o x archivos git add nombreArchivo1.extension nombreArchivo2.extension ... NombreCarpeta/nombreArchivo3.extension

git commit agrega los cambios en los archivos. 
git commit -m "Mensaje Commit"
-m -> flag que permite introducir un mensaje en la propia linea de comandos, si no se pone se abriría un editado con Vim :S :q!

<h1> ELIMINAR CAMBIOS SIN COMITEARLOS </h1>

git stash -> siempre que no se haya hecho un commit, se puede eliminar los cambios que se hayan realizado.

<h1> SINCRONIZAR EL CÓDIGO DEL REPOSITORIO ORIGEN AL REPOSITORIO LOCAL  </h1>

git pull actualiza el repositorio local con el repositorio origen. Si un usuario ha modificado el repositorio origen y 
no tienes esos cambios, lo ideal es hacer primero el git pull y luego trabajar sobre el código bueno.

//comando Completo si quiere actualizarme el codigo de una rama en específico cambio branch por la que sea.
git pull <remote> <branch>

git pull https://github.com/xjhruiz/proyecto-repo.git master


//configura la información de seguimiento para esta rama.
git branch --set-upstream-to=origin/<branch> master 

<h1> CREAR UN REPOSITORIO EN MI SERVIDOR </h1>

Se necesita tener acceso mediante ssh a tu servidor o cuenta de hosting, suele estar en la misma zona que los datos de acceso mediante FTP

comando para ello -p flag que específica el puerto
ssh usuario@dominio.com -p 22(defecto)

git --list muestra las opciones de configuración.

Dentro del serviodr hay instalar github saber el sistema operativo del hosting o servidor 

y configurar el repositorio en el servidor.

git init 
git remote add origin  https://github.com/xjhruiz/proyecto-repo.git
git pull origin master 
o se puede clonar el repositorio en github al repositorio en el servidor
git clone  git@github.com:xjhruiz/proyecto-repo.git o
git clone  https://github.com/xjhruiz/proyecto-repo.git

Si se accede a la url del proyecto desde el navegador, se tendría que ver. 
Ahora para actualizar el código del repositorio del servidor con el código del repositorio origen 
bastará con hacer un pull desde la terminal mediante ssh y dejar a un lado ftp .
(IMPORTANTE DIFERENCIAR RAMAS para no alterar codigo de Producción)


<h1> RAMAS QUÉ SON Y CÓMO CREARLAS </h1>

git branch -> muestra la rama actual en la que uno está.
* master 
por defecto solo se crea una con * indica la rama en la que uno está.

git branch -d nombreRamaAEliminar -> elimina una rama !!!!!JAMAS ELIMINAR LA RAMA MASTER!!!!

git push <remote> --delete <branch>

$ git push origin --delete fix/authentication

git checkout nombreRamaACambiar -> Cambia o Crear si esa rama no existe 
git checkout -b nombreRamaACrearVacia -> flag -b indica que se quiere crear una rama vacía, seguido del nombre de la rama
$ git checkout -b pruebas 
$ git branch
  master
* pruebas

<h1> VISUALIZAR DIFERENCIAS ENTRE RAMAS </h1>

git diff --stat master pruebas -> muestra las diferencias que hay entre las ramas por pantalla.
Muestra las línas modificadas y aquellos ficheros que ha sido agregados o eliminados. 


<h1> FUSIONAR CAMBIOS ENTRE RAMAS "MERGEAR" CAMBIOS </h1>

Para integrar los cambios de una rama a otra, de pruebas a master siempre hay que probar que dichos cambios funcionen correctamente y no alteren otras funcionalidades
Se hace un branch merging, mezclar o integrar ramas en la rama master. 
Se tiene que cambiar a la rama master (git checkout master) e integrar los cambios de la rama pruebas, con git merge. 

$ git checkout master
$ git merge pruebas 

Una vez integrado los cambios de la rama pruebas a la rama master, se puede borrar esa rama. 
git branch -d pruebas

<h1> SOLUCIÓN DE CONFLICTOS </h1>

Modifico el fichero index.html y hago un commit de los cambios y un push a la RamaA 

Modifico el fichero index.html desde la rama master y hago un commit y un push a la RamaB 

Hago un merge de la ramaA a la rama master , no hay ningun error.
Hago un merge de la ramaB a la rama Master, error 

    Auto-merging index.html
    CONFLICT (content): Merge conflict in index.html
    Automatic merge failed; fix conflicts and then commit the result.

$ git checkout master
$ git checkout -b ramaA
$ git add .
$ git commit -m 'conflictos entre ramas ramaA'
$ git push origin ramaA
$ 
$ git checkout master
$ git checkout -b ramaB
$ git add .
$ git commit -m 'conflictos entre ramas ramaB'
$ git push origin ramaB
$ 
$ git checkout master
$ git merge ramaA  TODO OK
$ 
$ git merge ramaB Error Conflictos.

Gracias al editor VSCode la extension muestra unas opciones para que sea más facil corregir estos conflictos de cambios.
Aceptar cambio entrante, Aceptar cambio Actual, Aceptar ambos cambios, Comprar Cambios

Seleccio Aceptar ambios cambios, depende de cada caso.
y se vuelve a hacer el git add . o solo el fichero
git commit -m "Solucionar conflictos entre ramas durante merge"

<h1> DESHACER UN MERGEA </h1>

Es aconsejable crear una copia de la rama en la que se va a integrar los cambios, por si algo va mal.
git reset --hard HEAD -> Permite volver a un paso anterior a realizar el merge 
el flag --hard HEAD devuelve los archivos del directorio de tu proyecto al punto anterir del merge.

<h1> DESHACER UN COMMIT </h1>

GIT RESET

git reset --head ORIG_HEAD -> deshacer el último commit realizado.
git reset --hard HEAD^ -> deshace el último commit realizado.
git reset --hard HEAD~2 <commit> deshace los dos último commit realizados.
Si se quiere eliminar deshacer más commit existe el comando revert
git reset --hard <commit> -> elimina el commit del historial. siendo commit el commit 4fc29e9e269372d13b358f7bf329890d83a1df95
si se quiere guardar un commir en otra rama
git reset
git branch newbranchname antes de git reset
git reset (--patch | -p) [tree-ish] [--] [ruta] -> permite seleccionar trozos de código y revertirlos o unstage los archivos
git reset HEAD^  <commit> -> elimina el commit pero mantiene los cambios realizados.
git reset HEAD ARCHIVO-A-UNSTAGE elimina archivos en estado staging , antes del committed
git reset MODE COMMIT -> restablece una rama o un commit anterior

Las opciones para MODE son:

--soft: no restablece el fichero índice o el árbol de trabajo, pero restablece HEAD para commit. Cambia todos los archivos a "Cambios a ser commited".  
--mixed: restablece el índice, pero no el árbol de trabajo e informa de lo que no se ha actualizado.
--hard: restablece el índice y el árbol de trabajo. Cualquier cambio en los archivos rastreados en el árbol de trabajo desde el commit son descartados.
--merge: restablece el índice y actualiza los archivos en el árbol de trabajo que son diferentes entre el commit y HEAD, pero mantiene los que son diferentes entre el índice y el árbol de trabajo.
--keep: restablece las entradas del índice, actualiza los archivos en el árbol de trabajo que son diferentes entre commit y HEAD. Sin un archivo que es diferente entre commit y HEAD tiene cambios locales, el reinicio se aborta.

Hay que tener cuidado con --hard y git reset, ya que se puede perder código escrito.

<h1> GIT REVERT </h1>
Si se ha subido el commit al repositorio remoto, es aconsejable usar git revert, ya que facilita el manejo del historial de los commit 
git revert -> deshace los cambios realizados por un commit anterior creando un commit completamente nuevo, sin alterar el historial de commit.
Aunque depende de cada casuistica querras hacer una cosa u otra.

git revert [--[no-]edit] [-n] [-m parent-number] [-s] [-S[<keyid>]] <commit>…
git revert --continue
git revert --quit
git revert --abort

opciones comunes:
-e -> no abre el editor de texto
--edit -> abre el editor de texto
-n evita que cree un nuevo commit y solo deshace el commit anterior y lo añadira en el directorio de trabajo  
-no-commit

 <h1> MÁS COMANDOS DE GIT VISITAR </h1>

https://sethrobertson.github.io/GitFixUm/fixup.html#remove_last