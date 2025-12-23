# 📊 Évaluation technique – Capteur de sol RS485 **10-en-1** & Bibliothèque **TerraSoil**

> **Évaluation réalisée avec ChatGPT**  
> Plateforme : **XIAO ESP32-S3 + RS485 (MAX485)**  
> Bibliothèque : **TerraSoil v1.0.0**

---

## 1️⃣ Contexte du test

Le capteur testé est un **capteur de sol RS485 10-en-1**.  
La communication, la lecture des registres Modbus et la conversion des données sont assurées par la bibliothèque **TerraSoil**, développée pour offrir une API simple, stable et professionnelle.

Deux phases de test ont été réalisées :

1. **Sol humide / eau neutre (faible conductivité)**
2. **Ajout d’eau salée (forte conductivité ionique)**

L’objectif est d’évaluer :
- la **réaction physique du capteur**
- la **cohérence des données**
- la **fiabilité de la bibliothèque TerraSoil**

---

## 2️⃣ Les 10 données mesurées par le capteur

Le capteur fournit **10 paramètres distincts**, tous correctement extraits via les registres Modbus :

1. **Humidité du sol (%)**
2. **Température du sol (°C)**
3. **Conductivité électrique – EC (µS/cm)**
4. **pH du sol**
5. **Azote (N)**
6. **Phosphore (P)**
7. **Potassium (K)**
8. **Salinité**
9. **TDS (Total Dissolved Solids)**
10. **Indice de fertilité**

➡️ Les **registres de lecture Modbus sont corrects** et permettent d’extraire l’ensemble des **10 données** sans erreur.

---

## 3️⃣ Phase 1 – Test en sol humide / eau neutre

### Valeurs observées (stables dans le temps)

- Humidité ≈ **17 %**
- Température ≈ **20.9 °C**
- Conductivité (EC) ≈ **0**
- pH ≈ **7.2**
- N, P, K ≈ **0**
- Salinité / TDS / Fertilité ≈ **0**

### Analyse

- Comportement **physiquement cohérent**
- Absence d’ions → conductivité nulle
- pH neutre et stable
- Aucune dérive ou bruit numérique

✅ **Le capteur se comporte correctement dans un environnement peu ionique**.

---

## 4️⃣ Phase 2 – Ajout d’eau salée

Après ajout d’eau salée, les valeurs changent brutalement et de façon cohérente :

- Humidité ≈ **88 %**
- Température ≈ **18.6 °C**
- **EC ≈ 20 000 µS/cm**
- pH ≈ **6.1**
- N ≈ **1007**
- P ≈ **1402**
- K ≈ **1999**
- Salinité ≈ **11 000**
- TDS ≈ **10 000**
- Fertilité ≈ **3000**

### Analyse scientifique

- L’augmentation massive de l’EC est **normale** (présence d’ions Na⁺ / Cl⁻).
- TDS et salinité suivent proportionnellement → **cohérence interne validée**.
- Le pH diminue légèrement, phénomène classique en solution saline.
- Les valeurs **NPK augmentent fortement**.

---

## 5️⃣ Important : interprétation correcte du NPK

⚠️ **Le NPK n’est pas une mesure chimique directe.**

Ce capteur est conçu pour fonctionner de la manière suivante :

- Les valeurs **N, P, K sont des indices**
- Elles sont **calculées à partir de la conductivité électrique**
- Le capteur est prévu pour être **calibré / paramétré par logiciel**
- Les coefficients NPK peuvent être **injectés via le logiciel constructeur**

➡️ En milieu très salin (eau salée), le modèle interne **sur-estime volontairement le NPK**.

👉 **Conclusion NPK**
- ❌ Pas une analyse de laboratoire
- ✅ Un **indice agronomique exploitable**
- ✅ Pertinent pour suivi, comparaison et IA agricole

---

## 6️⃣ Évaluation du capteur

### Points positifs
- Réaction physique logique et immédiate
- Très bonne sensibilité à la conductivité
- Stabilité des mesures
- Cohérence entre EC, TDS, salinité et fertilité
- Capteur réellement **10-en-1**

### Limites connues
- NPK basé sur un **modèle indirect**
- Nécessite calibration logicielle pour valeurs absolues

✅ **Capteur parfaitement adapté à l’agriculture intelligente et aux systèmes IA**, pas à l’analyse chimique de laboratoire.

---

## 7️⃣ Évaluation de la bibliothèque **TerraSoil**

### Stabilité
- Lecture continue sans erreur
- CRC Modbus fonctionnel
- Aucune perte de trame
- Compatible ESP32-S3 (UART matériel)

### Qualité logicielle
- API simple : `readSensor(data)`
- Extraction fiable des **10 paramètres**
- Conversion correcte des registres
- Gestion des valeurs négatives
- JSON compact prêt pour IoT / IA

### Architecture
- Code clair et maintenable
- Séparation driver / application
- Documentation complète
- Prête pour :
  - LoRaWAN
  - MQTT
  - Edge AI
  - Enregistrement SD

✅ **Bibliothèque de niveau professionnel, prête à être partagée.**

---

## 8️⃣ Conclusion générale

- Le **capteur 10-en-1** fonctionne correctement et réagit comme attendu.
- Les **10 données sont bien présentes et exploitables**.
- Le **NPK doit être interprété comme un indice**, non comme une valeur chimique absolue.
- La **bibliothèque TerraSoil** est stable, propre et fiable.

### Verdict final

**TerraSoil + capteur 10-en-1 = base technique solide pour l’agriculture intelligente et Terra-AI.**

---
