% Shell Scripting y AWK
% Adolfo Sanz De Diego
% @asanzdiego




# Autor




## Adolfo Sanz De Diego

Asesor. Desarrollador. Profesor. Formador.

- Blog: [asanzdiego.com](https://www.asanzdiego.com/)
- Correo: [asanzdiego@gmail.com](mailto:asanzdiego@gmail.com)
- GitHub: [github.com/asanzdiego](http://github.com/asanzdiego)
- Twitter: [twitter.com/asanzdiego](http://twitter.com/asanzdiego)
- LinkedIn: [in/asanzdiego](http://www.linkedin.com/in/asanzdiego)
- SlideShare: [slideshare.net/asanzdiego](http://www.slideshare.net/asanzdiego/)




# Shell Script




## Introducción

> - Un shell script es un **fichero de texto con comandos**.
> - Para ejecutarlo **no hay ni que compilar, ni tener nada instalado**, y puedes utilizar todos los comandos del sistema.
> - Usalo para **automatizar tareas del sistema y/o procesar datos** de forma rápida.
> - Usalo para hacer **pequeños scripts, no grandes programas**, para eso tienes otros lenguajes.

## Hola mundo

~~~{.bash}
#! /bin/bash

# script showing a "Hello world!"

echo "Hello world!"
~~~

## Permisos

- Antes de ejecutar hay que **darle permisos**, pero recuerda, un gran poder conlleva una gran responsabilidad :-)

~~~{.bash}
$ chmod +x 01_hello_world.sh
~~~

## Ejecución

- Para ejecutar un script:
    - si está en el $PATH, **el nombre directamente**
    - si no, desde la carpeta, **./nombre.sh**

~~~{.bash}
$ ./01_hello_world.sh
~~~

[examples/01_hello_word.sh](https://github.com/asanzdiego/curso-shell-script-y-awk-2019/blob/master/ejemplos/01_hello_word.sh)

## Nombres

- Estas son las **reglas de estilo** de Google:

~~~{.bash}
ficheros_shell_scripts.sh
VARIABLES_DE_ENTORNO
variables_locales
nombres_de_funciones
~~~

## Inicio

- Es una buena práctica **empezar los shells scripts** así:

~~~{.awk}
#! /bin/bash

# Short description of the script

set -o errexit  # the script ends if a command fails
set -o pipefail # the script ends if a command fails in a pipe
set -o nounset  # the script ends if it uses an undeclared variable
# set -o xtrace # if you want to debug
~~~

## Exit

- Es una buena práctica terminar los shells scripts con un un código de retorno:
    - **mayor de 0** si ha habido un error
    - **igual a 0** si termina correctamente (si no pones 'exit')

~~~{.bash}
#! /bin/bash

num_params=$#

if [ $num_params -lt 1 ]; then
    echo "At least one parameter must be introduced."
    exit 1 # error and exits with a return code > 0
fi

echo "All ok" # ok and exits with a return code = 0
~~~

## Parciales

- Podemos **guardar el resultado de la ejecución de comandos** en variables con $(codigo):

~~~{.bash}
date=$(date +'%Y-%m-%d %H:%M:%S')
~~~

## Funciones

- Es una buena práctica asignar los **parámetros de la función** al principio ya sean como variables locales o, si es necesario, globales.

~~~{.awk}
my_function() {
  local function_param_1="$1"    # 1st param assigned as local
  global_param_2=${2:-default}   # 2nd param assigned as global (default)
  local function_num_params=$#   # numbers of params assigned as local
  local all_function_params=($@) # all params assigned as a local
}
~~~

~~~{.bash}
my_function function_param_1 function_param_2 ... function_param_N
~~~

## Main

- Es una buena práctica **estructurar el código en funciones** y tener una función main que llamamos al final del script con todos los parámetros.

~~~{.bash}
# Main function
main() {

  check "$@"
  params "$@"
  print
}

main "$@" # call the main function with all the parameters
~~~

## Parámetros

- Los parámetros los cogemos de la **linea de comandos** cuando ejecutamos.

~~~{.awk}
# Default values
default_2="Mundo"

param_1=$1                 # the first script param
param_2=${2:-${default_2}} # the second script param (with default value)
num_params=$#              # the numbers of script params
all_params=($@)            # all params assigned as an array
~~~

~~~{.bash}
$ ./02_parameters.sh param_1 param_2 ... param_N
~~~

[examples/02_parameters.sh](https://github.com/asanzdiego/curso-shell-script-y-awk-2019/blob/master/ejemplos/02_parameters.sh)

## Template

<a href="https://github.com/asanzdiego/curso-shell-script-y-awk-2019/blob/master/recursos/template.sh">
  <img style="width:70%" src="../img/template.png" alt="Plantilla de Bash Shell Script"/>
</a>

[Plantilla de Bash Shell Script](https://github.com/asanzdiego/curso-shell-script-y-awk-2019/blob/master/recursos/template.sh)

## Chuleta

<a href="https://github.com/asanzdiego/curso-shell-script-y-awk-2019/blob/master/recursos/chuleta-shell-script.pdf">
  <img style="width:100%" src="../img/chuleta-shell-script.png" alt="Cheleta de Bash Shell Script"/>
</a>

[Chuleta de Bash Shell Script](https://github.com/asanzdiego/curso-shell-script-y-awk-2019/blob/master/recursos/chuleta-shell-script.pdf)

## ShellCheck

<a href="https://github.com/koalaman/shellcheck">
  <img style="width:70%" src="../img/shellcheck-a-shell-script-static-analysis-tool-bis.png" alt="ShellCheck, el lint de Bash Shell Script"/>
</a>

[ShellCheck, el lint de Bash Shell Script](https://github.com/koalaman/shellcheck)

## Google Guide

<a href="https://google.github.io/styleguide/shell.xml">
  <img style="width:70%" src="../img/google-shell-style-guide.png" alt="Guía de estilo de Google de Bash Shell Script"/>
</a>

[Guía de estilo de Google de Bash Shell Script](https://google.github.io/styleguide/shell.xml)

## Tutorial

<a href="https://linuxconfig.org/bash-scripting-tutorial-for-beginners">
  <img style="width:70%" src="../img/bash-scripting-tutorial-for-beginners-bis.png" alt="Tutorial de Bash Shell Script"/>
</a>

[Tutorial de Bash Shell Script](https://linuxconfig.org/bash-scripting-tutorial-for-beginners)




# AWK




## Introducción

> - Es una **hoja de cálculo** por línea de comandos.
> - Tiene **su propio penguaje** que es muy parecido a C.
> - Muy útil para **procesar datos dentro de un shell script**.
> - Muy útil para **hacer cosas raras con datos** y que con una hoja de cálculo normal es dificil de hacer.
> - Muy útil cuando **hay muchos datos** y una hoja de cálculo se queda colgada.

## Ejecución

~~~{.awk}
awk 'awk_program' data_file
~~~

~~~{.awk}
awk -f 'awk_file' data_file
~~~

## Grades

- Sacar las medias de los alumnos:

~~~{.awk}
Pepito      4.4  3.1  5.7
Fulanito    4.2  6.5  8.8
Menganito   5.6  5.0  5.3
~~~

~~~{.bash}
awk '{ print $1"="($2+$3+$4)/3 }' 03_grades.csv
~~~

[examples/04_grades.sh](https://github.com/asanzdiego/curso-shell-script-y-awk-2019/blob/master/ejemplos/04_grades.sh)

## Roles

- Agrupar por rol:

~~~{.awk}
Pepito:Jefe,Sistemas
Fulanito:Jefe,Desarrollo
Menganito:Operario,Sistemas,Desarrollo
~~~

~~~{.bash}
Sistemas -> Menganito Pepito 
Operario -> Menganito 
Jefe -> Fulanito Pepito 
Desarrollo -> Menganito Fulanito 
~~~

## Sin awk

~~~{.bash}
roles_file=./05_roles.csv

roles=$(cut -d : -f 2 $roles_file | sed 's/,/\n/g' | sort | uniq)

for rol in $roles; do
  echo -n "${rol} -> "
  echo $(grep -E "${rol}" "${roles_file}" | cut -d : -f 1)
done
~~~

[examples/06_roles_sin_awk.sh](https://github.com/asanzdiego/curso-shell-script-y-awk-2019/blob/master/ejemplos/06_roles_sin_awk.sh)

## Con awk

~~~{.awk}
# this will run only once at first
BEGIN { FS = ",|:" }
# this will be executed for each of the lines in the file
{
  name=$1
  for (i=2; i<=NF; i++) {
    roles[$i]=roles[$i]" "old_names
  }
}
# This will only run once at the end
END {
  for (rol in roles) {
    print rol" -> " roles[rol]
  }
}
~~~

[examples/08_roles.awk](https://github.com/asanzdiego/curso-shell-script-y-awk-2019/blob/master/ejemplos/08_roles.awk)

## Tutorial

<a href="http://www.grymoire.com/Unix/Awk.html">
  <img style="width:70%" src="../img/awk-a-tutorial-and-introduction-by-bruce-barnett.png" alt="Tutorial de AWK"/>
</a>

[Tutorial de AWK](http://www.grymoire.com/Unix/Awk.html)




# Acerca de




## Licencia

[Creative Commons Reconocimiento-CompartirIgual 3.0](http://creativecommons.org/licenses/by-sa/3.0/es/)

## Fuentes

[github.com/asanzdiego/curso-shell-script-y-awk-2019/](https://github.com/asanzdiego/curso-shell-script-y-awk-2019/)

## Slides

Las slides están hechas con **[MarkdownSlides](https://github.com/markdownslides/markdownslides)**.
