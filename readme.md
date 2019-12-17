DEPLOY SECURE APPS (part one)
===
![center](/chateau.jpg)

<h3>Lundi</h3> 

**Retour alternance**

**Veille war, jar, scripts shell**
 
**Monday Algo live coding :** d'abord on écrit un prog en français, puis on le traduit en anglais, puis dans un langage de prog de votre choix et on teste le tout !	

**Déployer le back de l'appli E-commerce localement puis lancer à partir du terminal (Ide fermé)**

**Idem pour le front**

**Réaliser un script shell pour démarrer l'appli côté back**

Bonus : ajouter dans votre script un moyen de créer les répertoires ecom puis products s'ils existent pas puis tester, tenter de rendre générique votre script afin qu'il soit executable dans n'importe quel serveur

<h3>Mardi</h3> 

**Veille PaaS, SaaS, DaaS, IaaS...Heroku**

**Réaliser une application simple de gestion de noms d'entreprises puis déployer là sur Heroku**

51.1 : Créer un compte sur heroku.com (sans donner votre CB :) 

51.2 : Installer Heroku sur votre machine

51.3 : Créer une appli spring boot simple avec un controller rest qui affiche "bonjour" puis tester localement d'abord

51.4 : Sur le terminal, au niveau de votre projet, créer une instance d'application en ligne ("heroku create" qui va assigner une url externe et créer un repo git)

51.5 : Vérifier dans votre console de gestion en ligne l'url puis renommer là en qqchose de plus élégant

51.6 : Utiliser git pour uploader votre application, heroku se chargera automatiquement de la redéployer (git init/add/commit/remote/push...) puis vérifier

*Nous souhaitons dorénavant, travailler avec le SGBD Postegresql, cela ne change rien à notre manière habituelle de travailler*

52.1 : Ajouter la dépendance à Maven dans votre projet (org.postgresql...)

52.2 : Ajouter à votre projet une classe Company (id + name), décorer là pour en faire une entité Jpa

52.3 : Ajouter l'interface CompanyRepository puis des instances de Company dans votre appli

52.4 : Enfin, Ajouter dans votre controller une méthode qui renvoi toutes les Company en base

52.5 : Ajuster votre fichier application.properties

*NB -> Ce n'est pas indispensable ici mais vous pouvez tester votre application localement, il faudra juste installer Postegresql*

*De retour sur votre console de gestion en ligne heroku*

53.1 : Sur votre tableau de bord (dashboard), ajouter à votre appli en ligne un sgbd (configure add-ons)

53.2 : Mettre à jour les changements (git init/add/commit/push...) puis tester

**Bonus : Refaire de même avec la 1ère version de l'appli de E-Commerce puis lancer votre front avec l'url du back en ligne**

<h3>Mercredi</h3> 

**Veille Failles sécurité + solutions**

**Application de gestion de taches : MISE EN OEUVRE DE LA COUCHE SPRING SECURITY**

Nous allons réaliser dans un premier temps la partie backend d'une application de gestion de taches consistant à consulter et ajouter des taches en fonction des roles (User ou Admin) des utilisateurs connectés, et ce en utilisant spring security et jwt.
- Respectivement, un utilisateur connecté (USER), pourra consulter la liste des taches 
- Un administrateur connecté (USER + ADMIN), pourra en plus, ajouter des taches

*De retrour sur votre Ide préféré*

54.1 Créer un projet avec les dependances suivantes habituelles : DevTools, Lombok, Spring data Jpa, H2, PostegreSql (mettre en commentaire), Spring Security(en commentaire), Spring Web. Puis ajouter manuellement Jwt en recherchant "JJwt + maven dependancy"

54.2 Crée l'entité Task (entities) puis l'interface Jpa associé (dao)

54.3 Mettre en oeuvre un controller rest (web) qui renvoi la liste des taches puis l'ajout d'une tache

54.4 Tester avec l'outil ARC (advanced Rest Client)

*Personnaliser la configuration de Spring Security (quasiment comme dans ma banque en ligne)*

55.1 Ajouter une classe (security) SecurityConfig (enlever les commentaires dans pom.xml) avec d'abord une authentification en mémoire.

55.2 Mise en oeuvre d'un formulaire d'authentification fourni par Spring 

55.3 Indiquer à Spring que toutes les ressources de l'appli necessite une authentification

*Jusqu'ici Spring utilise la securité basée sur les cookies, sur le formulaire d'authentification, il génère un token(csrf), les autres applis ne pourront pas en faire autant, de plus nous allons utiliser Jwt ici aussi il faut donc :*

56.1 Désactiver la génération automatique du synchronized token

*Nous avons utilisé l'authentification en mémoire (inMemoryAuthentication), ici nous utiliserons l'authentification basée sur une couche service*

57.1 Dans votre classe SecurityConfig, remplacer le système actuel (en mémoire) par un service dédié (UserDetailService) que vous devrez donc implémenter

57.2 Utiliser pour l'encodage du mot de passe saisi par l'utilisateur BCryptPasswordEncoder 

57.3 Ajouter les entités AppRole et UserRole avec leurs interfaces Jpa afin de gérer en base des utilisateurs avec des rôles respectifs

57.4 Réaliser l'interface AccountService avec les méthodes : ajouter user, rôles, rôles à des utilisateurs, trouver user par son userName

57.5 Implémenter l'interface AccountService

57.6 Implémenter maintenant l'interface UserDetailService afin de permettre à Spring de trouver un utilisateur à partir de notre interface AccountService

57.7 Ajouter des instances d'utilisateurs, de rôles et de rôles par utilisateurs dans l'appli puis tester 

57.8 Attribuer au rôle ADMIN la possibilitée d'ajouter des taches

57.9 Tester pour admin puis user qui ne doit pas pouvoir ajouter de taches

57.10 Ajouter enfin ici un controller rest permettant l'ajout de nouveaux utilisateurs avec un rôle par défaut(USER)
-> Sécurité oblige, trouver un moyen de ne pas renvoyer le mot de pass même crypté après un enregistrement, tout en gardant sous le bras le mot de pass pour confirmation de saisie !!!

<h3>Jeudi</h3>

**Veille JWT et Keycloak**

**MISE EN OEUVRE DE LA COUCHE JWT**

Adieu authentification basée sur les sessions (références), bienvenue à l'authentification basée sur les tokens(valeurs)**

58.1 Désactiver l'authentification basée sur les sessions -> demander à Spring d'utiliser le mode stateless

*étape 1 : première connection donc génération d'un token*

58.2 Ajouter le FILTRE JWT D'AUTHENTIFICATION (security) avec les 2 méthodes d'authentification (attempt & success)

58.3 Tester en vous connectant puis récupérer le token 

58.4 Visualiser(copier/coller) le contenu sur jwt.io, pratique pour voir les roles, n'est-ce pas ?

*étape 2 : deuxième connection donc vérification du token*

58.5 Mise en oeuvre d'un FILTRE JWT D'AUTHORISATION qui recevra systhématiquement toutes les requêtes pour vérification avant orientation


<h3>Vendredi</h3> 

**Veille sur Google Cloud + réussir sa présentation orale**

Terminer tous les travaux de la semaine et les envoyer sur github puis transferer à vos tuteurs(rices) et formateur

**Retour sur les anciens projets et aux problématiques associées**

**Dossier professionnel**

**Dossier projet**

**Programme vacances**

RESSOURCES
===

[Deploying Spring Boot Applications](https://docs.spring.io/spring-boot/docs/current/reference/html/deployment.html)

[Introduction aux scripts shell](https://openclassrooms.com/en/courses/43538-reprenez-le-controle-a-laide-de-linux/42867-introduction-aux-scripts-shell)

[Tuto Spring Boot App with Heroku](https://www.youtube.com/watch?v=KDK5xXPJVIg)

[Spring Security with Jwt](https://youtu.be/UspQ6arrMiw)

[Spring Boot App with Keycloack](https://www.youtube.com/watch?v=0cziL__0-K8)
