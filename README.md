# shelly est desormais intégré nativement dans home assistant
et contrairement a la version presente dans HACS , shelly est maintenant complètement pris en charge, en local
mais le tuto ci apres est toujours valable 
 
# Utilisation du Shelly 1 Mini Gen3 avec Home Assistant

Le Shelly 1 Mini Gen3 peut être utilisé sans passer par l'intégration officielle de Home Assistant.
Voici comment utiliser les commandes HTTP pour contrôler le relais.
Vous avez tous les schemas de câblage en bas.

![alt text](shelly_1_mini_gen3.jpg?raw=true)


## Configuration des adresses IP fixes

Pour faciliter l'accès aux relais, configurez des adresses IP fixes dans votre routeur. Voici les adresses IP utilisées dans cet exemple :

- **shelly_portail** : `http://192.168.1.159`
- **shelly_arbre** : `http://192.168.1.86`
- **shelly_maison** : `http://192.168.1.133`

## Commandes HTTP pour contrôler le relais

Le Shelly 1 Mini Gen3 peut être contrôlé via des commandes HTTP. Voici les commandes de base :

- **Allumer le relais** :
  ```
  http://<adresse_ip>/relay/0?turn=on
  ```

- **Éteindre le relais** :
  ```
  http://<adresse_ip>/relay/0?turn=off
  ```

- **Inverser l'état du relais** :
  ```
  http://<adresse_ip>/relay/0?turn=toggle
  ```

## Intégration dans Home Assistant

### Méthode 1 : Utilisation d'un fichier `rest_command.yaml`

1. Créez un fichier nommé `rest_command.yaml` dans le même répertoire que votre fichier `configuration.yaml`.

2. Ajoutez les commandes REST pour chaque relais :

    ```yaml
    shelly_portail_on:
      url: "http://192.168.1.159/relay/0?turn=on"
      method: GET

    shelly_portail_off:
      url: "http://192.168.1.159/relay/0?turn=off"
      method: GET

    shelly_arbre_on:
      url: "http://192.168.1.86/relay/0?turn=on"
      method: GET

    shelly_arbre_off:
      url: "http://192.168.1.86/relay/0?turn=off"
      method: GET

    shelly_maison_on:
      url: "http://192.168.1.133/relay/0?turn=on"
      method: GET

    shelly_maison_off:
      url: "http://192.168.1.133/relay/0?turn=off"
      method: GET
    ```

3. Importez cette configuration dans votre fichier `configuration.yaml` :

    ```yaml
    rest_command: !include rest_command.yaml
    ```

### Méthode 2 : Intégration directe dans `configuration.yaml`

Ajoutez directement les commandes REST dans votre fichier `configuration.yaml` :

```yaml
rest_command:
  shelly_portail_on:
    url: "http://192.168.1.159/relay/0?turn=on"
    method: GET

  shelly_portail_off:
    url: "http://192.168.1.159/relay/0?turn=off"
    method: GET

  shelly_arbre_on:
    url: "http://192.168.1.86/relay/0?turn=on"
    method: GET

  shelly_arbre_off:
    url: "http://192.168.1.86/relay/0?turn=off"
    method: GET

  shelly_maison_on:
    url: "http://192.168.1.133/relay/0?turn=on"
    method: GET

  shelly_maison_off:
    url: "http://192.168.1.133/relay/0?turn=off"
    method: GET
```

**Note :** Un redémarrage de Home Assistant est nécessaire après ces modifications.

## Création d'une automatisation

Voici un exemple d'automatisation pour contrôler l'éclairage extérieur :

```yaml
alias: Eclairage Extérieur V2
description: ""
triggers:
  - platform: state
    entity_id:
      - binary_sensor.portail_voiture_ouverture
    to: "on"
    id: allume
  - platform: state
    entity_id:
      - binary_sensor.portail_pieton_ouverture
    to: "on"
    id: allume
conditions:
  - condition: sun
    before: sunrise
    after: sunset
    enabled: true
actions:
  - service: rest_command.shelly_portail_on
    data:
      mode: auto
  - service: rest_command.shelly_arbre_on
    data:
      mode: auto
  - service: rest_command.shelly_maison_on
    data:
      mode: auto
  - delay:
      hours: 0
      minutes: 0
      seconds: 30
      milliseconds: 0
  - service: rest_command.shelly_portail_off
    data:
      mode: auto
  - service: rest_command.shelly_arbre_off
    data:
      mode: auto
  - service: rest_command.shelly_maison_off
    data:
      mode: auto
mode: restart
```

---

# Câblage Contact Sec
![alt text](calblage_contact_sec.jpg?raw=true)

# Câblage Contact Sec avec interrupteur
![alt text](calblage_contact_sec_avec_inter.jpg?raw=true)


# Câblage Mode Relai
![alt text](mode_relai.jpg?raw=true)

# Câblage Mode Relai avec interrupteur
![alt text](mode_relai_avec_inter.jpg?raw=true)


# Câblage Mode Relai avec bouton poussoir
![alt text](mode_relai_bouton_poussoir.jpg?raw=true)

# Câblage Mode Relai vas-et-vient
![alt text](mode_relai_vas_et_vient.jpg?raw=true)











