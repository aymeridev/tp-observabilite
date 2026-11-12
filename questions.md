1. Prometheus collecte les métriques des différentes cibles. Il les stocke ensuite dans une base de données temporaire.
- Grafana sert à la visualisation. Il ne stocke pas de données, interroge directement les sources de données (Prometheus) sous forme d'un dashboard.

2. C'est un service qui expose les métriques système d'une machine Linux (CPU, mémoire, etc.)

3. C'est le mode de collecte de Prometheus. chaque scrape_interval Prometheus envoie une requête à l'endpoint /metrics de chaque cible.
Il lit les métriques renvoyées et les enregistre avec un horodatage.

4. C'est le fichier de configuration de Prometheus. Il définit:
- les paramètres globaux, comme scrape_interval et evaluation_interval
- les jobs de scraping (scrape_configs), c'est à dire les cibles  à interroger.

5. Les conteneurs sont sur le même réseau Docker. Le DNS interne de Docker traduit le nom du service (prometheus) en adresse IP du conteneur. Grafana tourne lui-même dans un conteneur: pour lui, localhost désigne son propre conteneur.

6. PromQL est le langage de requête de Prometheus. il sert à sélectionner, filtrer et calculer des séries temporelles.

7. Pour rendre les données persistantes sur le disque. Un conteneur est éphémère : s'il est supprimé ou recréé, tout ce qui se trouve dans son système de fichiers disparaît.

8. Le dernier scrape a échoué : timeout, service arrêté, erreur réseau ou réponse HTTP invalide. cela fait que plus aucune nouvelle donnée n'est collectée pour cette cible, ce qui crée des trous dans les graphiques Grafana.

9. Pour détecter les problèmes avant qu'elles ne causent une panne : un disque plein fait planter les applications, un manque de RAM, ou encore un CPU à 100 % qui entraînera de la latence.

10. UP : le dernier scrape a réussi, la cible répond correctement.
- DOWN : le dernier scrape a échoué, la cible est injoignable ou répond mal

11. node_cpu_seconds_total est un counter: il cumule le nombre de secondes passées par le CPU dans chaque mode depuis le démarrage, et ne fait qu'augmenter. Sa valeur brute n'a donc pas de sens. `rate` calcule la vitesse d'augmentation par seconde sur une fenêtre de temps.

12. lo est l'interface de loopback. Son trafic reste interne à la machine et ne passe jamais par le réseau. L'inclure fausserait les mesures du trafic réseau réel en ajoutant des communications locales entre processus.