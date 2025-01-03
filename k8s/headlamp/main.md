# 📚 Une solide alternative à lens Headlamp pour manager vos clusters Kubernetes sur votre machine.

## Contexte : 
Depuis l'avènement du [dashboard Kubernetes](https://github.com/kubernetes/dashboard), plusieurs outils ont vu le jour pour faciliter la gestion vos clusters Kubernetes sur votre pc.
Jusque-là, lens était le plus avancé, mais il est passé de l'autre côté de la force 

Lens est acquis par la société [Mirantis](https://www.mirantis.com) qui a décidé de fermer les sources et de le rendre payant **(ça rappel docker également acquis par la même société)**.
[La version 6.0.0](https://forums.k8slens.dev/t/lens-6-release-and-vision-for-the-future/106) de Lens a été publiée le 28 juillet, 2022 et a été la dernière version complètement gratuite avec toutes les fonctionnalités.

Mirantis propose une version **personnelle** gratuite de Lens, mais avec quelques limitations, voir le [pricing model](   https://k8slens.dev/pricing)

### Alternatives à Lens :
- Il existe un [fork](https://github.com/MuhammedKalkan/OpenLens) de lens, par **[Muhammed Kalkan](https://github.com/MuhammedKalkan)** qui s'appel *openlens* qui est open source et gratuit. <br />
    Le problème est que le fork est à l'arrêt à cause de la fermeture des sources de Lens comme expliqué dans le readme, donc sujet à des bugs et vulnérabilités. <br >
    La dernière version est la v6.5.2-366 en date du 30 juin 2023 (il faut trouver une solution alternative). <br />
    Il reste encore fonctionnel avec quelques menus disparus comme le menu des logs et d'exec dans un pod.<br />
    Pour rajouter ces menus, il suffit d'installer l'extension **@alebcay/openlens-node-pod-menu** et redemarrer Lens. <br />
- [k9s](https://k9scli.io/) est un outil open source, la courbe d'apprentissage est plus élevé, mais il est très puissant et très complet.
- 
    



mais ne sera plus maintenu, .


Vous aurez besoin d'un environnement Docker ou Docker compatible pour utiliser Kind :

- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Docker](https://docs.docker.com/get-started/get-docker/)  ou
- [Podman](https://podman.io/getting-started/installation) ou
- ou tout autre outil qui permet, de lancer un conteneur runtime compatible avec Docker comme [Orbstack](https://www.orbstack.dev/) disponible sur Mar ARM64.

Personnellement, Orbstack reste mon outil préféré pour les développements locaux.

### Installation

En fonction de votre système d'exploitation, [suivre](https://kind.sigs.k8s.io/docs/user/quick-start/#installation) la procédure d'installation.


**Exemple :** 
```bash
# Pour ARM Macs
[ $(uname -m) = arm64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.26.0/kind-darwin-arm64
chmod +x ./kind
sudo mv kind /usr/local/bin/
```

```bash
# Pour windows  
curl.exe -Lo kind-windows-amd64.exe https://kind.sigs.k8s.io/dl/v0.26.0/kind-windows-amd64
#Déplacer le fichier dans le dossier C:\votre-dossier\ 
#Ajouter le chemin du dossier dans le PATH
```
[modifier-le-path-de-windows-ajouter-un-dossier-au-path ](https://lecrabeinfo.net/modifier-le-path-de-windows-ajouter-un-dossier-au-path.html )


### Verifier l'installation en exécutant la commande suivante :
```bash
kind
```
Vous devriez voir une sortie similaire à celle-ci :

![kind](https://mombesoft-blog-post-1474x.s3.us-east-1.amazonaws.com/assets/kind-01.png)

#### ! Kind est installé et prêt à être utilisé 🎉. 

## Créer un cluster kubernetes avec kind :

Rien de plus simple, il suffit d'exécuter la commande suivante :
```bash
kind create cluster
```
![kind](https://mombesoft-blog-post-1474x.s3.us-east-1.amazonaws.com/assets/kind-create-cluster.png)

Kind va créer un cluster Kubernetes avec un seul nœud (machine linux ou conteneur docker dans notre cas), et le nommer par défaut `kind`.

Pour vérifier que le cluster est bien créé, il suffit d'exécuter la commande suivante :
```bash
kind get clusters
```
Pour créer un cluster avec un nom différent :
```bash
kind create cluster --name mon-cluster-x
```
Le boulot de kind est fait, vous pouvez maintenant utiliser votre cluster comme vous le feriez avec un vrai cluster Kubernetes entre guillemets.

## Afficher vos contextes kubernetes:
```bash
kubectl config get-contexts
```
Vous devriez voir un résultat similaire à ceci :
![kind](https://mombesoft-blog-post-1474x.s3.us-east-1.amazonaws.com/assets/kind-cluster-get-contexts.png)

#### Maintenant que vous avez un cluster kubernetes de prêt, vous pouvez par exemple déployer une application avec kubectl :
```bash
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --port=80 --target-port=80 --type=ClusterIP
```
Ceci va créer un déploiement nginx avec un service exposé sur le port 80.
Pour afficher les pods, les services et les déploiements :
```bash
kubectl get pods,svc,deploy
```
L'IP du service affiché n'est pas accessible depuis l'extérieur, vous devez donc utiliser le port-forwarding pour accéder à l'application :
```bash
kubectl port-forward service/nginx 8080:80
```
![kind](https://mombesoft-blog-post-1474x.s3.us-east-1.amazonaws.com/assets/kubectl-nginx.png)

Vous pouvez maintenant accéder à l'application en allant sur l'url suivante : http://localhost:8080
![kind](https://mombesoft-blog-post-1474x.s3.us-east-1.amazonaws.com/assets/nginx.png)

## Supprimer un cluster kind :

Après avoir fini de travailler avec votre cluster, vous pouvez le supprimer en exécutant la commande suivante :
```bash
kind delete cluster
# ou spécifiez le nom du cluster à supprimer si vous l'avez nommé différemment :
kind delete cluster --name mon-cluster-x
```

## Les alternatives à kind : 
Il existe une multitude d'outils pour créer des clusters Kubernetes locaux, voici quelques-uns :

Similair à kind :
- [minikube](https://minikube.sigs.k8s.io/docs/start/) Le tout prémier outil pour créer des clusters Kubernetes locaux.
- [k3d](https://k3d.io/) Très similaire à kind, mais utilise k3s en arrière-plan.
- [k0s](https://k0sproject.io/)

Version desktop :

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) (gros consommateur de RAM)
- [Rancher Desktop](https://rancherdesktop.io/) 
- [Podman Desktop](https://podman-desktop.io/)
- [Orbstack](https://orbstack.dev/) super outils en plus d'émuler k8s, il est capable de gérer des machines virtuelles, pour le moment disponible que sur (Mac ARM64 seulement). Consomme très peu en resources.
 

## Conclusion
Kind est un outil très pratique pour créer des clusters Kubernetes locaux pour développer et tester des applications.
Je l'ulitilse beaucoup pour tester de nouveaux concepts et outils dans kubernetes, sans avoir à monter tout une infra complète.
## Tips et astuces
Vous pour créer un cluster avec plusieurs noeuds (conteneurs) :
```bash
kind create cluster --name mon-cluster-x --config=kind-config.yaml
```

Ci-dessous un exemple de fichier de configuration pour un cluster kubernetes HA (Haute disponibilité) avec 3 controleurs et 3 workers :
```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
  - role: control-plane
  - role: control-plane
  - role: worker
  - role: worker
  - role: worker
```
## Références
- [kind](https://kind.sigs.k8s.io/)
- [minikube](https://minikube.sigs.k8s.io/docs/start/)
- [k3d](https://k3d.io/)
- [k0s](https://k0sproject.io/)
- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- [Rancher Desktop](https://rancherdesktop.io/)
- [Podman Desktop](https://podman-desktop.io/)
- [Orbstack](https://orbstack.dev/)
- [k3s](https://k3s.io/)
- [Le crab](https://lecrabeinfo.net/modifier-le-path-de-windows-ajouter-un-dossier-au-path.html )