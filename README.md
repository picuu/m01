# Resumen UF2 NF3 - Automatización (scripting)

## Creación de un script

Se recomienda que la extensión del fichero sea `.sh`.

Para que el interprete sepa con que shell debe ejecutar el proceso, la primera línea del archivo deberá ser `#!/bin/bash`.

## Definición de variables

Para definir una variable se utiliza el nombre de la variable, el símbolo igual y el contenido. Por ejemplo `nombre_variable="contenido string"`. No debe haber espacios entre el nombre de la variable, el símbolo de asignación y el contenido.

No se deben declarar previamente ni asignar un tipo.

## Comillas

* **Comillas simples**: interpreta literalmente la cadena de carácteres que contiene.
* **Comillas dobles**: interpreta el texto y las variables ($).
* **Comillas inversas**: se ejecuta el comando que contiene. Es lo mismo a `$(comando)`.

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

## Comandos útiles

**echo** para enviar texto por la salida estándard.

**sleep** para "parar" la ejecución durante el número de segundos que se especifiquen como parámetro.

**read** para leer texto de la entrada estándard y asignarlo a una variable (no hace falta definir la variable previamente).

**$(( expresión matemática ))** para procesar expresiones aritméticas y lógicas.

**test** para evaluar si una expresión es cierta o falsa.

No muestra nada por la salida estándard. Se puede acceder al resultado desde la variable `$?`, que contiene el resultado de la última operación (0 si es exito, cualquier otro número si ha habido errores).

  * **Ficheros**
    * Existe? `-e`
    * Es un fichero? `-f`
    * Es un directorio? `-d`
  * **Strings**
    * Son iguales? `=`
    * Está vacía? `-z`
  * **Números**
    * equal (=) `-eq`
    * not equal (!=) `-ne`
    * lower than (<) `-lt`
    * lower than or equal (<=) `-le`
    * greather than (>) `-gt`
    * greather than or equal (>=) `-ge`
  * **Permisos**
    * pemisos de ejecución `-x`
    * pemisos de escritura `-w`
    * pemisos de lectura `-r`
  * **Operaciones lógicas**
    * AND `-a`
    * OR `-o`  

## Variables de entorno

* `$HOME`: Carpeta personal del usuario.
* `$USER`: Nombre del usuario.
* `$UID`: UID del usuario.
* `$HOSTNAME`: Nombre de la máquina.


añadir $0 (nombre del fichero)
