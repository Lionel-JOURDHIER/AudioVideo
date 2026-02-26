# 🎥 🎤 🎥 🎤 🎥 🎤 🎥 🎤 🎥 🎤 🎥 🎤
# IA en Temps Réel — Vision & Audio

---
### Introduction

L'objectif de cette journée est de sortir du cadre des datasets de test pour confronter vos modèles au monde réel. Vous allez intégrer vos réseaux de neurones dans des boucles de traitement continu (flux vidéo et flux audio) en utilisant **OpenCV** et **PyAudio/Librosa**.

---

### Ce que vous allez apprendre aujourd'hui :

* **La gestion des ressources :** Faire tourner un modèle d'IA à 30 FPS (images par seconde) demande de l'optimisation.
* **Le lissage des prédictions :** Un modèle peut hésiter entre deux frames. Comment éviter que le texte à l'écran ne clignote entre deux classes ? (Indice : Moyenne glissante sur les dernières prédictions).
* **L'UX de l'IA :** Comment rendre une prédiction complexe compréhensible pour un utilisateur final.
  
---

## Projet 1 : Vision — "Chat, Chien ou Autre Chose ?"

L'idée est d'utiliser la webcam pour classifier ce qui se trouve devant l'objectif en temps réel.

### Objectifs :

1. **Intégration du modèle :** Utilisez votre modèle "Cats vs Dogs" entraîné précédemment. Si vous n'avez pas votre modèle sauvegardé, utilisez un modèle pré-entraîné comme **VGG16** ou **MobileNetV2** (plus léger) via `tensorflow.keras.applications`.
2. **Boucle de capture :** Utilisez `cv2.VideoCapture(0)` pour lire le flux de la webcam.
3. **Traitement d'image :** Pour chaque image capturée, vous devez appliquer le même prétraitement que lors de l'entraînement (redimensionnement, normalisation).
4. **Affichage :** Affichez la prédiction (nom de la classe et score de confiance) directement sur la vidéo avec `cv2.putText`.

### Observation critique (La classe nulle)

Votre modèle a été entraîné sur un choix binaire (Chat ou Chien). Que se passe-t-il si vous montrez une tasse ou votre visage à la caméra ?

* **Le problème :** Le modèle "force" une décision et prédira toujours l'une des deux classes avec une certaine probabilité.
* **Défi :** Implémentez un **seuil de confiance** (ex: si , affichez "Inconnu"). Réfléchissez à l'importance d'une "classe nulle" ou d'une classe "Background" dans un projet réel.

---

## Projet 2 : Audio — "L'Oreille Intelligente"

Nous allons maintenant créer une interface capable d'écouter l'environnement et de reconnaître les sons urbains du dataset **UrbanSound8K**.

### Objectifs :

1. **Interface de contrôle :** Créez une interface simple (ou utilisez des touches clavier comme 's' pour Start et 'q' pour Quit) pour lancer l'écoute.
2. **Gestion du Buffer :** Le modèle a été entraîné sur des extraits de 4 secondes. Vous devez créer un **buffer audio** (un tampon) qui accumule les données du micro avant de les envoyer au modèle.
3. **Prétraitement "On-the-fly" :** À chaque intervalle (ex: toutes les 2 secondes), extrayez les caractéristiques (Mel-Spectrogramme ou MFCC) du buffer actuel.
4. **Classification continue :** Affichez à l'écran la classe détectée (ex: "Siren", "Street Music", "Drilling") en mettant à jour le texte dès qu'un nouveau son est identifié.

### Contraintes techniques :

* **Fréquence d'échantillonnage :** Assurez-vous que le flux micro correspond au `sr` utilisé lors de l'entraînement (ex: 16kHz ou 22.050Hz).
* **Glissement (Overlap) :** Pour plus de réactivité, essayez de faire des prédictions glissantes (ex: prédire toutes les secondes sur les 4 dernières secondes écoulées).

---

## Bonus Final : Le Système Sentinel

Si vous terminez en avance, fusionnez les deux projets :

* Le système écoute en arrière-plan.
* **Si et seulement si** un bruit spécifique est détecté (ex: "Dog bark" ou "Siren"), la fenêtre vidéo s'ouvre et enregistre une image de ce qu'il se passe à ce moment-là.

