# D-tection-d-intrusion-avec-Snort-sur-pfSense

### Étape 1 : Installation de l'IDS/IPS Snort sur pfSense

1. ce Connecterl'interface web de pfSense.
2. Accédez à "System" > "Package Manager".
3. Dans l'onglet "Available Packages", on cherche "snort" et installer.

### Étape 2 : Configuration de Snort

1. Une fois l'installation terminée, accédez à "Services" > "Snort" pour configurer Snort.

2. ![image](https://github.com/manmaryem/D-tection-d-intrusion-avec-Snort-sur-pfSense/assets/137881827/e914e7e8-656f-4860-8c4c-1be98f346d83)

![image](https://github.com/manmaryem/D-tection-d-intrusion-avec-Snort-sur-pfSense/assets/137881827/6fca5c4b-ddfd-47b1-95e4-a112f3de98c3)


### Étape 3 : Configuration des règles personnalisées

Pour bloquer spécifiquement les attaques SSH par force brute et les scans de ports (nmap), on crée des règles personnalisées. Dans l'onglet "Custom Rules", ajoutez les règles suivantes :

![image](https://github.com/manmaryem/D-tection-d-intrusion-avec-Snort-sur-pfSense/assets/137881827/02dd12e2-7ff9-4faa-b6ad-cd6befc5d15d)


![image](https://github.com/manmaryem/D-tection-d-intrusion-avec-Snort-sur-pfSense/assets/137881827/29ab9f17-ac27-4866-b92b-b1ff2b9afb3c)


#### Bloquer les attaques SSH par force brute :
```
alert tcp $EXTERNAL_NET any -> $HOME_NET 22 (msg:"Possible SSH brute force attack"; threshold: type limit, track by_src, count 5, seconds 60; sid:1000001;)
block drop tcp $EXTERNAL_NET any -> $HOME_NET 22 (msg:"SSH brute force detected, blocking"; sid:1000002;)
```
![image](https://github.com/manmaryem/D-tection-d-intrusion-avec-Snort-sur-pfSense/assets/137881827/1c6c97e7-8ca7-4891-8b23-d1dbec87b8ba)

![image](https://github.com/manmaryem/D-tection-d-intrusion-avec-Snort-sur-pfSense/assets/137881827/cf9264c0-74dd-449f-8c2e-855b0b55bd6f)


#### Bloquer les scans de ports (nmap) :
```
alert ip $EXTERNAL_NET any -> $HOME_NET any (msg:"Possible Nmap port scan"; threshold: type limit, track by_src, count 5, seconds 60; sid:1000003;)
block drop ip $EXTERNAL_NET any -> $HOME_NET any (msg:"Nmap scan detected, blocking"; sid:1000004;)
```
![image](https://github.com/manmaryem/D-tection-d-intrusion-avec-Snort-sur-pfSense/assets/137881827/a658d20c-1568-4b95-8b43-acea4c45ce42)


- Ces règles définissent des seuils pour le nombre d'attaques autorisées en un laps de temps donné avant de bloquer l'adresse source.

### Étape 4 : Tests et vérification

![image](https://github.com/manmaryem/D-tection-d-intrusion-avec-Snort-sur-pfSense/assets/137881827/596dccfb-3162-465e-b1c9-f5e95e7697a0)


### Étape 5 : Vérification des logs dans l'onglet "Blocked"

- à l'onglet "Blocked" de Snort pour vérifier que les adresses IP sources des attaques sont bien listées comme bloquées.

  ![image](https://github.com/manmaryem/D-tection-d-intrusion-avec-Snort-sur-pfSense/assets/137881827/70eb4da8-8e42-4bc2-91a8-5fc50a2909ac)

  ![image](https://github.com/manmaryem/D-tection-d-intrusion-avec-Snort-sur-pfSense/assets/137881827/aa6288b9-ffb6-4987-9751-dc48c8d2fd78)



