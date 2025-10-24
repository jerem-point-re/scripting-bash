<div align="center">

![bashLogo](bash_dark.svg)

# Introduction au scripting Bash
[it-connect.fr](https://www.it-connect.fr/cours-tutoriels/administration-systemes/scripting/bash/)

</div>

> [!TIP]
> Le Bash (Bourne Again SHell) est un interpréteur de commandes largement utilisé dans les systèmes Unix et Linux. Il permet d'exécuter des commandes, d'automatiser des tâches et de gérer des scripts.

## J. Premiers pas avec les commandes Bash

<div align="center">

| Commande | Option.s | Argument.s |
|:---|:---:|---:|
| ls | -a | /home/admin |
| mkdir |  | nouveau_repertoire |
| rm | -r | repertoire_a_supprimer |
| pwd |  |  |
| cd |  | /chemin/vers/repertoire |
| touch |  | fichier.txt |
| man |  | ls |

</div>

> [!NOTE]
> Pour les options, il y a souvent une version courte (ex: -R) et une version longue (ex: --recurse)

> [!WARNING]
> Les commandes Bash sont sensibles à la casse. Par exemple, -r et -R sont deux options différentes

<div align="right">

### a. Exemples de commandes

</div>

```bash
pwd
```
> Affiche le répertoire de travail actuel.

> [!TIP]
> `~` représente le répertoire personnel de l'utilisateur actuel.

```bash
cd /chemin/vers/repertoire
```
> Change le répertoire de travail actuel vers celui spécifié.

> [!TIP]
> Utilisez `cd ..` pour revenir au répertoire parent. Puis `cd ~` pour revenir au répertoire personnel de l'utilisateur actuel.

```bash
ls
```
> Liste les fichiers et répertoires dans le répertoire actuel.

> [!TIP]
> Utilisez `ls -l` pour une liste détaillée avec des informations supplémentaires.

```bash
ls -l /chemin/vers/repertoire
```
> Liste les fichiers et répertoires dans le répertoire spécifié avec des détails.

```bash
mkdir nouveau_repertoire
```
> Crée un nouveau répertoire nommé "nouveau_repertoire".

```bash
mkdir ~/nouveau_repertoire/{"docs","images","videos"}
```
> Crée plusieurs répertoires ("docs", "images", "videos") dans "nouveau_repertoire" situé dans le répertoire personnel de l'utilisateur.

```bash
touch fichier.txt
```
> Crée un nouveau fichier vide nommé "fichier.txt".

> [!TIP]
> Vous pouvez créer plusieurs fichiers en une seule commande, par exemple : `touch fichier1.txt fichier2.txt fichier3.txt` et aussi `touch fichier{1..5}.txt` pour créer fichier1.txt à fichier5.txt.

> [!WARNING]
> Si le fichier existe déjà, la commande `touch` met simplement à jour son horodate de dernière modification.

> [!TIP]
> Pour créer un fichier avec du contenu, vous pouvez utiliser la redirection, par exemple : `echo "Bonjour le monde" > fichier.txt`.

> [!CAUTION]
> Utilisez `>>` au lieu de `>` pour ajouter du contenu à un fichier existant sans l'écraser.

> [!NOTE]
> `touch` peut également être utilisé pour créer des fichiers spéciaux comme des fichiers de périphériques ou des tubes nommés avec des options supplémentaires ou simplement des fichiers `.mp4`, `.md`, ...

```bash
rm fichier.txt
```
> Supprime le fichier nommé "fichier.txt".

> [!CAUTION]
> Soyez très prudent.e.s avec la commande `rm`, car elle supprime les fichiers de manière permanente et aussi si le chemin est incorrect, vous pourriez supprimer des fichiers importants et amener à des pertes de données voire le crash du système.

```bash
rm -r repertoire_a_supprimer
```
> Supprime le répertoire nommé "repertoire_a_supprimer" et tout son contenu de manière récursive.

> [!TIP]
> Pour supprimer le contenu d'un répertoire sans supprimer le répertoire lui-même, vous pouvez utiliser `rm -r repertoire_a_supprimer/*`.

<div align="right">

### b. Aide sur les commandes

</div>

```
commande --help
```
> Affiche l'aide pour une commande spécifique (ex. `ls --help`).

```bash
man commande
```
> Affiche le manuel d'une commande spécifique (ex. `man ls`).

> [!NOTE]
> Les pages de manuel peuvent être parcourues avec les touches fléchées, et vous pouvez quitter en appuyant sur q.

> [!TIP]
> Les 'man pages' sont disponibles sur Internet pour de nombreuses commandes, par exemple : https://man7.org/linux/man-pages/man1/ls.1.html

## IJ. Premier script Bash

```bash
touch mon_script.sh
```
> Crée un nouveau fichier de script nommé "mon_script.sh".

On peut éditer ce fichier avec un éditeur de texte comme `nano`, `vim`, ou `Visual Studio Code`.
Ici, on va utiliser l'éditeur de texte de la ligne de commande

```bash
nano mon_script.sh
```
> Ouvre le fichier "mon_script.sh" dans l'éditeur de texte `nano`.

Ensuite, on ajoute le contenu suivant au fichier :

```bash
#!/bin/bash

# Ceci est un commentaire
echo "Hello, World!"
```

> [!NOTE]
> La première ligne `#!/bin/bash` est appelée "shebang" et indique que le script doit être exécuté avec l'interpréteur Bash.

Après avoir enregistré et fermé le fichier (dans `nano`, on fait `CTRL + O` pour enregistrer -- puis Entrée, puis `CTRL + X` pour quitter), on doit rendre le script exécutable avec la commande suivante :

```bash
chmod +x mon_script.sh
```
> Rend le script "mon_script.sh" exécutable.

Enfin, on peut exécuter le script avec la commande suivante :

```bash
./mon_script.sh
```
> Exécute le script "mon_script.sh".

## IIJ. Exemple de script un peu plus avancé

> Ce script effectue un ping vers une adresse IP spécifiée et affiche si le serveur est en ligne ou non.

```bash
#!/bin/bash

# Définir l'adresse du serveur sur lequel effectuer le ping
SERVEUR=192.168.1.254
# La déclaration d'une variable se fait sans espace autour du signe `=` et sans le préfixe `$`

echo "Ping du serveur $SERVEUR ..."
# Envoyer 2 pings au serveur correspondant à la varialbe $SERVEUR
ping -c 2 $SERVEUR > /dev/null
# `>` redirige la sortie standard vers /dev/null pour ne pas afficher le résultat du ping dans le terminal

# Déterminer si le serveur est en ligne ou pas
if [ $? -ne 0 ]
# `$?` contient le code de retour de la DERNIÈRE commande exécutée (ici, le ping), `-ne` signifie "not equal" (différent de), et `0` signifie succès (pas d'erreur)
then
# si le code de retour du ping n'est pas égal à 0, cela signifie que le ping a échoué, on retourne :
  echo "Erreur - Le serveur $SERVEUR n'a pas répondu au ping !"
else
# sinon, si le code de retour est égal à 0, cela signifie que le ping a réussi, on retourne :
  echo "PONG - Le serveur $SERVEUR est en ligne."
fi
# Le `fi` marque la fin de la structure conditionnelle `if`
```

## IV. Ressources supplémentaires : la pipeline

> La pipeline permet de chaîner plusieurs commandes ensemble en utilisant le symbole `|`. La sortie standard (stdout) de la première commande devient l'entrée standard (stdin) de la seconde commande, et ainsi de suite.

<div align="right">

### a. Exemple de pipeline

</div>

```bash
ls -l /var/log | grep "error" | sort
```

- `ls -l /var/log` : Liste les fichiers dans le répertoire `/var/log` avec des détails.
- `grep "error"` : Filtre les lignes contenant le mot "error".
- `sort` : Trie les lignes en ordre alphabétique.

> [!NOTE]
> Vous pouvez ajouter autant de commandes que vous le souhaitez dans une pipeline, en les séparant par des |. Chaque commande traite les données reçues de la commande précédente.

<div align="right">

### b. Autre exemple avec notre script de ping

</div>

```bash
cat ping_script.sh | grep "echo" | wc -l
```
- `cat ping_script.sh` : Affiche le contenu du fichier `ping_script.sh`.
- `grep "echo"` : Filtre les lignes contenant le mot "echo".
- `wc -l` : Compte le nombre de lignes (wc : word count, -l : que les lignes).

> [!TIP]
> On peut écrire cette commande plus simplement :

```bash
grep "echo" ping_script.sh | wc -l
```

## V. Les conditions `if`, `else`, `elif`

```bash
if [ condition ]
then
    # Code à exécuter si la condition est vraie
fi
```

> [!NOTE]
> La condition à évaluer est spécifiée entre [ ], avec des espaces autour des crochets.

> [!WARNING]
> L'instruction if est toujours suivie par l'instruction then et celle-ci doit être placée sur une nouvelle ligne ou après un point-virgule (;).

> L'instruction `fi` détermine la fin de la structure conditionnelle.

<div align="right">

### a. Exemples avec `if`

</div>

```bash
#!/bin/bash

if [ -d /chemin/vers/repertoire ]
# Vérifie si le répertoire "/chemin/vers/repertoire" existe.
then
    echo "Le répertoire existe."
fi
```

```bash
#!/bin/bash

if [ -f fichier.txt ]
# Vérifie si "fichier.txt" existe et est un fichier régulier.
then
    echo "Le fichier existe."
fi
```

> [!TIP]
> Les options courantes pour les conditions incluent :
> - `-d` : vérifie si un répertoire existe.
> - `-f` : vérifie si un fichier régulier existe.
> - `-s` : vérifie si un fichier existe et n'est pas vide.
> - `-e` : vérifie si un fichier ou répertoire existe.
> - `-r` : vérifie si un fichier est lisible.
> - `-w` : vérifie si un fichier est modifiable.
> - `-x` : vérifie si un fichier est exécutable.

```bash
#!/bin/bash

if [ -s fichier.txt ]
# Vérifie si "fichier.txt" existe et n'est pas vide.
then
    echo "Le fichier n'est pas vide."
fi
```

```bash
#!/bin/bash

nombre=23
# Déclare une variable nommée "nombre" avec la valeur 15 qui sera évaluée.

if [ $nombre -gt 10 ]
# Vérifie si le nombre est supérieur à 10.
then
    echo "Le nombre est supérieur à 10."
fi
```

<div align="right">

### b. Exemples avec `if`...`else`

</div>

```bash
#!/bin/bash

# Valeur à évaluer
nombre=7

# Vérifier si le nombre est pair ou impair
if [ $((nombre % 2)) -eq 0 ]
then
    echo "Le nombre $nombre est pair."
else
    echo "Le nombre $nombre est impair."
fi
```

<div align="right">

### c. Exemples avec `if`...`elif`...`else`

</div>

```bash
#!/bin/bash

# Définir l'heure
heure=$(date +%H)

if [ $heure -lt 12 ]
then
    echo "Bonjour !"
elif [ $heure -lt 18 ]
then
    echo "Bon après-midi !"
else
    echo "Bonsoir !"
fi
```

## VJ. Maîtriser les boucles : `for`, `while`, `until`

<div align="right">

### a. La boucle `for`

</div>

```bash
for i in {1..5}
do
    echo "Itération numéro $i"
done
```

> [!NOTE]
> Les deux exemples ci-contre montrent deux façons différentes d'écrire une boucle for en Bash. La première utilise une séquence générée par {1..5}, tandis que la seconde utilise une syntaxe C-like avec for ((...)). Les deux boucles produisent le même résultat, c'est-à-dire afficher les numéros d'itération de 1 à 5.

```bash
for ((i=1; i<=5; i++))
do
    echo "Itération numéro $i"
done
```

```bash
for fichier in /chemin/vers/repertoire/*
do
    echo "$fichier : $(cat $fichier | wc -l) ligne.s"
done
```
> Ce script parcourt tous les fichiers dans le répertoire spécifié et affiche le nom de chaque fichier ainsi que le nombre de lignes qu'il contient.

```bash
for fichier in *.txt
do
    mv "nouveau_$fichier"
done
```
> Ce script renomme tous les fichiers `.txt` dans le répertoire actuel en ajoutant le préfixe "nouveau_" à chaque nom de fichier.

```bash
#!/bin/bash
services=("nginx" "mysql" "ssh" "cron")

for service in "${services[@]}"
do
    systemctl status $service > /dev/null
    if [ $? -eq 0 ]
    then
        echo "Le service $service est en cours d'exécution."
    else
        echo "Le service $service n'est pas en cours d'exécution."
    fi
done
```
> Ce script vérifie l'état de plusieurs services système (nginx, mysql, ssh, cron) et affiche s'ils sont en cours d'exécution ou non.

```bash
#!/bin/bash
services=("ssh" "apache2")

for service in "${services[@]}"
do
    systemctl restart $service
    echo "Le service $service a été redémarré."
done
```
> Ce script redémarre les services "ssh" et "apache2" et affiche un message de confirmation pour chaque service redémarré.

> [!WARNING]
> Assurez-vous d'avoir les permissions nécessaires pour redémarrer les services système.

> [!TIP]
> Exécutez le script avec sudo si nécessaire : `sudo ./votre_script.sh`

<div align="right">

### b. Avec `if` et `else` dans la boucle `for`

</div>

```bash
#!/bin/bash

services=("ssh" "apache2")
# Déclare un tableau de services à vérifier

for service in ${services[@]}
# Parcourt chaque service dans le tableau
do
  echo "Vérification de l'état du service $service"
# Ici, la condtion du if n'est pas entre [ ], car on utilise une commande et cela renverrait une erreur
  if systemctl is-active --quiet $service
# Utilise systemctl pour vérifier si le service est actif, l'option --quiet supprime la sortie standard
  then
    echo "Le service $service est actif."
# Si le service est actif, affiche un message correspondant, sinon affiche un message d'inactivité
  else
    echo "Le service $service est inactif."
  fi
done
```

## VIJ. La boucle `while`

```bash
compteur=1
while [ $compteur -le 5 ]
do
    echo "Itération numéro $compteur"
    ((compteur++))
done
echo "Boucle terminée."
```
> Ce script utilise une boucle `while` pour afficher les numéros d'itération de 1 à 5. La variable `compteur` est initialisée à 1 et incrémentée à chaque itération jusqu'à ce qu'elle dépasse 5.

<div align="right">

### a. Incluant plusieurs conditions

</div>

```bash
#!/bin/bash

Nombre1=1
Nombre2=1

while [ $Nombre1 -le 10 ] && [ $Nombre2 -le 5 ]
do
    echo "Nombre1 : $Nombre1 - Nombre2 : $Nombre2"
    ((Nombre1++))
    ((Nombre2++))
done
```
> Ce script utilise une boucle `while` pour afficher les valeurs de deux variables, `Nombre1` et `Nombre2`, tant que `Nombre1` est inférieur ou égal à 10 et `Nombre2` est inférieur ou égal à 5. Les deux variables sont incrémentées à chaque itération.

<div align="right">

### b. Avec `if` et `else` dans la boucle `while`

</div>

```bash
#!/bin/bash

while [ -z $username ]
do
  echo "Veuillez saisir votre identifiant : "
  read username
done

echo "Identifiant renseigné : $username"
```
> Ce script utilise une boucle `while` pour demander à l'utilisateur de saisir son identifiant tant que la variable `username` est vide. La commande `read` lit l'entrée de l'utilisateur et la stocke dans la variable `username`. Une fois que l'utilisateur a saisi un identifiant, le script affiche l'identifiant sélectionné.

> [!NOTE]
> L'option `-z` dans la condition `[ -z $username ]` vérifie si la variable `username` est vide.

<div align="right">

### c. Exemple avec une boucle infinie et une condition de sortie

</div>

```bash
#!/bin/bash
while true
do
    echo "Tapez 'exit' pour quitter ou appuyez sur Entrée pour continuer."
    read input
    if [ "$input" == "exit" ]
    then
        echo "Sortie de la boucle."
        break
    else
        echo "Vous avez appuyé sur Entrée. La boucle continue."
    fi
done
```
> Ce script crée une boucle infinie qui demande à l'utilisateur de taper 'exit' pour quitter la boucle ou d'appuyer sur Entrée pour continuer. Si l'utilisateur tape 'exit', la boucle se termine grâce à l'instruction `break`. Sinon, la boucle continue.

<div align="right">

### d. Exemple avec une boucle `while` pour lire un fichier ligne par ligne

</div>

```bash
#!/bin/bash
fichier="mon_fichier.txt"
while IFS= read -r ligne
do
    echo "Ligne lue : $ligne"
done < "$fichier"
```
> Ce script lit un fichier nommé "mon_fichier.txt" ligne par ligne et affiche chaque ligne lue. La variable `IFS` est utilisée pour préserver les espaces blancs dans chaque ligne.

<div align="right">

### e. Exemple avec une boucle `while` pour bannir des adresses IP dans CrowdSec

</div>

```bash
#!/bin/bash

# Lire le fichier ligne par ligne avec la boucle While
while IFS= read -r ip
do
   # Bannir l'adresse IP dans CrowdSec (ajout décision)
   echo "cscli decisions add --ip $ip"
   cscli decisions add --ip "$ip"

done < "/home/flo/ip.txt"
```
> Ce script lit un fichier nommé "ip.txt" ligne par ligne, où chaque ligne contient une adresse IP. Pour chaque adresse IP lue, il exécute la commande `cscli decisions add --ip` pour bannir cette adresse IP dans CrowdSec.

> [!TIP]
> CrowdSec est un système de sécurité collaboratif qui utilise des données de plusieurs sources pour détecter et bloquer les comportements malveillants sur les systèmes informatiques.

## VIIJ. La boucle `until`

```bash
compteur=1
until [ $compteur -gt 5 ]
do
    echo "Itération numéro $compteur"
    ((compteur++))
done
echo "Boucle terminée."
```
> Ce script utilise une boucle `until` pour afficher les numéros d'itération de 1 à 5. La variable `compteur` est initialisée à 1 et incrémentée à chaque itération jusqu'à ce qu'elle soit supérieure à 5.

> [!WARNING]
> La boucle `until` continue tant que la condition est fausse, contrairement à la boucle `while` qui continue tant que la condition est vraie.

<div align="right">

### a. Exemple avec une boucle `until` pour lire un fichier ligne par ligne

</div>

```bash
#!/bin/bash
fichier="mon_fichier.txt"
ligne=""
exec 3< "$fichier"
until [ -z "$ligne" ]
do
    read -r ligne <&3
    if [ -n "$ligne" ]; then
        echo "Ligne lue : $ligne"
    fi
done
exec 3<&-
```
> Ce script lit un fichier nommé "mon_fichier.txt" ligne par ligne en utilisant une boucle `until`. La boucle continue jusqu'à ce que la variable `ligne` soit vide, ce qui indique la fin du fichier. Chaque ligne lue est affichée à l'écran.

> [!NOTE]
> L'utilisation de `exec 3< "$fichier"` ouvre le fichier pour la lecture sur le descripteur de fichier 3, et `exec 3<&-` ferme ce descripteur à la fin du script.

## IX. La saisie utilisateur

```bash
#!/bin/bash
echo "Veuillez entrer votre prénom : "
read prenom
echo "Bonjour, $prenom !"
```
> Ce script demande à l'utilisateur de saisir son prénom, puis affiche un message de bienvenue avec le prénom saisi.

> [!TIP]
> La commande s'écrit également `read -p "Veuillez entrer votre prénom : " prenom` pour afficher l'invite de saisie sur la même ligne.

<div align="right">

### a. Saisie avec une valeur par défaut

</div>

```bash
#!/bin/bash
default_prenom="Utilisateur"
echo "Veuillez entrer votre prénom (appuyez sur Entrée pour '$default_prenom') : "
read prenom
prenom=${prenom:-$default_prenom}
echo "Bonjour, $prenom !"
```
> Ce script demande à l'utilisateur de saisir son prénom. Si l'utilisateur appuie simplement sur Entrée sans saisir de prénom, la variable `prenom` prend la valeur par défaut "Utilisateur".

<div align="right">

### b. Saisie avec une validation

</div>

```bash
#!/bin/bash
while true
do
    echo "Veuillez entrer votre âge (entre 1 et 120) : "
    read age
    if [[ $age =~ ^[0-9]+$ ]] && [ $age -ge 1 ] && [ $age -le 120 ]
    then
        echo "Âge valide : $age ans."
        break
    else
        echo "Âge invalide. Veuillez réessayer."
    fi
done
```
> Ce script demande à l'utilisateur de saisir son âge et valide que l'entrée est un nombre compris entre 1 et 120. Si l'entrée est invalide, le script demande à l'utilisateur de réessayer jusqu'à ce qu'une valeur valide soit saisie.

<div align="right">

### c. Saisie avec une limite de temps

</div>

```bash
#!/bin/bash
echo "Veuillez entrer votre prénom (vous avez 5 secondes) : "
read -t 5 prenom
if [ -z "$prenom" ]
then
    echo "Temps écoulé ! Aucun prénom saisi."
else
    echo "Bonjour, $prenom !"
fi
```
> Ce script demande à l'utilisateur de saisir son prénom avec une limite de temps de 5 secondes. Si l'utilisateur ne saisit rien dans ce délai, un message indique que le temps est écoulé.

<div align="right">

### d. Saisie de plusieurs informations

</div>

```bash
#!/bin/bash
echo "Veuillez entrer votre prénom et votre nom : "
read prenom nom
echo "Bonjour, $prenom $nom !"
echo "Votre prénom est $prenom et votre nom est $nom."
```
> Ce script demande à l'utilisateur de saisir son prénom et son nom en une seule ligne. Il affiche ensuite un message de bienvenue ainsi que les informations saisies.

<div align="right">

### e. Saisie avec une boucle `while`

</div>

```bash
#!/bin/bash
while [ -z $username ]
do
  echo "Veuillez saisir votre identifiant : "
  read username
done
echo "Identifiant renseigné : $username"
```
> Ce script utilise une boucle `while` pour demander à l'utilisateur de saisir son identifiant tant que la variable `username` est vide. La commande `read` lit l'entrée de l'utilisateur et la stocke dans la variable `username`. Une fois que l'utilisateur a saisi un identifiant, le script affiche l'identifiant sélectionné.

<div align="right">

### f. Saisie masquée pour les mots de passe

</div>

```bash
#!/bin/bash
echo "Veuillez entrer votre mot de passe : "
read -s password
echo "Mot de passe saisi."
```
> Ce script demande à l'utilisateur de saisir son mot de passe de manière masquée (sans afficher les caractères saisis), puis affiche un message confirmant la saisie du mot de passe.

> [!TIP]
> L'option `-s` dans la commande `read` permet de masquer l'entrée de l'utilisateur, ce qui est utile pour les mots de passe ou autres informations sensibles.

## X. Création d'un menu interactif

```bash
#!/bin/bash
while true
do
    echo "Menu Principal"
    echo "1. Afficher la date et l'heure"
    echo "2. Afficher l'espace disque"
    echo "3. Afficher les utilisateurs connectés"
    echo "4. Quitter"
    echo -n "Veuillez choisir une option (1-4) : "
    read choix
    case $choix in
        1)
            date
            ;;
        2)
            df -h
            ;;
        3)
            who
            ;;
        4)
            echo "Au revoir !"
            break
            ;;
        *)
            echo "Option invalide. Veuillez réessayer."
            ;;
    esac
    echo ""
done
```
> Ce script crée un menu interactif qui permet à l'utilisateur de choisir parmi plusieurs options : afficher la date et l'heure, afficher l'espace disque, afficher les utilisateurs connectés, ou quitter le menu. La boucle `while` continue à afficher le menu jusqu'à ce que l'utilisateur choisisse de quitter en sélectionnant l'option 4.

<div align="right">

### a. Exemple avec la commande `select`

</div>

```bash
#!/bin/bash

PS3="Quelle tâche souhaitez-vous exécuter ? "
options=("Voir la version du système" "Voir l'espace disque" "Voir la configuration IP" "Voir la date du dernier redémarrage" "Quitter")

select choix in "${options[@]}"; do
    case $REPLY in
        1)
            echo "Version du système :"
            uname -a
            ;;
        2)
            echo "Espace disque :"
            df -h
            ;;
        3)
            echo "Configuration IP :"
            ip a
            ;;
        4)
            echo "Date du dernier redémarrage :"
            who -b
            ;;
        5)
            echo "Exit"
            break
            ;;
        *)
            echo "Option invalide, veuillez réessayer !"
            ;;
    esac
done
```
> Ce script utilise la commande `select` pour créer un menu interactif avec plusieurs options. L'utilisateur peut choisir une tâche à exécuter en entrant le numéro correspondant. Le script affiche ensuite les informations demandées ou quitte le menu si l'option "Quitter" est sélectionnée.

<div align="right">

### b. Exemple avec des couleurs dans le menu

</div>

```bash
#!/bin/bash

# Définir les couleurs avec des variables
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[0;33m'
NC='\033[0m' # Aucune couleur

PS3="Quelle tâche souhaitez-vous exécuter ? "
options=("Voir la version du système" "Voir l'espace disque" "Voir la configuration IP" "Voir la date du dernier redémarrage" "Quitter")

select choix in "${options[@]}"; do
    case $REPLY in
        1)
            echo -e "${GREEN}Version du système :${NC}"
            uname -a
            ;;
        2)
            echo -e "${GREEN}Espace disque :${NC}"
            df -h
            ;;
        3)
            echo -e "${GREEN}Configuration IP :${NC}"
            ip a
            ;;
        4)
            echo -e "${GREEN}Date du dernier redémarrage :${NC}"
            who -b
            ;;
        5)
            echo -e "${YELLOW}Exit${NC}"
            break
            ;;
        *)
            echo -e "${RED}Option invalide, veuillez réessayer !${NC}"
            ;;
    esac
done
```
> Ce script est similaire au précédent, mais il ajoute des couleurs aux messages affichés dans le menu. Les variables `RED`, `GREEN`, `YELLOW`, et `NC` sont utilisées pour définir les codes de couleur ANSI. La commande `echo -e` est utilisée pour interpréter les séquences d'échappement et afficher les messages en couleur.

<div align="right">

### c. Exemple avec une boucle `while` et un `read` pour le menu interactif

</div>

```bash
#!/bin/bash

# Définir les couleurs avec des variables
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[0;33m'
NC='\033[0m' # Aucune couleur

while true; do
    echo -e "${YELLOW}Menu :"
    echo -e "${GREEN}1) Voir la version du système"
    echo -e "2) Voir espace disque"
    echo -e "3) Voir configuration IP"
    echo -e "4) Voir la date du dernier redémarrage"
    echo -e "5) Quitter"
    echo -e "${YELLOW}Choisissez une option : ${NC}"
    read choix

    case $choix in
        1)
            echo -e "${GREEN}Version du système :${NC}"
            uname -a
            ;;
        2)
            echo -e "${GREEN}Espace disque :${NC}"
            df -h
            ;;
        3)
            echo -e "${GREEN}Configuration IP :${NC}"
            ip a
            ;;
        4)
            echo -e "${GREEN}Date du dernier redémarrage :${NC}"
            who -b
            ;;
        5)
            echo -e "${YELLOW}Exit${NC}"
            break
            ;;
        *)
            echo -e "${RED}Option invalide, veuillez réessayer !${NC}"
            ;;
    esac
done
```
> Ce script crée un menu interactif en utilisant une boucle `while` et la commande `read` pour capturer l'entrée de l'utilisateur. Il affiche les options du menu avec des couleurs et exécute la tâche sélectionnée par l'utilisateur. La boucle continue jusqu'à ce que l'utilisateur choisisse de quitter en sélectionnant l'option 5.

## XJ. Fonctions Bash

```bash
#!/bin/bash
ma_fonction() {
    echo "Ceci est une fonction Bash."
}
ma_fonction
```
> Ce script définit une fonction nommée `ma_fonction` qui affiche un message lorsqu'elle est appelée. La fonction est ensuite appelée à la fin du script.

<div align="right">

### a. Exemples de fonctions avec des arguments

</div>

```bash
#!/bin/bash
additionner() {
    resultat=$(( $1 + $2 ))
    echo "Le résultat de l'addition est : $resultat"
}
additionner 5 10
```
> Ce script définit une fonction nommée `additionner` qui prend deux arguments, les additionne et affiche le résultat. La fonction est appelée avec les valeurs 5 et 10, et le résultat de l'addition est affiché.

<div align="right">

### b. Autres exemples de fonctions

</div>

```bash
#!/bin/bash
saluer() {
    nom=$1
    echo "Bonjour, $nom !"
}
saluer "Alice"
```
> Ce script définit une fonction nommée `saluer` qui prend un argument (le nom) et affiche un message de bienvenue. La fonction est appelée avec le nom "Alice".

<div align="right">

### c. Fonction avec valeur de retour

</div>

```bash
#!/bin/bash
factorielle() {
    if [ $1 -le 1 ]
    then
        echo 1
    else
        prev=$(factorielle $(( $1 - 1 )))
        echo $(( $1 * prev ))
    fi
}
resultat=$(factorielle 5)
echo "La factorielle de 5 est : $resultat"
```
> Ce script définit une fonction récursive nommée `factorielle` qui calcule la factorielle d'un nombre donné. La fonction est appelée avec la valeur 5, et le résultat est affiché.

<div align="right">

### d. Fonction avec variable locale

</div>

```bash
#!/bin/bash
afficher_message() {
    local message=$1
    echo "Message : $message"
}
afficher_message "Ceci est un message local."
```
> Ce script définit une fonction nommée `afficher_message` qui utilise une variable locale `message` pour stocker l'argument passé à la fonction. La fonction affiche ensuite le message. L'utilisation de `local` garantit que la variable `message` n'est accessible qu'à l'intérieur de la fonction.
> [!TIP]
> L'utilisation de variables locales dans les fonctions aide à éviter les conflits de noms avec d'autres variables dans le script principal.

<div align="right">

### e. Fonction avec plusieurs arguments

</div>

```bash
#!/bin/bash
calculer_moyenne() {
    local somme=0
    local count=$#
    for nombre in "$@"
    do
        somme=$((somme + nombre))
    done
    local moyenne=$((somme / count))
    echo "La moyenne est : $moyenne"
}
calculer_moyenne 10 20 30 40 50
```
> Ce script définit une fonction nommée `calculer_moyenne` qui calcule la moyenne de plusieurs nombres passés en arguments. La fonction utilise une boucle `for` pour additionner les nombres et calcule la moyenne en divisant la somme par le nombre d'arguments.

## XIJ. Gestion des erreurs et débogage

```bash
#!/bin/bash
set -e
# Active le mode "exit on error" pour arrêter le script en cas d'erreur
echo "Début du script."
# Commande qui pourrait échouer
cp fichier_inexistant.txt /destination/
echo "Cette ligne ne sera pas exécutée si la commande précédente échoue."
```
> Ce script utilise `set -e` pour activer le mode "exit on error", ce qui signifie que le script s'arrêtera immédiatement si une commande échoue. Cela aide à prévenir les erreurs silencieuses et facilite le débogage.

> [!TIP]
> Vous pouvez également utiliser `set -x` pour activer le mode de débogage, qui affiche chaque commande avant son exécution.

```bash
#!/bin/bash
trap 'echo "Une erreur est survenue à la ligne $LINENO."' ERR
# Définit un gestionnaire d'erreurs qui affiche un message avec le numéro de ligne en cas d'erreur
echo "Début du script."
# Commande qui pourrait échouer
cp fichier_inexistant.txt /destination/
echo "Fin du script."
```
> Ce script utilise la commande `trap` pour définir un gestionnaire d'erreurs qui affiche un message avec le numéro de ligne où l'erreur s'est produite. Cela permet de mieux comprendre où le script a échoué.

> [!TIP]
> La variable spéciale `$LINENO` contient le numéro de la ligne actuelle dans le script, ce qui est utile pour le débogage.
