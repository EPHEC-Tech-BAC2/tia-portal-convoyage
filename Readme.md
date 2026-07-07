# Système automatisé de convoyage et d'usinage — Projet PO3 (TIA Portal / S7-1200)

Projet réalisé en binôme (EL YAZAMI Yassine, ZAGHOULI Wasim) dans le cadre du cours Projet d'automatisation à l'EPHEC (Classe 2AU, 2025-2026), implémenté et mis en service sur automate réel.

## Le système

Une pièce traverse 4 tapis roulants et subit deux opérations d'usinage successives : un fraisage puis un perçage. Deux vérins électriques servent d'aiguillages entre les tapis.

Cycle automatique :
1. La pièce est détectée en entrée (Tapis 1)
2. Arrêt de 3s, le Vérin 1 aiguille la pièce vers le Tapis 2
3. Fraisage devant la fraiseuse (5s)
4. Transfert vers le Tapis 3, perçage devant la foreuse (6s)
5. Le Vérin 2 aiguille la pièce vers le Tapis 4
6. Évacuation, incrémentation du compteur de cycles

Cinq modes de fonctionnement, organisés selon un GEMMA : Initial, Automatique, Manuel, Réinitialisation, Urgence.

## Matériel

- PLC : Siemens S7-1200 (CPU 1215C DC/DC/RLY)
- HMI : Siemens SIMATIC KTP (écran tactile)
- 4 moteurs de convoyeurs, 1 fraiseuse, 1 foreuse, 2 vérins électriques
- 5 capteurs de présence, 4 fins de course, 1 arrêt d'urgence (contact NC)

## Logiciel

Programme structuré autour d'un bloc principal (Main [OB1]) qui appelle chaque grafcet selon l'étape active :

- **GS** — Grafcet de Sécurité, priorité absolue sur l'arrêt d'urgence
- **GC** — Grafcet de Conduite, transitions entre les modes
- **GA** — Grafcet Automatique, cycle de production complet (14 étapes)
- **GM** — Grafcet Manuel, commande individuelle par appui maintenu
- **GRST1 / GRST2** — remise en position des vérins après urgence ou sortie du mode manuel
- **OUTPUT [FC8]** — sorties physiques, affichages HMI, animations
- **Count [FB1]** — compteur de cycles

Deux blocs de données centralisent les variables : `DB_Control` (interface HMI↔PLC) et `DB_Step` (état de chaque étape des grafcets).

## Contenu du dossier

- `projet-tia-portal/` : projet TIA Portal à ouvrir
- `schemas-electriques/` : schéma de câblage (dwg)
- `grafcet/` : GRAFCETs niveaux 1 et 2
- `io-config/` : tableau I/O/M initial + fichier d'import des variables pour PLCSim
- `rapport/` : guide utilisateur complet (fonctionnement, matériel, grafcets, câblage, listing du programme)