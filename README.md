# 🚧 Barrière Automatique — ESP32 + L298N

Maquette de barrière automatique pilotée par un microcontrôleur ESP32, réalisée dans le cadre d'un projet de formation en automatisme et systèmes embarqués.

---

## 🎯 Objectifs du projet

- Piloter mécaniquement une barrière (ouverture / fermeture automatique)
- Gérer des fins de course pour les positions extrêmes
- Intégrer des sécurités logicielles et matérielles
- Contrôler le passage des véhicules entrants et sortants
- Mettre en place un système de signalisation visuelle

---

## ✅ Fonctionnalités implémentées

| Fonctionnalité | Description |
|---|---|
| 🔄 Machine d'états | Gestion structurée des phases : OUVERTURE → ATTENTE → FERMETURE → ARRET |
| 🛑 Fins de course | Détection précise des positions ouvert/fermé (INPUT_PULLUP) |
| 💡 Signalisation LED | Rouge = fermé · Jaune = mouvement · Vert = ouvert |
| ⏱️ Anti-rebond | Filtrage logiciel 50 ms sur les fins de course |
| ⏰ Timeout sécurité | Arrêt automatique si blocage moteur détecté (10 s) |
| 🆘 Stop d'urgence | Arrêt immédiat prioritaire via bouton dédié |
| 🔑 Réarmement sécurisé | Reprise uniquement après appui maintenu 5 secondes |
| 🚗 Détection obstacle | Barrière infrarouge IR break beam avec ré-ouverture automatique |

---

## 🧰 Matériel utilisé

| Composant | Référence / Description |
|---|---|
| Microcontrôleur | ESP32 |
| Pont en H | L298N |
| Moteur | Motoréducteur 12 V — 100:1 — 60 rpm |
| Fins de course | × 2 (ouvert / fermé) |
| LED | Rouge, Jaune, Vert |
| Bouton STOP urgence | Bouton mécanique NO câblé vers GND |
| Bouton START manuel | Digital Push Button V3 |
| Détecteur obstacle | IR Break Beam (émetteur + récepteur) |
| Alimentation | 12 V externe |

---

## 📌 Brochage ESP32

| Broche | Rôle |
|---|---|
| GPIO 25 | ENB — Enable moteur (L298N pont B) |
| GPIO 26 | IN3 — Sens moteur |
| GPIO 27 | IN4 — Sens moteur |
| GPIO 13 | Fin de course OUVERT |
| GPIO 5 | Fin de course FERMÉ |
| GPIO 18 | Bouton START (ouverture manuelle) |
| GPIO 21 | Bouton STOP urgence |
| GPIO 19 | Capteur IR obstacle |
| GPIO 17 | LED Rouge |
| GPIO 16 | LED Jaune |
| GPIO 4 | LED Vert |

---

## 🔄 Machine d'états

```
BOOT
  │
  ▼
OUVERTURE ──── timeout ────► ERREUR_TIMEOUT
  │
  │ fin de course OUVERT
  ▼
ATTENTE_OUVERT ──── obstacle détecté → reste ouvert
  │
  │ délai écoulé + zone libre
  ▼
FERMETURE ──── obstacle détecté ────► OUVERTURE (ré-ouverture)
  │          ── timeout ────────────► ERREUR_TIMEOUT
  │
  │ fin de course FERMÉ
  ▼
ARRET_FINAL

        ⚠️ STOP d'urgence prioritaire depuis n'importe quel état
                          │
                          ▼
                    STOP_URGENCE
                          │
              appui START maintenu 5 s
                          │
                          ▼
                       OUVERTURE
```

---

## 📁 Structure du dépôt

```
barriere-automatique-esp32/
│
├── README.md
├── docs/
│   └── compte_rendu.md        # Journal de bord détaillé (séances A1 → D4)
├── src/
│   └── barriere_main/
│       └── barriere_main.ino  # Code source final
├── tests/
│   ├── A2_test_pwm_moteur.ino
│   └── A3_test_bouton_stop.ino
└── hardware/
    └── composants.md          # Liste composants + brochage
```

---

## 🚀 Mise en service

1. Câbler les composants selon le tableau de brochage
2. Alimenter le L298N en **12 V externe**
3. Ouvrir `src/barriere_main/barriere_main.ino` dans l'**IDE Arduino**
4. Sélectionner la carte : **ESP32 Dev Module**
5. Flasher le programme
6. Ouvrir le **Moniteur Série** à **115 200 baud** pour suivre les logs

---

## 📅 Journal de développement

| Date | Étape | Description |
|---|---|---|
| 05/01/2025 | A1 | Étude datasheet motoréducteur |
| 06/01/2025 | A2 | Test PWM moteur + L298N |
| — | A3 | Bouton stop basique |
| 12/01/2026 | B1 | Intégration alimentation 12V + câblage maquette |
| 12/01/2026 | B2 | Machine d'états ouverture/fermeture |
| 13/01/2026 | B3 | Fins de course INPUT_PULLUP |
| 19/01/2026 | B4 | Signalisation LED |
| 26/01/2026 | C1 | Conception rampe FreeCAD + impression 3D |
| 02/03/2026 | D1 | Anti-rebond logiciel fins de course |
| 02/03/2026 | D2 | Timeout sécurité ouverture/fermeture |
| 03/03/2026 | D3 | Stop d'urgence logiciel |
| 03/03/2026 | D4 | Réarmement sécurisé + détection obstacle IR |

---

## 👤 Auteur

Projet réalisé dans le cadre d'une formation en automatisme et systèmes embarqués.
