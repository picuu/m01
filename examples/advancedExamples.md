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
