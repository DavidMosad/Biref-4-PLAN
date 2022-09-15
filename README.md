# Biref-4-PLAN


Brief 4 : Terraform + Ansible + scaling = 🤯
Contexte du projet

Vous êtes de retour de vacances bien salutaires. Néanmoins, vos congés ont été ternis par le goût amer de l’imperfection du script de déploiement sur lequel vous avez travaillé avant de partir.

Alors dès votre reprise, vous creusez les tréfonds du Web à la recherche de la solution ultime. Au fur et à mesure de votre cheminement dans les méandres de l’Internet, vous remarquez des mots qui reviennent souvent : “Terraform”, “Ansible”, “Chef”, “Puppet”, peut-être même “Docker”, mais pas “Kubernetes”, non. Pas pour cette fois.

Ainsi, vous demandez-vous, serait-il plus efficace d’utiliser des outils conçus pour le déploiement automatisé plutôt que de réinventer la roue ?
Ces outils, utilisés par des milliers, des dizaines de milliers, peut-être même des centaines de milliers d’informaticiens de par le monde seraient-ils plus puissants que votre script ?

Un seul moyen de le savoir : essayer !

Vous vous sentez contrit, mais vous savez maintenant quoi faire pour expier vos péchés :

    Concevoir une infrastructure sécurisée, résiliente et scalable
    Utiliser Terraform et Ansible pour déployer
    Créer un Scale set pour configurer le scaling
    Monitorer l’application et ses ressources
    Sauvegarder les données de l’application
    Implémenter un script de test de charge
    Expérimenter le scaling
    Rédiger les documentations et présentations

Modalités pédagogiques

Tous les déploiements sont à faire en utilisant soit Terraform, soit Ansible.
Vous déploierez une des applications suivantes :

    Nextcloud,
    Jenkins,
    Gitea,
    Nagios,
    Wordpress,
    ou une application de votre choix (soumis à validation de Bryan ou d’Alfred).

Vous pourrez déployer des containers Docker si vous le souhaitez.

Toutes les données de l’application seront stockées en base de données ou dans un espace de stockage.

La méthode Scrum sera mise en place, avec un Scrum de Scrum tous les lundis.

Concernant la durée du brief, un point sera fait le 07/09 pour décider d’une semaine d’extension ou non.
Chapitre 0 : préparation

    Préparer minutieusement votre plan projet comprenant :
    - une topologie de l’infrastructure,
    - la liste des ressources Azure,
    - la liste des tâches à faire,
    - la stratégie de scaling,
    - les tests et métriques de monitoring,
    - le plan de test de charge,
    - la politique de backup.
    Créer un projet Github
    Créer un dépôt Github associé au projet
    Reporter la liste des tâches dans le Kanban
    Restitution collective en groupe jeudi matin

Chapitre 1 : déploiement d’une infrastructure minimale

    Déployer un resource group
    Déployer un vnet
    Déployer un subnet
    Déployer une IP publique avec une étiquette DNS (DNS label)
    Déployer une VM linux
    Se connecter en SSH sur la VM pour exécuter une commande

Chapitre 2 : (si nécessaire) déploiement d’une base de données

    déployer une instance Azure Database for MariaDB (ou PostgreSQL)
    Créer une règle de firewall pour autoriser la VM application
    installer sur la VM applicative le paquet mysql-client (ou postgresql-client-14) et utiliser la commande mysql pour tester la connection à la base de données

BONUS :

    Créer un private endpoint connection pour la VM applicative au lieu d’une règle de firewall

Chapitre 3 : (si nécessaire) déploiement d’un espace de stockage

    Créer un compte de stockage
    Créer un espace de stockage SMB avec un quota de 5GO
    Ajouter une règle ACL pour autoriser la VM applicative
    installer sur la VM applicative le paquet cifs-utils et tester le montage de l’espace de stockage

BONUS :

    Créer un private endpoint connection pour la VM applicative au lieu d’une règle ACL

Chapitre 4 : script cloud-init

    Implémenter un script cloud-init qui installe et configure l’application choisie
    Vérifier que l’application est bien disponible depuis le web en utilisant le FQDN de la VM

Chapitre 5 : déploiement d’une application gateway

    Créer un subnet dédié
    Déployer l’application gateway en utilisant l’adresse IP publique de la VM applicative. Le backend est la VM applicative.

Chapitre 6 : mise en place de TLS

    Créer un Azure Key Vault
    Créer un compte de stockage public
    Créer un conteneur de stockage
    Dans ce conteneur de stockage, créer un chemin “/.well-known/acme-challenge”
    Créer un blob avec un contenu aléatoire dans ce chemin
    Configurer une règle de chemin sur l’Application gateway qui redirige “/.well-known/acme-challenge/*” vers le conteneur de stockage
    Tester l’URL depuis un navigateur web
    Implémenter un script qui :
        crée un certificat via Certbot avec le challenge http-01
        qui crée un blob dans le conteneur de stockage avec le bon nom et le bon contenu pour résoudre le challenge.
        Ajoute le certificat dans le vault
    Vérifier via votre navigateur que le site est bien considéré comme sûr

BONUS :

    Utilisez le service Azure Automation pour vérifier la date d’expiration du certificat et le renouveler 10 jours avant expiration.

Chapitre 7 : Monitoring de l’application

    Activer le monitoring de la VM dans Azure Monitor
    Activer le cas échéant le monitoring de la base de données et/ou de l’espace de stockage dans Azure Monitor
    Activer le monitoring de l’application dans Azure Monitor
    Créer un workbook (classeur) avec les métriques pertinentes à suivre pour vos ressources et votre aplication
    Créer une alerte en cas d’indisponibilité de l’application
    Créer une alerte en cas d’usage CPU > 90% sur la VM applicative
    Créer une alerte si la date d’expiration du certificat TLS est < 7 jours
    Créer une alerte si l’espace disponible sur l’espace de stockage < 10%
    Configurer les alertes pour envoyer des emails à votre groupe et aux formateurs

Chapitre 8 : script de test de montée en charge

    Implémenter un script qui effectue des requêtes HTTP à l’application de manière massive et rapide.
    Constater l’impact du script sur l’usage des ressources de la VM applicative.

BONUS :

    Créer une ressource Azure Load Testing pour effectuer un test de charge
    Créer un Quick test
    Exécuter le test et constater l’impact du test sur l’usage des ressources de la VM applicative

Chapitre 9 : backup

    Créer un backup de l’espace de stockage le cas échéant
    Créer un backup de la base de données le cas échéant
    Supprimer des données dans la base et/ou dans l’espace de stockage
    Vérifier que l’application ne fonctionne plus
    Attendre 10 minutes
    Restaurer le backup
    Vérifier que l’application fonctionne à nouveau
    Vérifier que la défaillance apparaît dans Azure Monitor

Chapitre 10 : scale set

:warning: Échange des projets entre équipes sur base d’un transfert de dépôt Git et de documentation. Une réunion de passation d’une durée maximale de 30 minutes sera à planifier.
Ceux qui ont choisi Terraform récupèrent un projet Ansible et vice-versa.

    Créer un VM scale set pour la VM applicative
    Exécuter un scale out de 8 replica
    Exécuter le test de montée en charge
    Constater l’impact du test sur les VMs
    Exécuter un scale in de 2 replica
    Exécuter le test de montée en charge
    Constater l’impact du test sur les VMs

Chapitre 11 : auto scale

    Configurer le profile d’autoscale du scale set
    Créer une règle de scale out
    Créer une règle de scale in
    Exécuter le test de montée en charge
    Constater l’impact du test sur les VMs

Critères de performance

    Le code est lisible (clair et facilement compréhensible)
    Les scripts s’exécutent correctement et déploient automatiquement l’application
    L’application est correctement monitorée et sauvegardée
    Le plan exécuté est proche du plan prévu
    la méthode Scrum a été suivie
    la passation de votre travail au chapitre 10 a donnée satisfaction

Modalités d’évaluation

Restitution en groupe.
Relecture commentée de vos livrables par les formateurs.
Livrables

    documentation des infrastructures (Terraform et Ansible)
    lien vers les dépôts des sources
    un executive summary de votre travail (technique et organisationnel) à présenter en groupe
    dans votre executive summary, vous présenterez le fonctionnement de Terraform ou d’Ansible
    votre avis sur la réception du travail de l’autre groupe

Objectifs

À l’issue de ce brief, vous aurez :

    mieux anticipé vos actions que dans le brief précédent
    déployé de nouvelles ressources dans Azure
    implémenté des scripts
    utilisé Terraform
    utilisé Ansible
    écrit du YAML
    documenté votre travail
    présenté votre travail
    pratiqué Scrum
    abordé la gestion de projet
    abordé la communication en équipe
    utilisé certbot pour déployer un certificat TLS
    sauvegardé les données applicatives
    monitoré l’application et la VM
    configuré de l’alerting
    scalé une application
    subi le travail des autres

