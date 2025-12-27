# CI-CD Bobapp
Lien SonarCloud : https://sonarcloud.io/project/overview?id=Sapotte_CI-CD
Lien DockerHub : https://hub.docker.com/repository/docker/salsh90

## Workflow

Lors de du **push** à partir de n'importe quelle branche pour la partie **CI** : intégration avec vérification du build (compilation, vérification des tests et de la couverture) et de la qualité de code avec Sonar ou lors du **merge** de la branch develop vers la main pour la partie **CD** (delivery, avec création des images Docker back et front). On effectue la partie **CD** (docker) que lorsque le push est effectué sur la branche main.
Si un pipeline est déjà lancé lors de la création d'une nouvelle, la plus ancienne est annulée afin de gagner du temps.

### Etape 1 : Vérification du build et vérification de la qualité de code avec Sonar
Ne se fait que lors du **push** sur la branche develop.
On évite de le refaire lors du merge de la branche develop vers la main.
#### Checkout : Permet de récupérer le code du repository pour effectuer le build
#### Setup du JDK et de Node nécessaires aux build back et front
#### Build back et front avec vérification des tests et de la couverture + génération des rapports de tests 
* Couverture API : *[index.html](back/target/site/jacoco/index.html)*
* Couverture front: *[index.html](front/coverage/bobapp/index.html)*
#### Téléchargement des rapports de tests
#### Sonar : analyse du code avec Sonar : permet de vérifier la qualité du code (bugs/vulnérabilités/code smells/duplications/couverture) → empêche de merger du mauvais code
*Remarque : setup d'un JDK 17 nécessaire pour l'analyse Sonar*

### Etape 2 : Publication des images Docker
Se fait lors du merge de la branche develop vers la main. 
On publie la version se trouvant sur main après le merge → permet de publier une version préalablement vérifiée
Permet de créer des images Docker pour le back et le front, ce qui facilite le déploiement.
On ne build qu'une seule fois et on peut les lancer n'importe où ("Build once, run anywhere")


## KPIs proposés
 * Couverture des tests > 70% back et front : vérifie le bon fonctionnement du code et empêche les régressions sur le nouveau code (actuellement trop basse, augmenter la couverture minimale de test à chaque nouvelle feature afin d'éviter les régressions sur la couverture)
 * Qualité de code sur le nouveau code :
    * Aucune erreur bloquante (blocker issue)
    * Aucune erreur critique (critical issue)
Cela permettra d'avoir un code plus maintenable, d'éviter les régressions et les erreurs de code.
    ### Métriques actuels
    Le code n'est pas assez testé et laisse "passer" des erreurs en prod qui pourraient être évitées, les nouvelles fonctionnalités peuvent entraîner des régressions su le code existant.
    Plusieurs défauts bloquants ou critiques sont remontés par Sonar, ces défauts peuvent avoir un impact sur la fiabilité, la sécurité de l'application.

## Analyse des avis utilisateurs
* Spinner infini lors du post d'une blague : peut-être dû à une requête qui ne finit jamais ou une erreur back non gérée correctement côté front ou une fuite de mémoire Angular. 
Cette problématique semble être nouvelle, elle est donc sûrement due à une nouvelle fonctionnalité ayant "cassé" celle-ci, la présence de tests front et back auraient prévenu cette régression.
* Bug sur le post d'une vidéo + demande non traitée : manque de tests et vérification de la qualité de code absente ce qui empêche la détection précoce des bugs. 
* Ne plus rien recevoir + délai de réponse important : encore une fois une régression mise en prod sans vérification préalable grâce aux tests et à la vérification de la qualité du code. Le manque d'automatisation entraîne une complexité dans la mis en place de nouvelles features ou de correction des bugs. La CI/Cd permettra de facilité les livraisons avec des images fiables. On pourrait aussi mettre en place un environnement de reette permettant de déployer sur un environnement hors-prod et tester fonctionnellement les nouvelles fonctionnalités et faire une autre vérification sur les régressions.
* Suppression du site des favoris : une mauvaise maintenance de l'application entraîne une multiplication des bugs et une insatisfaction des utilisateurs La CI/CD permettra une facilité d'intégration et de livraison du code avec une meilleure maintenabilité et une découverte en amont des bugs.



