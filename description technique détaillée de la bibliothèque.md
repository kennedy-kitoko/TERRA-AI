Voici une description technique détaillée de la bibliothèque **TerraSoil**, basée sur les registres spécifiés dans votre fichier `Npkv2.pdf` et sur l'architecture matérielle ESP32.

### 1. Description de la Bibliothèque TerraSoil

La bibliothèque **TerraSoil** est une couche d'abstraction logicielle (middleware) conçue pour transformer le protocole industriel **Modbus RTU** en objets de données simples pour les développeurs.

**Rôle principal :**

* **Orchestration du Bus RS485 :** Elle gère automatiquement le basculement de la broche **RTS (DE/RE)** pour passer du mode réception au mode émission (Half-Duplex).
* **Traductions de Registres :** Elle convertit les adresses hexadécimales brutes du capteur en variables compréhensibles (Humidité, pH, etc.).
* **Sécurisation des Données :** Elle calcule et vérifie le **CRC16** (Cyclic Redundancy Check) pour s'assurer qu'aucune interférence électromagnétique n'a modifié les valeurs pendant le transport sur le câble.
* **Gestion Temporelle :** Elle respecte les timings critiques du protocole RS485 (silence entre les trames, timeouts de réponse).

---

### 2. Correspondance des Registres (Selon Npkv2.pdf)

Le fichier technique `Npkv2.pdf` définit des adresses spécifiques pour chaque donnée. Voici comment la bibliothèque TerraSoil les utilise :

| Paramètre | Registre (HEX) | Adresse Décimale | Description technique |
| --- | --- | --- | --- |
| **Moisture** | `0x0000` | 0 | Humidité volumétrique du sol (). |
| **Temperature** | `0x0001` | 1 | Température ( °C, gère les valeurs négatives). |
| **Conductivity** | `0x0002` | 2 | Conductivité électrique (EC) en µS/cm. |
| **pH** | `0x0003` | 3 | Potentiel Hydrogène ( pH). |
| **Nitrogen (N)** | `0x0004` | 4 | Teneur en Azote (mg/kg). |
| **Phosphorus (P)** | `0x0005` | 5 | Teneur en Phosphore (mg/kg). |
| **Potassium (K)** | `0x0006` | 6 | Teneur en Potassium (mg/kg). |
| **Salinity** | `0x0007` | 7 | Teneur en sel (mg/L). |
| **TDS** | `0x0008` | 8 | Total des Solides Dissous. |
| **Fertility** | `0x000C` | 12 | Indice de fertilité global. |

---

### 3. Composants et Architecture Matérielle Utilisés

Pour fonctionner, la bibliothèque s'appuie sur les composants physiques suivants :

#### A. Le Contrôleur UART de l'ESP32

La bibliothèque utilise le périphérique **HardwareSerial** de l'ESP32. Elle exploite les registres matériels internes de l'ESP32 mentionnés dans votre documentation technique :

* `UART_RS485_CONF_REG` : Pour activer le support du mode RS485.
* `UART_RS485_EN` : Active la logique spécifique au bus différentiel.

#### B. Le Transceiver RS485 (Circuit B)

L'ESP32 ne peut pas parler directement au capteur (niveaux de tension différents). La bibliothèque pilote un composant intermédiaire (type **MAX485** ou **ADM483**) selon le **Circuit B (Manual Switching)** :

1. **Broche RTS (DE/RE) :** Connectée à une GPIO de l'ESP32. La bibliothèque met cette broche à `HIGH` pour envoyer la commande au capteur, puis à `LOW` immédiatement après pour écouter la réponse.
2. **Lignes A et B :** Le bus différentiel qui relie physiquement le capteur sur de longues distances (jusqu'à 1200m).

#### C. Le Capteur 10-en-1 (Esclave Modbus)

* **Sondes en acier 316L :** Elles agissent comme des électrodes pour mesurer la constante diélectrique (humidité) et la résistance (EC).
* **Microcontrôleur Interne :** Reçoit les requêtes de la bibliothèque TerraSoil, lit les capteurs analogiques, et renvoie les 10 valeurs formatées en 16 bits.

**En résumé :** TerraSoil est le "traducteur" qui permet à l'ESP32 de commander le transceiver RS485 pour interroger précisément les registres de mémoire du capteur définis dans le manuel NPKV2.
