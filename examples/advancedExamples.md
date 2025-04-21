# Advanced examples

## Crear ficheros y organizar palabras

```bash
words="digalo mansana alo manzana"

for word in $words; do
    initial=$(echo "$word" | cut -c1-1 )
    file="$initial.txt"
    if test -f $file; then
        echo "$file ya existe"
    else
        echo "Creando $file..."
        touch $file
    fi
    echo "$word" >> $file
done
```

## Get groups from user

```bash
read -p "Introdueix el nom d'usuari per veure a quins grups pertany: " userName

if ! grep -q "^$userName:" /etc/passwd; then
    echo "L'usuari '$userName' no existeix"
    exit 1
fi

userGID=$(grep "^$userName:" /etc/passwd | cut -d ':' -f 4)
primaryGroup=$(grep "^[^:]*:[^:]*:$userGID:" /etc/group | cut -d ':' -f 1)
secondaryGroups=$(grep -E ":.*\b$userName\b" /etc/group | cut -d ':' -f 1)

echo "Grups de l'usuari $userName:"
echo "Grup primari: $primaryGroup"

if [[ -n "$secondaryGroups" ]]; then
    echo "Grups secundaris: $secondaryGroups"
else
    echo "No pertany a cap grup secundari."
fi
```

## Get groups from user v2

```bash
username=$1

for line in `cat /etc/group | grep $username`; do
  cols=(${line//:/ })
  echo ${cols[0]}
done
```

## Delete files given by params with confirmation

```bash
for file in $*; do

  isValidInput=false
  userInput="n"

  while ! [ "$isValidInput" = "true" ]; do
    echo ;
    read -n1 -r -p "Do you want to delete *$file* ? (y/n): " key

    if [[ "$key" = "y" || "$key" = "n" ]]; then
      isValidInput=true
      userInput=$key
    else echo -e "\nInvalid input. Type 'y' (yes) or 'n' (no).\n"
    fi
  done

  echo ;

  if [ "$userInput" = "y" ]; then
    rm $file &> /dev/null
    test $? -eq 0 && echo "File *$file* removed successfully." || echo "File *$file* couldn't be removed."
  fi
done
```

## Slice a file in the given amount of equal slices

The `slice-` at the end of the command is to give a prefix to all sliced files.

```bash
file_name=$1
slices=$2

split --number=$slices $file_name slice-

echo "$file_name splitted in $slices parts:"
ls -l | grep -oE '(slice-.*)$'
```
