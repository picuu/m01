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
