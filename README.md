![cover](./img/img0.jpg)

# Linux Kernel : Histoire, Architecture & Analyse de code

## Préface

Un système d'exploitation (ou OS : Operating System en anglais) est un ensemble de logiciel permettant de gérer les différentes ressources d'un ordinateur. En outre c'est le système d'exploitation de votre ordinateur qui vous permet d'ouvrir des applications tels que Google Chrome, Firefox, Outlook, ... En effet, au lieu de demander à chaque application de gérer les ressources matérielles (hardware en anglais) de ces futurs utilisateurs; l'OS centralise toutes les demandes et allocutions concernant ces ressources matérielles de l'ordinateur. Ainsi au lieu d'avoir des comportements anarchiques avec des logicielles qui se battent sans merci pour avoir un peu de RAM, d'espace de stockage ou accès à un SSD plutôt qu'un disque dur, c'est l'OS qui va agir comme gardien des ressources matérielles de l'ordinateur.

Prenons un exemple concret, vous êtes sur votre bureau et vous souhaitez ouvrir Google Chrome afin d'aller explorer internet et son web. Vous allez alors lancer Google Chrome et ce dernier va normalement s'ouvrir sans trop de soucis. Or, cette tâche tout à fait anodine a en fait demandé la sollicitation de l'OS puisque rappeler vous, c'est ce dernier et uniquement ce dernier qui détient l'accès et gère les ressources matérielles de votre ordinateur. Ainsi, lorsque vous lancez Google Chrome (ou tout autre logiciel, programme, script, ...) sur votre ordinateur, ce dernier va alors demander à l'OS s'il a la possibilité de s'ouvrir, c'est à dire s'il reste de la place sur la RAM par exemple pour stocker et afficher les pages webs. Si oui, alors Google Chrome s'ouvre et à vous les joies d'internet. Sinon, Google Chrome plante et rien ne se passera.

L'idée générale à retenir d'un OS, c'est qu'aucun logiciel ne doit accéder aux ressources matérielles sans l'accord et le contrôle de ce premier. Car dans le cas contraire, ce serait l'anarchie sur votre ordinateur et chaque programme tentera de prendre le plus de ressources possibles et au final aucun de ces programmes ne marchera. Il vaut donc mieux laisser un système d'exploitation faire le travail d'accès et de gestion du matériels pour tous les logiciels où ces derniers puissent cohabiter, plutôt que l'inverse.

Nous pouvons donc résumer le travail d'un système d'exploitation avec ce schéma ci-dessous :

<p align="center">
  <img src="./img/img2.jpg" alt="drawing" title="OS" width="700"/>
</p>

Nous savons maintenant expliquer à quoi sert, de base, un système d'exploitation. Chaque système d'exploitation est unique et ne propose pas les mêmes services que ces pairs donc il est plutôt hardu de définir exhaustivement tous les services que propose tous les OS, mais dorénavant vous avez l'essentiel.

## Commençons 

Au vu de la complexité que représente un système d'exploitation, il est évidemment impossible de lister exhaustivement toute l'architecture et d'analyser tout le code de Linux dans une présentation concise et si possible intéressante. Dans ce rapport, nous ne ferons donc qu'aller à l'essentiel sur de nombreux points surtout au niveau de l'architecture qui est plutôt assez impressionante et rebutante quand on s'y plonge dedans. En ce qui concerne l'analyse du code, nous n'allons évidemment pas commenter chaque ligne de code du noyau Linux, nous prendrons un module comme exemple de tout le projet Linux avec des défauts et des avantages caractéristiques de tout le kernel.

Il est bon de préciser aussi, que ce rapport ne fait aucunement la promotion d'un système d'exploitation en dépit d'un autre. Chaque OS a ces avantages et ces inconvénients et répondent à différentes problématiques spécifiques. Nous ne ferons qu'ici un rapport expliquant et détaillant les forces et les faiblesses du noyau Linux.

Mais avant de commencer réellement et de se lancer dans le grand bain, il est important de savoir dans quoi nous allons plonger.

## Historique

Si vous avez bien compris la préface, vous avez compris qu'un ordinateur ne peut fonctionner sans OS, donc logiquement depuis que les ordinateurs existent (cc Alan Turing) il y a toujours un ensemble d'algorithme qui s'apparante aujourd'hui à un système d'exploitation. 

Avant Linux, il y avait deux principaux système d'exploitation qui régnait sur le monde de l'informatique :
- MS-DOS : système d'exploitation propriétaire et payant de Microsoft 
- Unix : système d'exploitation propriétaire et payant aussi de AT&T (considéré comme plus performant que son concurrent mais plus dur à prendre en main)

A cette époque, pas d'interface graphique, faute de mémoire vive, mais les ordinateurs fonctionnaient de la même façon qu'aujourd'hui. Seul problème, vous l'avez peut être remarqué, les deux principaux systèmes d'exploitation sont propriétaires et payants ce qui est plutôt contraignants quand on est feru d'informatique : payer une licence 200-300€ à chaque fois que l'on souhaite monter un ordinateur c'est plutôt contraignant quand on souhaite se mettre à un niveau avancé en informatique.

C'est alors que deux projets vont voir le jour en même temps pour développer un nouveau système d'exploitation totalement gratuit et surtout libre. Un logiciel libre signifie qu'en plus d'être gratuit, le code source est consultable par les utilisateurs (open source en anglais) ce qui donne la possibilité de le modifier selon leur bon vouloir et même de le redistribuer pour en faire leur propre version et même la "commercialiser" :
- D'un côté le projet GNU de Richard Stallman qui souhaitait créer un système d'exploitation gratuit basé sur Unix
- De l'autre un étudiant, Linus Torvald, de l'Université de Helsinki (Finlande), entreprend de créer sur son temps libre son propre système d'exploitation : Linux (contraction de Linus et Unix)

Il s'avère que les projets se développent à peu près en même temps mais surtout qu'ils sont complémentaires. D'un côté tandis que Richard Stallman créait les programmes de base (programme de copie de fichier, de suppression de fichier, éditeur de texte), Linus s'était lancé dans la création du « cœur » d'un système d'exploitation : le noyau. Le projet GNU (programmes libres) et Linux (noyau d'OS) ont fusionné pour créer GNU/Linux.
