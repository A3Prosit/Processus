## **I. Définition des termes**

- processus : instance d'un programme, chacun son espace mémoire , peut être divisé en threads. Il est maintenu par l'OS
- thread : plus petite unité de process dans un procesus, tournent dans le même espace mémoire, feature de l'environement d'opération pas du CPU). Il est maintenu par le programmeur

* Kernel

* C++

* OS

* Gestion des processus

* Différences architecturales

* Temps partagé

* Gestion mémoire

* allouée

* primitif système

* Gestion des utilisateurs

* Dynamiquement

* Procédure sous-jacente

* Architecture

* Gestion matériel

* Java

## **II. Analyse du contexte**

* Quoi ?

Pourquoi un algo algorithme java ne fonctionne pas en C++/Linux.

* Comment ?

En étudiant les processus (des différentes architectures).

* Pourquoi ?

Architectures différentes.

## **III. Définition de la problématique**

* Comment faire fonctionner un programme sur les deux OS ?

* Comment fonctionne les processus sous linux ?

* Comment fonctionne le kernel sous linux ?

## **IV. Définition des contraintes**

## **V. Généralisation**

Compatibilité

## **VI. Hypothèses**

* Incompatibilité est due à la différente gestion du matériel

* Gestion mémoire différente en java et c++ (garbage collector)

* Sous Linux, processus gérés sous ID alors que Windows en nom de services

* Sous linux, chaque thread est géré par des _tasks_struct_

* La gestion des temps de parole est différente

  

## **VII. Plan d’action**

**Études :**

* Différences entre java/C++


### ***Processus***


variables d'env: les données du prog ne sont pas suffisantes pour un processus. Il faut également des variables d'environement: les fichiers sur lesquels il opère, la valeur du compteur ordinal, etc. On les utilise car il est possible d'avoir 2 processus utilisant le même code mais n'étant pas au même stade du programme ou n'utilisant pas les mêmes fichiers. On doit également pouvoir interrompre le processus dans le cadre du pseudo-parallélisme


composants principaux d’un bloc de contrôle de processus = là où on met les variables d'env et le processus

· PID

· Pointeur sur Stack

· Etat du processus

· Program counter

· Registre CPU

· Liste des fichiers ouverts

* Gestion (différents états etc...)

* Thread (synchronisation)

nice pid = monte la priorité
kill -SIGSTP = ctrl+z = pause
kill -SIGKILL = ctrl+c = kill
jobs = ps pour l'actuel
ps = liste les processus
ps -u montre:
-  user
- PID
-  %CPU
- %MEM
-  VSZ(mém virtuelle)
-  RSS (mém phys hors swap)
-  TTY (terminal executant)
-  STAT (D sommeil nn interuptible, R(unning),  S(leeping) interuptible, T en pause)
-  START heure de démarrage
-  TIME temps de proco utilisé
-  COMMAND commande exécutée

relancer un prog:
premier plan ```fg [PID]```
arrière plan ```bg [PID]```


***Systèmes d’exploitation***

* Scheduler

### Architecture OS

systèmes multi-tâches:

où tâche fait référence à un process ou un thread (et non pas une task comme le threads n'interagissant pas directement avec l'OS où un task peut avoir plusieurs threads)

lorsqu'un OS peut exécuter le programme d'un utilisateur, lire les données d'un disque et imprimmer un fichier en même temps. Il est possible qu'un programme soit exécuté plusieurs fois en même temps mais pas avec le même environement (ex: différents fichiers, pas au même niveau dans le programme)

windows = processus, linux = task

Temps partagé:

Il fut un temps les OS multi-tâches  tournaient sur des pc avec un seul micro-processeur. Il ne peut à un instant donné n'exécuter qu'un seul programme mais le système peut passer d'un prog à l'autre en exécutant chaque prog pendant quelques dizaines de millisec ce qui donne une illusion de multi-tasking . On parle alors de système à temps partagé et on parle de pseudo-parallélisme

![](https://www.tutorialspoint.com/operating_system/images/independent_sequential.jpg)

Variables d'env:

On peut avoir plusieurs instances du même programme (code)  n'exécutant pas les mêmes tâches au même moment. on doit donc utiliser des variables d'env pour stocker les fichiers ouverts, la valeur du compteur ordinal, etc. Dans le cas du pseudo parallélisme on peut donc mettre le programme en pause et le reprendre là où il en était plus tard.
La liste des variables d'env dépend de l'OS et même de sa version. Elle se trouve dans le descripteur de processus (process descriptor)

Espace mémoire
Dans la plupart des OS chaque processus possède son propre espae mémire non accessible aux autres processus. On parle de l'espace d'adressage du processus.

traitement des durées:
puisque le proco commute entre les processus, la vitesse d'exécution d'un processus ne sera pas uniforme  et variera à chaque fois
Lorsqu’un processus a besoin de mesurer des durées avec précision, c’est-à-dire lorsque certains événements doivent absolument se produire au bout de quelques millisecondes (lecture/écriture par ex), il faut prendre des mesures particulières pour s’en assurer. On utilise alors des minuteurs

système multi-utilisateurs:

un système multi-user peut exécuter de façon pseudo concurrente(en même temps et se disputant pour les ressources) et indépendante (n'a pas à faire attention aux autres) les apps de plusieurs users

un sytème multi-user et forcément multi-task

Le fonctionnement est similaire au multi-tasking, on attribue des laps de temps à chaque user
Dans le cas d'un système multi-user l'OS doit mettre en place certains mécanismes:
- un méca d'authentification pour vérif l'identité de l'utilisateur
- un méca de protection pour empêcher des prog de bloquer les autres
- un méa de comptabilit pour limiter les ressources allouées à chaque user



La notion fondamentale d'un OS multi-tâche est celle de processus, il s'agit d'une instance d'un programme

préamptif = se fait interompre
colaboratif : je me lib tout seul

### Architecture Kernel, deux types (les info les plus importantes)
	 
Un OS comporte un certain nombre de routines, les plus importantes constituent le noyau (kernel). Il est chargé en mémoire vive à l'initialisation du système et contient e nombreuses procédures nécessaires au bon fonctionnement du système. Les routines moins critiques sont appelées des utilitaires

Le kernel est composé d 4 parties principales: les gestionaire des tâches, le gestionnaire de mémoire, le gestionaire de fichier et le gestionnaire de périph d'entrée-sortie. Il a également 2 parties auxiliaires: le chargeur du système d'explotation et l'interpreteur de commandes

**Gestionnaire des tâches**

Sur un OS a temps partagé une des parties les plus importantes est le scheduler ou ordonnanceur. Sur un système mono proco il divise le temps en laps de temps (slices) Il décide périodiquement d'interrompre le processus en cours et démarre/reprend un autre, soit parceque l'actuel à épuise son temps ou est bloqué

**Gestionnaire de mémoire**

Il doit connaitre les parties libres et les parties occupées, les allouer aux processus qui en ont besoin et récupérer celle qui n'est plus utilisée et traiter le swap

**Gestionnaire de fichiers**

Un des rôle principal de l'OS est d'offrir une abstraction des spécificités du disque et périphériques d'entrée-sortie pour offrir aux dev un modèle facile d'emploi

**Gestionnaire de périphériques**

Une autre fonction principale est la gestion des I/O. Il envoie des commandes aux périphériques intercepte les interruptions et traite les erreurs. Il fourni une interface simple et facile d'emploi idéalement la même pour tous les périph

### Structure interne d'un OS

**système monolithique**:

collection de procédures, chacune pouvant à tout moment appeller n'importe quelle autre. La plus répandue
Il n'y a aucun masquage de l'information, chaque procédure est visible de toutes les autres.

**système à mode noyau et utilisateur**
Dans beaucoup d'OS il y a 2modes : le mode noyau et le mode utilisateur.
l'OS démarre en mode noyau pour initialiser les périph et mettre en place les routines de service pour les appels système. Il passe ensuite en mode utilisateur, on ne peut pas accéder directement aux périph on doit utiliser des appels système. Le noyau reçoit l'appel, vérif que la demande est valable, l'exécute puis renvoie le résultat au mode utilisateur. On ne peut changer le mode noyau qu'avec une compilation du noyau, même le super-utilisateur agit en mode utilisateur.

Unix et windows utilisent ce modèle c'est pour ça qu'on ne peut pas tout y programmer

**système à couches**

dans un système à plusieurs couches chaque couche s'appuie sur celle qui lui est inférieure. Les 2 archi précédentes sont des systèmes a 2 couches

le système mnix qui inspira Linux est un système à 4 couches:

- la plus basse traite les interruptions et les déroutements(traps) et fourni aux couches supp un modèle constitué de processus séquentiels indépendants qui communiquent avec des messages. Elle a 2 utilités : gérer les interruptions et déroutements, et offrir un système de messages
- la couche 2 contient les pilotes de priphériques (les drivers) un par type de périph (disque, horloge, terminal) et la "tâche système"
Les tâches de la couche 2  et 1 forment un seul programme , le kernel. Les tâches de la couche 2 sont indépendantes bien qu'elle fassent parti du même programme, elles communiquent avec un système de message
- la couche 3 comprends le memory manager (gestio de mémoire) qui traite tous les appels système concernant la mémoire ( fork, exec, brk ) et le système de fichier qui s'occupe des appels système du système de fichier (read, mount, chdir)
- la couche 4 contient tout les processus des utilisateurs, interpréteurs de commande, éditeurs de texte, compilo, prog écrits par les users
Linux s'est inspiré de ce modèle même si il ne contient officiellement que 2 couches

**systèmes à micro noyau:**

ces système n'ont que quelques fonctions: primitives de synchro, gestion des tâches simple et un mécanisme de comm entre processus. Des processus système s'exécutent au-dessus du micro-noyau pour implémenter les autres fonctions d'un OS comme l'allocation mémoire ou le gestionnaire de périph

au final ils sont plus lents que les syst monolithiques à cause du coup des passages de messages entre les couches

**système à modules:**

module: fichier dont le code peut être lié au noyau en cours d'exécution
Ce code objet est en général constitué d’un ensemble de fonctions qui implémente un système de fichiers, un pilote de périphérique, ou tout autre fonctionnalité de haut niveau d’un système d’exploitation. Il est exécuté en mode noyau au nom du processus courant, comme toute fonction liée statiquement dans le noyau.

noyau hybride et noyau en temps réel: micro noyau discutent direct avec les applis

avantages:
modulaire: 
indépendant de la plateforme
pas de perte de perf

### Rôles / OS
3 rôles:
charger les programmes

on peut changer de programme sans éteindre l'ordinateur contrairement à avant

machine virtuelle

le but d'un OS est aussi de fournir une API pour s'abstraire des couches matérielles, cela reviens donc à fournir une machine virtuelle

sinon on devrait charger les registres, déplacer le bras du disque dur, etc. A la place on considère que le disque contient des fichiers qu'on peut lire ou écrire

gestionnaire des ressources

Enfi l'OS doit servirà ordiner et contrôler l'allocation des ressources. Sinon on pourrait avoir 3 programmes qui essayent d'imprimer en même temps 

**A Faire :**

***Corbeille*** 1

*Corbeille secondaire

*** le plus important**
