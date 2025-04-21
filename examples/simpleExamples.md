# Ejemplos simples

## Cadenas
```bash
string2="string literal"
echo 'holi'
echo "$string2"
echo "$(date)"

```

## Operaciones aritmeticas

```bash
num1=2
num2=4

echo "sum $num1 + $num2 = " $((num1+num2))
echo "resta $num1 - $num2 = " $((num1-num2))
echo "div $num2 / $num1 = " $((num2/num1))
echo "mult $num1 * $num2 = " $((num1*num2))
echo "modul $num2 % $num1 = " $((num2%num1))
echo "potencia $num1 ** $num2 =" $((num1**num2))
```

## Condicionales


```bash
string1="hola"
string2="holi"

if [ "$string1" = "$string2" ]; then
    echo "$string1 y $string2 son iguales"
else
    echo "$string1 y $string2 no son iguales"
fi
```

```bash
read -p "Introduce un numero para saber si es par o impar" userNumber

if [[ "$userNumber" =~ ^[0-9]+$ ]]; then
    if [ $((userNumber % 2 )) -eq 0 ]; then
        echo "$userNumber es par"
    else
        echo "$userNumber no es par"
    fi
else
    echo "El valor introducido no es un numero positivo"
fi
```

## Estructuras iterativas

```bash
read -p "Introduce un numero apra hacer la cuenta atras: " userCounterLimit

counter=0
while [ $userCounterLimit -ge $counter ]
do
    echo "$counter"
    counter=$((counter + 1))
done
```

```bash
read -p "Introduce un numero apra hacer la cuenta atras: " userCounterLimit

echo "Bucle until: "

until [ "$userCounterLimit" -eq 0 ]  
do
    echo "$userCounterLimit"
    userCounterLimit=$((userCounterLimit - 1))
done
```

```bash
#iterar sobre los ficheros del directorio actual

for file in *; do
  echo "file: $file"
done

for file in $(ls); do
    echo "file: $file"
done
```

## Test

```bash
userDirectory="/etc"

if test -d "$userDirectory" && test -w "$userDirectory"; then
    echo "El directorio existe y tiene permisos de escritura"
else
    echo "El directorio no existe y/o no tiene permioss de escritura"
fi
```

## Formatear datos de un fichero

```bash
for line in $(cat /etc/passwd); do

    usuari=$(echo "$line" | cut -d: -f1)
    uid=$(echo "$line" | cut -d: -f3)
    gid=$(echo "$line" | cut -d: -f4)
    home=$(echo "$line" | cut -d: -f6)

    echo "Usuari: $usuari"
    echo "  UID: $uid"
    echo "  GID: $gid"
    echo "  Directori personal: $home"
    echo "----------------------------"

done
```
