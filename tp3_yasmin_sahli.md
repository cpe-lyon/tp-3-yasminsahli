# TP 3 - Utilisateurs, groupes et permissions
Yasmin SAHLI 3ICS

## Exercice 1 : Gestion des utilisateurs et des groupes

1. Utilisez la commande groupadd pour créer deux groupes dev et infra
```bash
sudo groupadd dev
sudo groupadd infra
```
2.Créez ensuite 4 utilisateurs alice, bob, charlie, dave avec la commande useradd, en demandant la création de leur dossier personnel et avec bash pour shell
```bash
sudo useradd -m alice
sudo usermod -s /bin/bash alice
```

3. A joutez les utilisateurs dans les groupes créés : alice, bob, dave dans dev & bob, charlie, dave dans infra
```bash
sudo usermod -a -G dev alice
sudo usermod -a -G infra bob
```
4. Donnez deux moyens d’aﬀicher les membres de infra
```bash
cat /etc/group
sudo cat /etc/gshadow
```

5. Faites de dev le groupe propriétaire des répertoires /home/alice et /home/bob et de infra le groupe propriétaire de /home/charlie et /home/dave
```bash
chgrp dev /home/alice 
``` 

6. Remplacez le groupe primaire des utilisateurs : dev pour alice et bob & infra pour charlie et dave
```bash
sudo usermod -g dev alice
```

7. Créez deux répertoires /home/dev et /home/infra pour le contenu commun aux membres de chaque groupe, et mettez en place les permissions leur permettant d’écrire dans ces dossiers.
```bash
sudo mkdir dev #création du répertoire
chgrp dev /home/dev 
sudo chmod g+w dev #permissions
```

8.  Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer ou supprimer ce fichier ?

Pour que seul le propriétaire d'un fichier ait le droit de renommer ou supprimer un fichier on fait : 
``` 
sudo chmod +t infra/infra
```
9. Pouvez-vous ouvrir une session en tant que alice ? Pourquoi ?

On ne peut pas accéder à la sessio en tant qu'Alice car elle n'a jamais rentré de mot de passe, le session est donc encore inactive.

10. Activez le compte de l’utilisateur alice et vérifiez que vous pouvez désormais vous connecter avec son compte.
```bash
sudo passwd alice # création d'un mdp
su alice # vérifie que l'on peut se connecter
```

11. Comment obtenir l’uid et le gid de alice?

```bash
id alice 
```
12. Quelle commande permet de retrouver l’utilisateur dont l’uid est1003?
```bash
id 1003
```

13. Quel est l’id du groupe dev?

L'id du groupe dev est 1002.gpass

14. Quel groupe a pour gid 1002 ?

L'id du groupe dev est 1002.

15. Retirez l’utilisateur charlie du groupe infra. Que se passe-t-il ? Expliquez.
```bash
sudo gpasswd -d charlie infra
```
16. 
```bash
sudo chage -l dave
```
17. 
L'interpréteur de root est bash.

18. 
Le compte nobody permet de lancer des démons en tant que nobody et permet donc de limiter les dégats qu'un pirate pourrait faire. 

19. 
Le mot de passe est gardé 15 minutes en mémoire par la commande sudo. 
Pour forcer l'oublie d'un mot de passe on utilise la commande : 
```
sudo -k
```

## Exercice 2 : Gestion des permissions

1. Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier contenant quelques lignes de texte. Quels sont les droits sur test et fichier ?

Sur fichier : 
    - l'utilisateur a les droits de lecture et d'écriture
    - le groupe et autres ont juste le droit de lecture
Sur test ( dossier ) : 
    -  l'utilisateur a tous les droits ( lectures, créer et supprimer des fichiers & entrer dans le dossier)
    - le grouoes & autres ont les droits de lecture et d'éxécution (rentrer dans le dossier)


2. Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’aﬀicher en tant que root. Conclusion ?

```bash
chmod a+rwx fichier 
```
Si l'on veut modifier et afficher le fichier en tant que root, il faut d'abord rentrer le mot de passe root.

3. Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echo Hello" > fichier. 
On a vu lors des TP précédents que cette commande remplace le contenu d’un fichier s’il existe déjà. Que peut-on dire au sujet des droits ?

La commande fonctionne, mais n'affiche rien, on possède le droit d'écriture mais pas celui de lecture.

4. Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez.

Lorsque que l'on essaye d'exécuter le fichier, un fichier vierge s'affiche avec l'information "permission denied".
Avec sudo, il faut rentrer le mot de passe root pour lire le fichier.

5. Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou aﬀichez le contenu du fichier fichier. Qu’en déduisez-vous ? Rétablissez le droit en lecture sur test.

On ne peut pas supprimer le droit de lecture du dossier lorsque l'on est déjà à l'intérieur de celui-ci.

6. Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvez- vous déduire de toutes ces manipulations ?
```
bash
chmod a-w sstest/
chmod a-w nouveau
```
La modification apportée à "nouveau" ne peut pas être enregistré, et il est demandé d'override les permissions por supprimer le fichier.

7. Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test. Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en lister le contenu, etc...Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?

Le droit d'éxécution bloque l'entrée dans un dossier, mais bloque aussi le fait d'effectuer quelconques modifications des fichiers qu'il contient.

8. Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd ..” ? Pouvez-vous donner une explication ?

On ne peut pas modifier les droits du dossiere dans lequel on se trouve.

9. Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suﬀisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.
```bash
chmod  g+r fichier
```
10. Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.
```bash
umask 007
```
11. Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos réper- toires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.
```
bash
umask 755
```

12. Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.
```
bash
umask 744
```

14. Aﬀichez les droits sur le programme passwd. Que remarquez-vous ? En aﬀichant les droits du fichier /etc/passwd, pouvez-vous justifier les permissions sur le programmepasswd?

```bash
-rw-r--r--   [...] passwd
```
On observe que personne n'a les droits.
