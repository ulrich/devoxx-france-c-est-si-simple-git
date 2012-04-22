# C'est si simple git !

Dépôt GIT correspondant à mon quickie donné à [Devoxx France 2012](http://www.devoxx.com/pages/viewpage.action?pageId=6128497 "Devoxx France 2012")

## Abstract :

Montrer les cas pratiques et problèmes de tous les jours comme :

* Comment cloner un _repository_ local,
* Comment un développeur crée une branche de bugfix,
* Comment un collègue s'accroche à la branche de ce dernier pour l'aider à fixer le bug de manière collaborative, 
* Comment gérer facilement des petits problèmes de _merge_,
* Comment _pusher_ cette branche dans le _remote_ gérer par l'admin,
* Comment l'admin gère/synthétise les _commit_ en les _squashant_.

## Pratique (environ 10mn) :

L'idée est faire collaborer _user1_ et _user2_ sur la résolution du _JIRA123_, _via_ un dépôt local commun aux deux développeurs. A l'issue de la résolution, l'un des deux _pushera_ les modifications dans une branche _JIRA123_ du dépôt principal.
Les _commits_ contenant les modifications de _user1_ et _user2_ seront _squashés_ et _mergés_ dans la branche _master_. Pendant la phase de _merge_, _superuser_ devra gérer un conflit _via_ la commande _diffuse_.

Soit le scénario suivant :

* superuser crée un projet Maven simple (prendre celui par défaut) :
    * `mvn archetype:generate`
* superuser crée un repository `GIT dans ce dernier :`
    * `git init`
* superuser ajoute et commit le projet :
    * `git add * && git ci -m 'superuser - Set the controls for the heart of the sun'`
* superuser lance une commande pour permettre le push dans son dépôt :
   * `git config receive.denyCurrentBranch ignore`
* user1 clone le repository :
    * `git clone repository/ user1`
* user2 clone le repository :
    * `git clone repository/ user2`
* user1 crée un branche de bugfix JIRA123 :
    * `git checkout -b JIRA123`
* user1 commence a travailler dessus et fait deux commits dedans :
    * `git ci -am 'user1 - fixed pom encoding'`
    * `git ci -am 'user1 - fixed JUnit version'`
* user2 se joint à lui pour l'aider,
* user2 fetch la branche user1/JIRA123 et crée donc une user2/JIRA123 :
    * `git fetch ../user1/ JIRA123:JIRA123`
* user2 se place sur la branche JIRA123 :
    * `git co JIRA123	`
* user2 ajoute un fichier README et fait un commit sur user2/JIRA123 :
    *  `git add * && git ci -am 'user2 - added README file'`
* user2 push la branche et ses commit dans origin :
    * `git push origin JIRA123`
* user1 ajoute un fichier tips.txt et fait un commit sur user2/JIRA123 :
    *  `git add * && git ci -am 'user1 - added tips file'`
* user1 veut pusher sa modification, mais doit se mettre à jour :
    * `git push origin JIRA123`
    * A l'inverse de SVN qui merge sur le serveur sans se soucier des modifications tierces, `GIT exige un merge local :`
	    * `git pull origin JIRA123`
* user1 push sa modification dans origin/JIRA123 :
    * `git push origin JIRA123`
* superuser se place dans la branche créée par les développeurs :
    * `git co JIRA123`
* superuser veut merger l'ensemble des commits dans master, mais via un seul commit. Il consulte donc l'historique des commits avant pour savoir quoi fusionner :
    * `git logc`
* superuser lance la commande lui permettant de squasher le tout :
    * `git rebase -i JIRA123~4`
* superuser se place sur master pour merger tout le travail de la branche JIRA123 :
    * `git co master`
* superuser a entre-temps commiter la bonne version de JUnit dans le POM :
    * `git ci -am 'superuser - used my personnal JUnit version'`
* superuser merge la branche :
    * `git merge JIRA123`
* superuser rencontre un merge et lance la commande :
    * `git mergetool`
* superuser a mergé et peut commiter le bugfix d'user1 et user2
    * `git ci -m 'superuser - fixed JIRA123 bugfix'`

**Donc comme prévu : user1 et user2 ont collaboré ensemble pour fixer JIRA123 et laissé superuser le soin de rassembler les commits.**


