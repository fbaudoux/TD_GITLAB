Les mains dans la forge 

Dans ce TD, nous allons réaliser un ensemble de manipulations sur la forge GitLab en simulant des cas d'usage de la vie réelle.


# Table des matières
1. [Nouveau projet](#projet)
2. [Déclaration des droits et utilisateurs](#droits)
3. [Intégration continue](#ci)
4. [Développement](#planning)

# Nouveau projet <a name="introduction"></a>

Nous allons commencer par créer un nouveau projet GitLab qui va nous servir de support pour ce TD.

![image](uploads/90fe281b8efa251ba1bc9857c4d5b522/image.png)

Je le nomme "configGitLab" et je le déclare pour un projet privé

![image](uploads/094ed63c8261fbfe41d4478d8d39cd3a/image.png)

Comme il n'y a aucun fichier source, GitLab ne m'affiche pas grand chose, donc je demande à créer le fichier README.md en utilisant l'éditeur en ligne

![image](uploads/4d2da6a48bc123fb0517fbb9ed6e1bd1/image.png)

Une fois ce fichier crée, GitLab commence à m'afficher une page avec divers informations pratiques (nombre de commits , de branches, taille du stockage ...) et des raccourcis permettant notamment de configurer l'intégration continue. Nous y reviendrons plus tard. 

![image](uploads/6974e9009b28c34ef7f229dc795d6580/image.png)

Pour le moment, nous allons nous intéresser un peu au menu de gauche. Dans ce menu, on va trouver toutes les fonctions mise à disposition par la forge pour notre projet. Il y en a trop pour être couvertes dans un seul TD, nous allons donc nous focaliser sur les plus utiles.

![image](uploads/4ae09a691f5b9ed1f53b2e1316e1ac61/image.png)

D'ailleurs, pour y voir plus clair, nous allons commencer par faire disparaitre quelques fonctions que nous n'utiliserons pas pour le moment.  
Pour faire ça, on doit se rendre dans le menu "Settings"  
![image](uploads/0dd6bf18b75a1e124d81ddc729c429e8/image.png)


On va retrouver divers rubriques. La rubrique "Général" nous permet de donner quelques informations supplémentaires sur le projet et de lui associer une icône

![image](uploads/6080b1e5b7744b89e5081183c3927a6c/image.png)

Cela commence a devenir plus intéressant dans le sous menu : "Visibility, project features, permissions"
On peut changer la visibilité du projet mais également cacher toutes les fonctions optionnelles de la forge : Wiki, merge request, intégration continue, ...

![image](uploads/42c144b3e33c0ca5148d481da92a2872/image.png)

D'ailleurs, je cache toutes les fonctions optionnelles et j'obtiens un menu plus simple.
Au fur et à mesure, nous viendrons réactiver ce qui nous sera nécessaire.

![image](uploads/23141a7e1057f3b5a4a894b519a84865/image.png)

Il est à noter que les fonctions peut être activés / désactivées en fonction des utilisateurs (membre / non membre de l'équipe ) pour les projets donc la visibilité est "Public" ou "Internal" 

![image](uploads/fb81b0596aae5279a4a6bace376688b4/image.png)

![image](uploads/ee2c26c3fa5384324e050ecf8efa823f/image.png)

![image](uploads/713f444e4a57f7010b31152d7cf26d0b/image.png)


# Déclaration des droits et utilisateurs <a name="droits"></a>

Parmi les fonctions non optionnelles, on trouve la gestion des utilisateurs à laquelle on accède via menu à gauche "Members".  

On peut déclarer des utilisateurs autorisés à intervenir sur les projets et leur assigner des rôles.

![image](uploads/d672cd0edc7fce2df1048e473a3790b4/image.png)

Cette notion de rôle est très importante dans la mesure on l'on va pouvoir autoriser certaines actions de la forge uniquement à certains rôles.
C'est le cas en particulier pour la gestion des branches.  
Via le menu Settings - Repository, on peut accèder à la sous catégorie "Protected branch"

![image](uploads/0d6bc18c7f7bf7762c8abea961a6ba18/image.png)

Dans cet écran, on va être capable de définir quels rôles ont le droit de modifier quelles branches.
Si un utilisateur n'a pas les droits pour modifier une branche, il devra travailler dans une autre branche et demander à intégrer son travail via une "merge request" ( si la fonction est laissée active )

![image](uploads/ffa9101a6ee9a5ce6df86bb4dd599cc7/image.png)

# Intégration continue <a name="ci"></a>

Nous avons un projet et des utilisateurs déclarés, la dernière étape avant de commencer les sprints c'est de configurer l'intégration continue.
Une fois que j'ai reactivé la fonction dans le projet, j'ai le menu "CI/CD" qui apparait à gauche

Si je sélectionne ce menu, j'ai la possibilité de lancer un intégration continue en demandant "run pipeline" , mais je vais obtenir une erreur du type :  

![image](uploads/6d8a20d106537b4bc115c78cd9598c87/image.png)

Je n'ai pas défini pource projet, comment la forge devait construire le projet.
Pour établir cette définition, je retourne sur la page principale du projet ( via le menu Project Overview ) et je peux cliquer sur le raccourci "Set up CI/CD"

J'obtiens un écran de ce type 

![image](uploads/75a5bffcae7ddcdcb748a652cc8512f4/image.png)

La configuration est un fichier yaml , qui se situe dans le repository du projet.

Je vais utiliser l'exemple donné par la documentation de GitLab et je crée le contenu du fichier comme suit :

```
build-job:
  stage: build
  script:
    - echo "Hello, $GITLAB_USER_LOGIN!"

test-job1:
  stage: test
  script:
    - echo "This job tests something"

test-job2:
  stage: test
  script:
    - echo "This job tests something, but takes more time than test-job1."
    - echo "After the echo commands complete, it runs the sleep command for 20 seconds"
    - echo "which simulates a test that runs 20 seconds longer than test-job1"
    - sleep 20

deploy-prod:
  stage: deploy
  script:
    - echo "This job deploys something from the $CI_COMMIT_BRANCH branch."
```

Dès que j'ai fait un commit sur ce fichier, si je retourne dans le menu CI/CD, je dois voir un pipeline en cours d'exécution.

![image](uploads/5e43f29cc64a8e186045960dfc9814f9/image.png)

Si je clique sur la ligne du pipeline, je vais voir le détail des taches

![image](uploads/d5154a8d2b0b6540cf51e68244a61d1b/image.png)

Le script d'exemple et la documentation de GitLab nous permettent rapidement de comprendre comment écrire des scripts simple.
Mais nous comprenons également que ces scripts permettent d'écrire des scripts très puissants et donc plus compliqués à maitriser.

https://docs.gitlab.com/ee/ci/quick_start/

![image](uploads/368a73d84ab8ae8cd06bdcabb2f03e2f/image.png)


## Qui executent les scripts ? La forge ?

En fait pas vraiment, la forge orchestre mais n'execute pas les scripts.
Pour cela, il y a les "workers"Runners".
On peut voir ce fonctionnement en allant dans la configuration de la CI/CD

![image](uploads/cb94131776591d02a300a3604491ed01/image.png)

Il y a un sous menu "Runners" qui explique le concept de runner
![image](uploads/dd467b966c0d7446825e9d435361fa87/image.png)

En dépliant le menu on a la liste des différents runners accessibles pour notre projet.
![image](uploads/587c17c4c692e76538b77287c04d321d/image.png)

On constate qu'il y a un runner partagé par potentiellement tous les projets et qui a été mis en place par l'IUT. C'est le runner qui s'appelle "runner-forge".

Le problème c'est qu'il n'y en a qu'un seul. On comprend vite qu'il va devoir partager son temps d'exécution entre tous les pipelines de tous les projets de la forge ! 

La solution c'est de déclarer un runner spécifique à notre projet.

## Je déclare un runner dédié à mon projet

Je suis les instructions données via les liens affichés dans la forge et je vais déclarer un runner "docker" sur ma machine.

Je crée un volume docker pour avoir un persistence de ma configuration

```
docker volume create gitlab-runner-config

```

Je lance l'image docker du runner en précisant mon volume et en demandant l'enregsitrement d'un nouveau runner

```
docker run --rm -it -v gitlab-runner-config:/etc/gitlab-runner gitlab/gitlab-runner:latest register
```

![image](uploads/3d7ef2e38cda600946e3ed7876b48c1a/image.png)

Enfin , je lance le runner à partir de la configuration qui se trouve dans mon volume docker
```
docker run -d --name gitlab-runner --restart always -v gitlab-runner-config:/etc/gitlab-runner  gitlab/gitlab-runner:latest run
```


Sous GitLab , désormais, on doit voir un nouveau runner dédié à notre projet

![image](uploads/cea035cd2b19a0e8411bf1dd1c438276/image.png)

Pour être certain que c'est ce runner spécifique et pas celui partagé qui va être executé, je peux demander à ne pas utiliser les runners partagés pour mon projet.

![image](uploads/03a561549aa0409f27ad33c1548f322c/image.png)

## J'utilise un vrai code source

Le script de CI de base nous a permis de découvrir le mécanisme mais si on veut vraiment compiler un projet et produire un artefact, il nous faut du code source.

Je commence donc par cloner mon repository.

![image](uploads/d58388fbde299b576830bf44f7c1af9e/image.png)

Je me rend sur https://start.spring.io/ et je choisi la configuration suivante :

![image](uploads/3fd36a03ad707164f4927e7638353181/image.png)

Je demander à télécharger le fichier (demo.zip) que je décompresse dans le répertoire git de mon projet.

J'ajoute tous les nouveaux fichiers, commit , push .

![image](uploads/570cf982d2fb3e65fbb72d536858cf33/image.png)

Quand je retourne dans la forge, je vois mes fichiers sources.

![image](uploads/9178e79e3dbcea6c56741d9d2efdf7d0/image.png)

## Je modifie mon runner dédié

Le problème maintenant, c'est que mon fichier de CI ne correspond plus à ce que je veux faire
![image](uploads/f18331a0d3b98cd036046ba59b4bf10c/image.png)

Je le modifie donc pour qu'il puisse compiler un construire un projet SpringBoot

```
image: java:8

stages:
  - build
build:
  stage: build
  script: 
    - chmod +x ./gradlew
    - ./gradlew build -x test
  artifacts:
    paths:
      - build/libs/demo-0.0.1-SNAPSHOT.jar
```

Je supprime mon runner spécifique et je réactive le runner partagé pour vérifier que le pipeline fonctionne. On peut voir la ligne de pipeline qui s'est executé avec succès.
Sur la ligne, on trouve une icone pour télécharger l'artefact

![image](uploads/0e834fa4f99f6d3029494153a58a7584/image.png)

On peut également voir le détail de l'exécution 

![image](uploads/d7d4dd06926e85877540ff8878a73db7/image.png)

Il ne nous reste plus qu'a reconfigurer notre runner dédié.

![image](uploads/ed91e003ddbac4b548abc66b46854fb5/image.png)


```
docker run -d --name gitlab-runner --restart always -v /var/run/docker.sock:/var/run/docker.sock -v gitlab-runner-config:/etc/gitlab-runner gitlab/gitlab-runner:latest
```

Vérifier que mon runner est bien connecté à la forge

![image](uploads/cea035cd2b19a0e8411bf1dd1c438276/image.png)

A nouveau interdire les runners partagés

![image](uploads/bb33d88f52219f234b30ba68015a768f/image.png)

![image](uploads/ff55c30c4cda77720fb1a116624912a0/image.png)

Lancer un pipeline et vérifier que tout ce passe bien.

![image](uploads/75f5be9dc79078e189d56da71137d8f1/image.png)

Nous sommes prêts à développer !!

# Développement <a name="dev"></a>

La forge va nous aider dans le processus de développement.

On peut par exemple déclarer les sprints de dév via la notion de milestone.
Pour cela, il faut avoir activé l'option suivante :  

![image](uploads/34d43c19bdc1b8bcc880e349af93dab6/image.png)

Cela a pour effet de faire apparaitre le menu "suivant "issues" qui permet d'accèder aux milestones.

![image](uploads/a36c5ee64d383b12f3b43c50e2a68e04/image.png)

Nous allons définir notre premier sprint comme étant un milestone.
Nous avons simplement à indiquer des dates de début et de fin ainsi qu'un nom et nous pouvons créér un nouveau milestone.

![image](uploads/6516280c560b9121ee60edb6a0f48639/image.png)

Il apparait alors dans la liste et il est indiqué comme étant complet à 0%

![image](uploads/9892211a083df09a30b3453e02a99f3c/image.png)

Nous comprenons que cette notion va nous permettre d'avoir une idée de l'avancement de notre sprint.
Mais comment va t on définir ce qui constitue le sprint ? Il suffit de cliquer sur la ligne du sprint pour commencer à comprendre. 
La forge affiche alors :

![image](uploads/bb3ca25b789bfebd801d1d59e6c3b2a6/image.png)

Les éléments qui vont constituer un sprint sont les "issues". On va pouvoir les affecter à un milestone et l'avancement du milestone sera fonction du nombre d'issues qui ont été traitées.

Pour créer les "issues" qui vont constituer notre Sprint, il faut passer par le menu : 
![image](uploads/133d282af70c259bed8f3653378b7736/image.png)

On arrive sur la liste des issues déclarées et on peut en ajouter. Il y a un champ pour choisir le "milestone" auquel sera rattaché l "issue".

![image](uploads/a28b4dc628881828c7f0b4ecf1c44b23/image.png)

Dans l'exemple suivant, on voit qu'il y a 5 "issues" mais uniquement 2 sont rattachées à notre Sprint 0.

![image](uploads/32fe0b434bb0a638e1dcb29f1ac9ec24/image.png)

Pour rattacher une issue à un "milestone", il faut cliquer sur son nom dans la liste, cela ouvre une page de définition de l'issue et il y a une partie sur la droite qui permet de choisir un milestone

![image](uploads/db30fcde808bf92a80c3f2d82092091b/image.png)

![image](uploads/e26d091b29102e852bc31b8f9edd396e/image.png)

C'est également depuis cet écran que l'on peut assigner l'issue à un utilisateur 

![image](uploads/cca33a3a238ef728d934c3c4d9d1a6f7/image.png)

Quand une "issue" est affectée à un utilisateur, elle est considérée comme "en cours". On peut le voir dans le détail du milestone. 
Dans l'écran suivant, on voit que la fonction 1 est en cours et qu'elle est attribuée à mon utilisateur.

![image](uploads/53707f8034f85a6170bbd622ae5a9b8c/image.png)

![image](uploads/42b6b54044532f3b54a5a5c377d0d712/image.png)

Je simule le développement de la fonction 1 en faisant un "commit" qui crée un fichier nommé "fonction 1" 

![image](uploads/48ac8a0a30da9d1c321680fbaf9cfbec/image.png)

![image](uploads/909fa4db6c8d7c78f8a23af7431b3ac0/image.png)

Cela ne suffit pas à faire avancer le milestone car l'issue n'est pas fermée automatiquement.
Il faut fermer manuellement l'issue en la choisissant dans la liste et en demandant de la cloturer.

![image](uploads/022531bc3488160814d2c0e1636a9527/image.png)

![image](uploads/8301e1f2a4b487df9c62e964fcadf086/image.png)

L'avancement du milestone se met à jour.

![image](uploads/5fd3f65c7f60276c84c0873fd259388b/image.png)

Il y a une manière de faire qui automatise la fermeture des "issues". Il faut pour cela que la fonction "merge request" soit activée.

Si je crée une nouvelle issue associée à mon Sprint 0, j'ai la possibilité (lors de la création , ou plus tard , lorsque l'on commence le développement) de choisir de créer une branche dédiée à cette "issue".

![image](uploads/87488e477c34636fe2be923ab6945ac9/image.png)

Et quand j'ai réalisé mon commit sur cette branche , je peux demander un "merge request"

![image](uploads/c62fa109d6fa64efdbea8f87bb745e50/image.png)

Il est indiqué dans la fenêtre de définition du merge request que l'issue sera automatiquement fermée quand le merge sera réalisé.
![image](uploads/ae26cbe853e916227be31ff675727ae4/image.png)

Dans le menu sur la gauche , on voit apparaitre un "merge request"

![image](uploads/6b06b354aed99014a62d1911efc045fb/image.png)

Lorsque j'ouvre le "merge request" , j'ai la possibilité de ramener la fonction développée vers la branche master ( ou une autre branche selon la configuration du projet )

![image](uploads/7f0de15b0d864d2d3380f65678d3b084/image.png)

Mais j'ai également la possibilité de faire "une revue" des commits et de faire des commentaires. Ces fonctions sont accessibles en utilisant l'onglet "changes"

![image](uploads/beaa543059076f24859060f6ef4140c4/image.png)

![image](uploads/14c2cf4bb588d294c167eb145df9eedd/image.png)

Celui qui a demandé le merge request sera prévenu par mail qu'il y a des revues sur ces commits et pourra alors faire le nécessaire.

Quand le merge a été réalisé, l'issue est fermée et le milestone avance

![image](uploads/5993422a6781907f375bb781c16efcc2/image.png)

![image](uploads/69d8b63ad3c7d6c34a57400fbb60a920/image.png)

![image](uploads/dcaecab6a2ee7c685eff1c0b160fbb6f/image.png)

## Release

Le sprint va se terminer et il va aboutir à la sortie de la version 1.0 alpha du produit.
On peut donc déclarer dans la forge que l'on sortir une release associée à notre milestone.

Il faut passer par le menu suivant pour demander une nouvelle release.

![image](uploads/6f5307764ecc81b6d65d4eb23e84d501/image.png)

Dans l'écran de création de la release, on peut associer un milestone.

![image](uploads/ac550edc4b176107280f56a32481f89c/image.png)

L'avancement de la release correspondra à l'avancement du milestone.

![image](uploads/f93c2fd5d23383c5145955fab7b52919/image.png)

A noter, une release, n'est qu'un tag posé sur le repository.
Si on supprime le tag, on supprime la release.
