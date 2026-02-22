# Rapport de Laboratoire : Audit de Sécurité Mobile (Interception de Trafic)

## 1. Périmètre de l'Audit
* **Cible :** `testphp.vulnweb.com`
* **Environnement :** Android Emulator (Android 10)
* **Outil :** Burp Suite Community Edition v2026.1.4

## 2. Configuration du Laboratoire
* **IP Hôte :** `192.168.11.137`
* **Port Proxy :** `8082` (Burp Proxy Listener).
* **Configuration Android :** Proxy configuré en mode manuel sur le réseau Wi-Fi local.

## 3. Preuves d'Interception (Captures)

### A. Interface de l'Application Cible
Tentative de connexion sur le formulaire de login via le navigateur de l'émulateur.
![Interface Site](/site.png)

### B. Historique Global du Trafic (HTTP History)
Capture des différents points d'entrée consultés (`/login.php`, `/userinfo.php`, `/search.php`).
![Historique Burp](/History.png)

### C. Analyse d'une Requête de Connexion (POST)
Détails de l'envoi des identifiants (`uname=Rajaa`, `pass=Eddanir`). On observe que les données transitent sans aucun chiffrement.
![Requête POST](/requete_post.png)

### D. Analyse d'une Requête de Recherche (Search)
Capture des paramètres de recherche envoyés au serveur via `/search.php`.
![Requête Search](/requete_search.png)

## 4. Analyse des Vulnérabilités Observées
* **Défaut de Confidentialité :** L'utilisation du protocole HTTP au lieu de HTTPS expose les identifiants de l'utilisateur (`Rajaa`) et son mot de passe en clair sur le réseau.
* **Exposition de données via URL :** Certains paramètres de test sont visibles directement dans le chemin de la requête, facilitant le traçage.
* **Absence d'en-têtes de sécurité :** Les requêtes analysées ne présentent pas de mécanismes de protection contre l'interception ou le détournement de session côté client.

## 5. Recommandations Défensives
* **Migration TLS :** Forcer l'utilisation de HTTPS pour l'ensemble de l'application afin de protéger les données sensibles en transit.
* **Sécurisation des cookies :** Implémenter les attributs `Secure` et `HttpOnly` pour prévenir le vol de session.
* **Bonnes pratiques Android :** Désactiver les communications en clair dans le fichier `network_security_config.xml` de l'application mobile.

---
**Note de sécurité :** Conformément à l'Étape 8 du labo, le certificat CA utilisé pour l'analyse a été supprimé de l'émulateur après les tests pour garantir l'hygiène du système.