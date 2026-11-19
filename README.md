# Exploiter un cluster Kubernetes sur vSphere

Comment remplacer un nœud sans compromettre la disponibilité du cluster ? Cette sélection du projet d’équipe SLLYM illustre les vérifications et les opérations qui encadrent cette intervention dans un environnement Kubernetes à haute disponibilité.

## Dans ce dépôt

- [Santé du cluster](roles/cluster_health/tasks/main.yml) — sondes de l’API, état des nœuds et vérification d’etcd, la base qui conserve l’état du cluster.
- [Mise en maintenance](roles/node_drain/tasks/main.yml) — vérifie la présence du nœud, empêche les nouvelles affectations et peut évacuer les charges ou retirer l’objet Node selon son état et les options demandées.
- [Retrait d’un membre etcd](roles/etcd_management/tasks/remove_member.yml) — contrôle le quorum avant le retrait et exige une correspondance non ambiguë avec le membre à retirer. Une nouvelle exécution accepte le cas où le membre a déjà été retiré.
- [Préparation des machines](roles/containerd/tasks/main.yml) et [extension des volumes](roles/disk_expansion/tasks/main.yml) — configuration de l’environnement d’exécution des conteneurs et ajustement du stockage selon une disposition déclarative.

## Métriques avec Grafana

Le projet déployait la supervision avec Argo CD. Le [fragment Grafana](extraits/grafana/values-prometheus.yaml), adapté de la configuration du chart de supervision d’équipe, montre comment déclarer automatiquement Prometheus comme source de métriques. Il utilise une adresse fictive à remplacer dans un environnement d’essai.

## Projet d’origine et réutilisation

Le projet de fin d’études SLLYM, réalisé en équipe à l’ESGI, utilisait un module Terraform partagé pour créer les machines virtuelles sur vSphere et Ansible pour installer Kubernetes avec kubeadm. Les rôles présentés interviennent sur un cluster existant. Un remplacement complet demande le playbook d’orchestration, les inventaires et les variables du cluster. Le fragment Grafana s’utilise avec le chart Grafana qui rend ces valeurs.
