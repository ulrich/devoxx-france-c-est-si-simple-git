# C'est si simple git !

D�p�t GIT correspondant � mon quickie donn� � [Devoxx France 2012](http://www.devoxx.com/pages/viewpage.action?pageId=6128497 "Devoxx France 2012")

## Abstract :

Montrer les cas pratiques et probl�mes de tous les jours comme :

* Comment cloner un _repository_ local,
* Comment un d�veloppeur cr�e une branche de bugfix,
* Comment un coll�gue s'accroche � la branche de ce dernier pour l'aider � fixer le bug de mani�re collaborative, 
* Comment g�rer facilement des petits probl�mes de _merge_,
* Comment _pusher_ cette branche dans le _remote_ g�r� par l'admin,
* Comment l'admin g�re/synth�tise les _commit_ en les _squashant_.

## Pratique (environ 10mn) :

L'id�e est de faire collaborer _user1_ et _user2_ sur la r�solution du _JIRA123_, _via_ un d�p�t local commun aux deux d�veloppeurs. A l'issue de la r�solution, l'un des deux _pushera_ les modifications dans une branche _JIRA123_ du d�p�t principal.
Les _commits_ contenant les modifications de _user1_ et _user2_ seront _squash�s_ et _merg�s_ dans la branche _master_. Pendant la phase de _merge_, _superuser_ devra g�rer un conflit _via_ la commande _diffuse_.

Soit le sc�nario suivant :

* superuser cr�e un projet Maven simple (prendre celui par d�faut) :

            mvn archetype:generate
* superuser cr�e un repository GIT dans ce dernier :

            git init
* superuser ajoute et commit le projet :

            git add * && git ci -m 'superuser - Set the controls for the heart of the sun'
* superuser lance une commande pour permettre le push dans son d�p�t :
            git config receive.denyCurrentBranch ignore
* user1 clone le repository :

            git clone repository/ user1
* user2 clone le repository :

            git clone repository/ user2
* user1 cr�e un branche de bugfix JIRA123 :

            git checkout -b JIRA123
* user1 commence a travailler dessus et fait deux commits dedans :

            git ci -am 'user1 - fixed pom encoding'
            git ci -am 'user1 - fixed JUnit version'
* user2 se joint � lui pour l'aider,
* user2 fetch la branche user1/JIRA123 et cr�e donc une user2/JIRA123 :

            git fetch ../user1/ JIRA123:JIRA123
* user2 se place sur la branche JIRA123 :

            git co JIRA123	
* user2 ajoute un fichier README et fait un commit sur user2/JIRA123 :

            git add * && git ci -am 'user2 - added README file'
* user2 push la branche et ses commit dans origin :

            git push origin JIRA123
* user1 ajoute un fichier tips.txt et fait un commit sur user2/JIRA123 :

            git add * && git ci -am 'user1 - added tips file'
* user1 veut pusher sa modification, mais doit se mettre � jour :

            git push origin JIRA123
    * A l'inverse de SVN qui merge sur le serveur sans se soucier des modifications tierces, GIT exige un merge local :

	            git pull origin JIRA123
* user1 push sa modification dans origin/JIRA123 :

            git push origin JIRA123
* superuser se place dans la branche cr��e par les d�veloppeurs :

            git co JIRA123
* superuser veut merger l'ensemble des commits dans master, mais via un seul commit. Il consulte donc l'historique des commits avant pour savoir quoi fusionner :

            git logc
* superuser lance la commande lui permettant de squasher le tout :

            git rebase -i JIRA123~4
* superuser se place sur master pour merger tout le travail de la branche JIRA123 :

            git co master
* superuser a entre-temps commiter la bonne version de JUnit dans le POM :

            git ci -am 'superuser - used my personnal JUnit version'
* superuser merge la branche :

            git merge JIRA123
* superuser rencontre un merge et lance la commande :

            git mergetool
* superuser a merg� et peut commiter le bugfix d'user1 et user2

            git ci -m 'superuser - fixed JIRA123 bugfix'

**Donc comme pr�vu : user1 et user2 ont collabor� ensemble pour fixer JIRA123 et laiss� superuser le soin de rassembler les commits.**
