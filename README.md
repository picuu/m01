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

Para poder ejecutar habrá que dar permisos al fichero creado `chmod u+x *.sh`

## Definicion de variables

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

## Comandos utiles

**echo** para enviar texto por la salida estándard.
```bash
echo "Hello World"
```

**sleep** para "parar" la ejecución durante el número de segundos que se especifiquen como parámetro.
```bash
sleep X
```

**read** para leer texto de la entrada estándard y asignarlo a una variable (no hace falta definir la variable previamente). Con el parámetro `-p` se podrá definir una prompt, y con `-s` el texto introducido por el usuario será 'secreto'.
```bash
read -p "Mensaje para el usuario" variableDefinida
```

**$(( expresión matemática ))** para procesar expresiones aritméticas y lógicas.

**test** para evaluar si una expresión es cierta o falsa.

No muestra nada por la salida estándard. Se puede acceder al resultado desde la variable `$?`, que contiene el resultado de la última operación (0 si es exito, cualquier otro número si ha habido errores). Se puede modificar/añadir un codigo de salida con `exit <codigo>`.

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

## Variables especiales

* `$?`: Evalua la salida de la operación previa.
* `$0`: Nombre del script.
* `$#`: Numero total de argumentos.
* `$1`, `$2`, ... `$9`: Parámetro X (número) del script.
* `$*`: Conjunto iterable de parámetros del script tratados como un string con separacion de espacios.
* `$@`: Conjunto iterable de parámetros del script tratados como elementos individuales en una lista.
* `$$`: PID del proceso actual.
* `$!`: PID del proceso en segundo plano

## Condicionales

### If

Se puede hacer un if con diferentes sintaxis: `if [ comparacion ]` o ` if test comparacion`. Se entrara en el bloque si la salida de la comparacion/condicion es 0.

```bash
if [ condicion1 ]
then
 # entra en primer if
elif [ condicion2 ]
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

Busca cuál es el primer patron que cumple la variable indicada y ejecuta el bloque de codigo correspondiente

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
while condicion
do
 #codigo a ejecutar
done
```

### Until


```bash
until condicion
do
 #codigo a ejecutar
done
```

### For


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
```

## Otros comandos

añadir grep, cut, tar, y uso de variables

---

**Autores**: [picuu](https://github.com/picuu) & [annacano0](https://github.com/annacano0)  
**Fecha**: 21 de abril de 2025  
