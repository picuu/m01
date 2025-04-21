# Resumen UF2 NF3 - Automatización (scripting)

## Índice

- [Creacion de un script](#creacion-de-un-script)
- [Definicion de variables](#definicion-de-variables)
- [Comillas](#comillas)
- [Comandos utiles](#comandos-utiles)
- [Variables de entorno](#variables-de-entorno)
- [Variables especiales](#variables-especiales)
- [Condicionales](#condicionales)
- [Estructuras de iteracion](#estructuras-de-iteracion)
- [Otros comandos](#otros-comandos)

## Creacion de un script

Se recomienda que la extensión del fichero sea `.sh`.

Para que el interprete sepa con que shell debe ejecutar el proceso, la primera línea del archivo deberá ser `#!/bin/bash`.

Para poder ejecutar habrá que dar permisos al fichero creado con `chmod u+x *.sh`

## Definicion de variables

Para definir una variable se utiliza el nombre de la variable, el símbolo igual y el contenido. Por ejemplo `nombre_variable="contenido string"`. No debe haber espacios entre el nombre de la variable, el símbolo de asignación y el contenido.

No se deben declarar previamente ni asignar un tipo.

Una variable puede ser modificada: `numero=$((numero-1))`. Para ejecutar el contenido de una variable se debera hacer `$nombre_variable`, pero para mostrar el contenido sin ejectutar se deberá hacer `echo $nombre_variable`.

Una variable numérica puede ser incrementada y decrementada.

```bash
$ num=5
$ echo $num
>> 5

$ (( num++ ))
$ echo $num
>> 6

$ (( num-- ))
$ echo $num
>> 5
```

## Comillas

- **Comillas simples**: interpreta literalmente la cadena de carácteres que contiene.
- **Comillas dobles**: interpreta el texto y las variables ($).
- **Comillas inversas**: se ejecuta el comando que contiene. Es lo mismo a `$(comando)`.

```bash
$ CADENA="Test"
$ echo $CADENA *.txt    # mostra la CADENA i el llistat del directori
>> Test f1.txt f2.txt
$ echo '$CADENA *'      # mostra la cadena literalment
>> $CADENA *
$ echo "$CADENA *"      # només substitueix el valor de la CADENA
>> Test *

$ which sort
>> /usr/bin/sort
$ ls -l `which sort`    # utilitza el resultat de which sort com a paràmetre de ls -l.
#            ↓          Es substitueix `which sort` pel resultat de la comanda
$ ls -l /usr/bin/sort
>> -rwxr-xr-x 1 root root 113120 Jan 18 2018 /usr/bin/sort
```

## Comandos utiles

### echo

Para enviar texto por la salida estándard. Con el parámetro `-e` se podrán usar carácteres especiales como saltos de línea (\n) y tabulaciones (\t).

```bash
$ echo -e "Hello\nWorld"
>> Hello
>> World
```

### sleep

Para "parar" la ejecución durante el número de segundos que se especifiquen como parámetro.

```bash
sleep X
```

### read

Para leer texto de la entrada estándard y asignarlo a una variable (no hace falta definir la variable previamente). Con el parámetro `-p` se podrá definir una prompt, y con `-s` el texto introducido por el usuario será 'secreto'.

```bash
read -p "Mensaje para el usuario" variableDefinida
```

### $(( expresión matemática ))

Para procesar expresiones aritméticas y lógicas.

```bash
$ echo "Suma = $(( 1 + 3 ))"
>> Suma = 4
```

### test

Para evaluar si una expresión es cierta o falsa.

No muestra nada por la salida estándard. Se puede acceder al resultado desde la variable `$?`, que contiene el resultado de la última operación (0 si es exito, cualquier otro número si ha habido errores). Se puede modificar/añadir un codigo de salida con `exit <codigo>`.

- **Ficheros**
  - Existe? `-e`
  - Es un fichero? `-f`
  - Es un directorio? `-d`
- **Strings**
  - Son iguales? `=`
  - Está vacía? `-z`
- **Números**
  - equal (=) `-eq`
  - not equal (!=) `-ne`
  - lower than (<) `-lt`
  - lower than or equal (<=) `-le`
  - greather than (>) `-gt`
  - greather than or equal (>=) `-ge`
- **Permisos**
  - pemisos de ejecución `-x`
  - pemisos de escritura `-w`
  - pemisos de lectura `-r`
- **Operaciones lógicas**
  - AND `-a`
  - OR `-o`

```bash
test 5 -ge 1
if [ $? = 0 ] ;
  then echo "5 es igual o mayor a 1"
  else echo "5 no es igual ni mayor a 1"
fi

test -d /etc && echo "Es un directorio" || echo "No es un directorio"
```

## Variables de entorno

- `$HOME`: Carpeta personal del usuario.
- `$USER`: Nombre del usuario.
- `$UID`: UID del usuario.
- `$HOSTNAME`: Nombre de la máquina.

## Variables especiales

- `$?`: Evalua la salida de la operación previa.
- `$0`: Nombre del script.
- `$#`: Numero total de argumentos.
- `$1`, `$2`, ... `$9`: Parámetro X (número) del script.
- `$*`: Conjunto iterable de parámetros del script tratados como un string con separacion de espacios.
- `$@`: Conjunto iterable de parámetros del script tratados como elementos individuales en una lista.
- `$$`: PID del proceso actual.
- `$!`: PID del proceso en segundo plano

## Condicionales

### If

Se puede hacer un if con diferentes sintaxis: `if [ comparacion ]` o `if test comparacion`. Se entrara en el bloque si la salida de la comparacion/condicion es 0.

```bash
if [ condicion1 ]
then
 # entra en primer if
elif [[ condicion2 && condicion3 ]] # se utilizan tantos [] como condiciones haya (en este caso hay dos que se separan con un AND)
then
 # entra en elseif
else
  # entra en else
fi

# si solo se quiere usar una condicion
if [ condicion ]
then
 echo "condicion es cierta"
fi
```

### Case

Busca cuál es el primer patrón que cumple la variable indicada y ejecuta el bloque de código correspondiente. Un patrón cumplirá con las estructuras de regex, como por ejemplo `[Hh]ola` para comprobar si la variable es "hola" o "Hola", o bien `h*` para comprobar que empieze por dicha letra.

```bash
case $var in
 pattern1)
  #codigo a ejecutar
  ;; #break
 pattern2)
  #codigo a ejecutar
  ;; #break
 *)
  #codigo por defecto
  ;;
esac
```

## Estructuras de iteracion

### While

```bash
while condición
do
 #código a ejecutar
done
```

### Until

```bash
until condición
do
 #código a ejecutar
done
```

### For

Ejecutara un bloque de sentencias de forma iterativa. Puede iterar x numero de veces o iterar sobre una lista de elementos separados por espacios en blanco.

```bash
for (( c=1; c<=5; c++ ))
do
 echo "$c"
done

# o para iterar sobre una lista (for-each)

list="one two three"
for element in $list
do
  echo "$element"
done

# también se puede iterar sobre un rango

for i in {1..20..2} ; do  # iterar del 1 al 20 avanzando de dos en dos
 echo "Iteración $i"
done

# hasta se puede iterar sobre la salida de un comando

for file in `ls` ; do
 echo $file
done
```

## Otros comandos

### Grep

- `-oE 'regexExpression'`: Muestra la parte que coincide con el pratrón `-o`, y permite Regex extendido `-E`.
- `-q`: No printa nada por pantalla (quiet), pero devuelve el codigo `1` o `0`.

### Cut

- `-d 'delimitador'`: Añade un delimitador.
- `-f1`, `-f2`,...: Selecciona un campo.
- `-cX-X`: Indica que se cortará por carácteres y no por campos, específicamente de X a X carácter.

### Tar

- `tar czf "$file_name"`: Empaqueta el archivo y lo comprime.
- `tar xzf "$file_name"`: Extraer el archivo comprimido.
- `tar tzf "$file_name"`: Mostrar el contenido sin extraerlo.

### Date

`date` proporciona la fecha actual, para poder formatearla en año_mes_dia -> `$(date +%Y_%m_%d)`

También puede usarse `$(date '+Y_%m_%d')` para formatear la fecha.

### Creacion / modificacion de usuario y grupos

Es necesario hacer estos comandos con permisos de superusuario.

- `useradd -m -s /bin/bash -c "comentario" "nombre"`: Añade un usuario con comentario.
- `useradd -m -s /bin/bash -G "grupo" "nombre"`: Añade un usuario con grupo.
- `useradd -p "contraseña" "nombre"`: Añade un usuario con contraseña.
- `echo "name:password" | sudo chpasswd`: Modifica la contraseña del usuario.

- `groupadd nombre`: Crea un grupo

### Modificar ownership / permisos

```bash
sudo chown usuario:grupop1 /directorio1
sudo chmod 770 /directorio1
```

### Touch

- `touch file.txt`: Crea un fichero.
- `echo "$word" >> "$file"`: Añade una lina al final del fichero.

### Split

- `split -n l/n --verbose file.txt`: Divide el fichero en n secciones y muestra los ficheros creados con `verbose`.

### Create directory from route

- `mkdir -p /my/route`: Crea un directorio desde la ruta.

### Redirigir mensaje de error (STDERR)

- `2> /dev/null`

### Redirigir todas las salidas (STDOUT y STDERR)

- `&> /dev/null`

### exec

- Reemplaza el proceso actual por el proceso que le pasas como argumento, como por ejemplo `sudo exec proceso parametros`

### Coger las letras específicas de una string

- `"${word:0:1}"`: Coge el primer caracter de la variable `word`.
- `"${word:1:3}"`: Coge, a partir del segundo caracter (índice 1) los siguientes 3 de la variable `word` (incluido el carácter con índice 1).

```bash
$ word="hola"
$ firstChar="${word:0:1}"
$ echo $firstChar
>> h

$ firstChar="${word:1:3}"
$ echo $firstChar
>> ola

$ firstChar="${word:0:2}"
$ echo $firstChar
>> ho
```

### Separar una cadena de texto por un delimitador y guardar en un array

```bash
$ string="uno,dos,tres,cuatro"
$ array=(${string//,/ }) #separar por comas y guardar en un array
$ echo ${array[0]}
>> uno

$ echo ${array[1]}
>> dos
```

### pwd

- `pwd`: Muestra el directorio actual.

---

**Autores**: [picuu](https://github.com/picuu) && [annacano0](https://github.com/annacano0)  
**Fecha**: 21 de abril de 2025  
