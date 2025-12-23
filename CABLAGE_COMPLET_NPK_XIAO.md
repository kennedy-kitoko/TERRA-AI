# 🔌 CÂBLAGE COMPLET - CAPTEUR NPK + RS485 + XIAO ESP32-S3

## 📋 LISTE DES COMPOSANTS

1. **Capteur NPK Soil Sensor** (SN-300*-TR-*-N01)
   - Câble 4 fils : Marron, Noir, Jaune, Bleu

2. **Module RS485 (MAX485 ou équivalent)**
   - Pins : VCC, GND, A, B, DE, RE, RO (RX), DI (TX)

3. **XIAO ESP32-S3**
   - Microcontrôleur

4. **Batterie/Alimentation 5V**
   - Tension : 5V DC
   - Courant recommandé : ≥500mA

---

## 🔋 SCHÉMA DE CÂBLAGE COMPLET

```
┌─────────────────────────────────────────────────────────────────────┐
│                      ALIMENTATION 5V                                 │
│                    (Batterie / Adaptateur)                           │
│                                                                       │
│                    +5V ──┬──────────────────────┐                   │
│                          │                       │                   │
│                    GND ──┼───────────┬───────────┼─────────┐        │
└──────────────────────────┼───────────┼───────────┼─────────┼────────┘
                           │           │           │         │
                           │           │           │         │
    ┌──────────────────────┼───────────┼───────────┘         │
    │                      │           │                     │
    │  ┌───────────────────▼───────────▼──────────┐          │
    │  │         MODULE RS485 MAX485              │          │
    │  │                                           │          │
    │  │  VCC ────────────── +5V (du bus)         │          │
    │  │  GND ────────────── GND (masse commune)  │          │
    │  │                                           │          │
    │  │  A ──────────┐                            │          │
    │  │              │  (Twisted Pair / Paire    │          │
    │  │  B ──────────┤   torsadée recommandée)   │          │
    │  │              │                            │          │
    │  │  DE ─────────┼──┐                         │          │
    │  │  RE ─────────┘  │ (reliés ensemble)      │          │
    │  │                 │                         │          │
    │  │  RO (RX) ───────┼────────────────────────┼──────┐   │
    │  │  DI (TX) ───────┼────────────────────────┼────┐ │   │
    │  │                 │                         │    │ │   │
    │  └─────────────────┼─────────────────────────┘    │ │   │
    │                    │                              │ │   │
    │  ┌─────────────────▼──────────────────────┐       │ │   │
    │  │      XIAO ESP32-S3 (VUE DE DESSUS)    │       │ │   │
    │  │                                         │       │ │   │
    │  │   5V  ──────────────── +5V (du bus)    │       │ │   │
    │  │   GND ──────────────── GND (masse)     │       │ │   │
    │  │                                         │       │ │   │
    │  │   GPIO1  (D1) ◄────────────────────────┼───────┘ │   │
    │  │   GPIO43 (D6) ◄────────────────────────┼─────────┘   │
    │  │   GPIO44 (D7) ◄────────────────────────┼─────────────┘
    │  │                                         │
    │  └─────────────────────────────────────────┘
    │
    │  ┌──────────────────────────────────────────┐
    │  │    CAPTEUR NPK SOIL SENSOR               │
    │  │    (SN-300*-TR-*-N01)                    │
    │  │                                           │
    │  │  🟤 MARRON ─── VCC (4.5-30V) ────────────┼──► +5V
    │  │  ⚫ NOIR ───── GND ────────────────────────┼──► GND
    │  │  🟡 JAUNE ──── 485-A ─────────────────────┼──► A (RS485)
    │  │  🔵 BLEU ───── 485-B ─────────────────────┼──► B (RS485)
    │  │                                           │
    │  └───────────────────────────────────────────┘
    │
    └─► MASSE UNIQUE COMMUNE À TOUS LES COMPOSANTS
```

---

## 📊 TABLE DE CONNEXION DÉTAILLÉE

### 🔴 **ALIMENTATION 5V → DISTRIBUTION**

| Batterie 5V | Destination        | Câble          |
|-------------|--------------------|----------------|
| **+5V**     | Module RS485 VCC   | Rouge/Rouge    |
| **+5V**     | XIAO ESP32-S3 (5V) | Rouge/Rouge    |
| **+5V**     | Capteur NPK (Marron)| Rouge/Marron  |
| **GND**     | Module RS485 GND   | Noir/Noir      |
| **GND**     | XIAO ESP32-S3 GND  | Noir/Noir      |
| **GND**     | Capteur NPK (Noir) | Noir/Noir      |

⚠️ **MASSE UNIQUE** : Tous les GND doivent être reliés ensemble !

---

### 🟡 **CAPTEUR NPK → MODULE RS485**

| Capteur NPK        | Couleur | → | Module RS485 |
|--------------------|---------|---|--------------|
| Alimentation +     | 🟤 Marron | → | **VCC** (+5V via bus)|
| Masse              | ⚫ Noir   | → | **GND** (masse commune)|
| Signal 485-A       | 🟡 Jaune  | → | **A** (ligne A)|
| Signal 485-B       | 🔵 Bleu   | → | **B** (ligne B)|

---

### 🔵 **MODULE RS485 → XIAO ESP32-S3**

| Module RS485 | → | XIAO ESP32-S3 | Fonction |
|--------------|---|---------------|----------|
| **VCC**      | → | **5V**        | Alimentation (+5V du bus)|
| **GND**      | → | **GND**       | Masse commune|
| **RO** (RX)  | → | **GPIO44** (D7)| Réception données|
| **DI** (TX)  | → | **GPIO43** (D6)| Transmission données|
| **DE + RE**  | → | **GPIO1** (D1) | Contrôle direction RS485|

⚠️ **IMPORTANT** : Relier **DE** et **RE** ensemble puis connecter à GPIO1

---

## 🔧 DÉTAIL DES CONNEXIONS MODULE RS485

```
Module RS485 (MAX485 / similaire)
┌─────────────────────────┐
│                         │
│  [VCC] ──► +5V          │
│  [GND] ──► Masse        │
│                         │
│  [A]   ──► Ligne A      │ ◄──── Jaune (Capteur)
│  [B]   ──► Ligne B      │ ◄──── Bleu (Capteur)
│                         │
│  [DE]  ──┐              │
│          ├─► GPIO1      │ (reliés ensemble)
│  [RE]  ──┘              │
│                         │
│  [RO]  ──► GPIO44 (RX)  │
│  [DI]  ──► GPIO43 (TX)  │
│                         │
└─────────────────────────┘
```

---

## ⚡ ALIMENTATION ET CONSOMMATION

### **Consommation estimée :**

| Composant          | Consommation    |
|--------------------|-----------------|
| Capteur NPK        | ~20mA (actif)   |
| Module RS485       | ~5-10mA         |
| XIAO ESP32-S3      | ~80-150mA       |
| **TOTAL**          | **~100-180mA**  |

### **Batterie recommandée :**
- **Tension** : 5V DC stable
- **Capacité min** : 500mA
- **Type suggéré** : 
  - Power bank USB (5V 1A minimum)
  - Batterie Li-Po + régulateur 5V
  - 4x piles AA (6V) + régulateur DC-DC 5V

---

## 🛠️ PROCÉDURE DE CÂBLAGE ÉTAPE PAR ÉTAPE

### **ÉTAPE 1 : Préparer l'alimentation**
1. ✅ Connecter le +5V de la batterie à un rail positif
2. ✅ Connecter le GND de la batterie à un rail négatif (masse commune)
3. ✅ **Vérifier la tension** avec un multimètre : doit être ~5V

### **ÉTAPE 2 : Connecter le XIAO ESP32-S3**
1. ✅ **5V pin** du XIAO → Rail +5V
2. ✅ **GND pin** du XIAO → Rail GND (masse)
3. ⚠️ **NE PAS BRANCHER L'USB** en même temps que le 5V externe !

### **ÉTAPE 3 : Connecter le module RS485**
1. ✅ **VCC** du RS485 → Rail +5V
2. ✅ **GND** du RS485 → Rail GND
3. ✅ **RO (RX)** → XIAO GPIO44 (D7)
4. ✅ **DI (TX)** → XIAO GPIO43 (D6)
5. ✅ **DE et RE** reliés ensemble → XIAO GPIO1 (D1)

### **ÉTAPE 4 : Connecter le capteur NPK**
1. ✅ Fil **🟤 Marron** → Rail +5V
2. ✅ Fil **⚫ Noir** → Rail GND
3. ✅ Fil **🟡 Jaune** → RS485 pin **A**
4. ✅ Fil **🔵 Bleu** → RS485 pin **B**

### **ÉTAPE 5 : Vérifications finales**
1. ✅ Tous les GND sont reliés (masse unique)
2. ✅ Aucun court-circuit entre +5V et GND
3. ✅ Les connexions RS485 A/B ne sont pas inversées
4. ✅ DE et RE du RS485 sont bien reliés ensemble

---

## 🔍 SCHÉMA SIMPLIFIÉ (Vue d'ensemble)

```
    [BATTERIE 5V]
         │
         ├───► +5V ──┬──► XIAO ESP32-S3 (5V)
         │           ├──► Module RS485 (VCC)
         │           └──► Capteur NPK (Marron)
         │
         └───► GND ──┬──► XIAO ESP32-S3 (GND)
                     ├──► Module RS485 (GND)
                     └──► Capteur NPK (Noir)

    [CAPTEUR NPK]
         │
         ├───► Jaune (A) ──┐
         │                 │
         └───► Bleu (B) ───┼──► [Module RS485]
                           │
    [XIAO ESP32-S3]        │
         │                 │
         ├───► GPIO44 ◄────┼─── RO (RX)
         ├───► GPIO43 ──────►─── DI (TX)
         └───► GPIO1  ──────►─── DE+RE
```

---

## ⚠️ POINTS IMPORTANTS DE SÉCURITÉ

1. **🔴 MASSE UNIQUE OBLIGATOIRE**
   - Tous les GND doivent être connectés ensemble
   - Une masse mal connectée = communication impossible

2. **🟡 Polarité RS485**
   - A (Jaune) → A
   - B (Bleu) → B
   - **NE PAS INVERSER** sinon pas de communication

3. **🔵 Contrôle DE/RE**
   - DE et RE **DOIVENT** être reliés ensemble
   - Connectés au GPIO1 du XIAO

4. **⚡ Alimentation**
   - Tension stable 5V ±5%
   - Ne pas brancher USB + 5V externe simultanément

5. **🛡️ Protection**
   - Utiliser des câbles torsadés pour RS485 si > 1m
   - Ajouter une résistance de terminaison 120Ω entre A et B si problèmes

---

## 📸 CHECKLIST AVANT MISE SOUS TENSION

- [ ] Tous les GND sont connectés ensemble (masse unique)
- [ ] +5V distribué à tous les composants
- [ ] RS485 : A vers A, B vers B
- [ ] RS485 : DE et RE reliés ensemble vers GPIO1
- [ ] GPIO43 (D6) connecté à DI (TX) du RS485
- [ ] GPIO44 (D7) connecté à RO (RX) du RS485
- [ ] Aucun court-circuit visible
- [ ] Batterie/alimentation fournit bien 5V
- [ ] USB DÉCONNECTÉ du XIAO si alimentation externe

---

## 🎯 TEST APRÈS CÂBLAGE

1. ✅ Mesurer la tension entre +5V et GND : doit être ~5V
2. ✅ Téléverser le code via USB (avant de connecter l'alimentation externe)
3. ✅ Débrancher l'USB
4. ✅ Connecter l'alimentation 5V externe
5. ✅ Ouvrir le Serial Monitor (si USB reconnecté en lecture seule)
6. ✅ Vérifier la sortie JSON toutes les 5 secondes

---

## 🆘 DÉPANNAGE

| Problème | Cause possible | Solution |
|----------|----------------|----------|
| Pas de communication | A/B inversés | Inverser Jaune et Bleu |
| Données corrompues | Masse mal connectée | Vérifier tous les GND |
| Pas de réponse | Mauvaise adresse Modbus | Vérifier adresse = 0x01 |
| Timeout | DE/RE mal connecté | Relier DE et RE ensemble |
| XIAO ne démarre pas | Tension < 4.5V | Vérifier alimentation |

---

**✅ Câblage terminé ! Vous êtes prêt à lire les données NPK ! 🌱**
