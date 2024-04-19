# **Introduction**
Le swap (également appelé espace d’échange ou mémoire de pagination) est un mécanisme essentiel dans les systèmes d’exploitation, y compris Linux. Voici ce que vous devez savoir à son sujet :

**Qu’est-ce que le swap ? :** 

Le swap est un espace de stockage utilisé par le système Linux pour gérer les données internes et les applications en cours d’exécution. Il se présente sous la forme d’un espace dédié sur le disque dur, qui peut être soit un fichier, soit une partition spécifique appelée “partition de swap”. En général, cet espace sert de substitut à la mémoire physique (RAM). Lorsque la RAM est saturée, le système d’exploitation déplace temporairement des pages de mémoire inactives ou peu utilisées vers cette zone désignée du disque dur ou du SSD1.

**Types de fichiers d’échange sous Linux :**

Fichier d’échange temporaire : Il utilise généralement de l’espace disque fragmenté et ne réserve pas une partie spécifique du disque dur. Cela le rend adapté aux systèmes avec un espace disque limité.
Fichier d’échange permanent : Il occupe une section contiguë du disque dur, nécessitant plus d’espace que le fichier d’échange temporaire. Cependant, il est moins gourmand en ressources en termes d’E/S (entrées/sorties).

**Rôles du swap dans Linux :**

*Mémoire virtuelle :* Le swap fait partie intégrante de la mémoire virtuelle, qui combine la RAM et l’espace de swap. Les adresses de pages qui font la correspondance entre la mémoire virtuelle et la mémoire physique peuvent être stockées dans l’espace d’échange.
*Gestion de la mémoire :* Le swap permet aux processus d’utiliser plus de mémoire que celle physiquement disponible. Lorsque la RAM est pleine, le système décharge certaines données vers le swap pour libérer de l’espace.
*Besoin en mémoire :* Certaines applications gourmandes en mémoire peuvent nécessiter l’utilisation du swap pour gérer l’excès de demande de mémoire.

**Taille recommandée pour la partition swap :**

La taille dépend de la quantité de RAM et de l’utilisation prévue du système. En général, une taille équivalente à deux fois la RAM est souvent recommandée, mais cela peut varier.
Si vous avez suffisamment d’espace disque, optez pour un fichier d’échange permanent plutôt qu’un fichier temporaire pour des performances optimales.

En résumé, le swap est un élément essentiel pour la gestion de la mémoire dans les systèmes Linux, et sa taille doit être adaptée aux besoins spécifiques de votre système.

# **Création d'un fichier swap permanent**

***Vérification de l’espace d’échange existant :***

Avant de créer un nouveau swap, vérifiez si vous avez déjà de l’espace d’échange sur votre système. Vous pouvez utiliser la commande free pour cela :

```
free -h
```

Si vous n’avez pas d’espace d’échange, la sortie affichera :

```
Swap: 0B
```

***Création d’un fichier d’échange :***

Tout d’abord, créez un fichier avec la taille d’espace d’échange souhaitée (par exemple, 2 Go) :

```
sudo fallocate -l 2G /swapfile
```

Ensuite, marquez ce fichier comme espace d’échange :

```
sudo mkswap /swapfile
```

***Activez le fichier d’échange :***

```
sudo swapon /swapfile
```

***Rendre les modifications permanentes :***

Pour que le fichier d’échange soit utilisé à chaque démarrage, ajoutez-le au fichier /etc/fstab. Faites d’abord une copie de sauvegarde du fichier :

```
sudo cp /etc/fstab /etc/fstab.back
```

Ensuite, ajoutez la ligne suivante à la fin du fichier /etc/fstab :

```
/swapfile none swap sw 0 0
```

Après avoir effectué ces étapes, redémarrez votre système pour que les modifications prennent effet.