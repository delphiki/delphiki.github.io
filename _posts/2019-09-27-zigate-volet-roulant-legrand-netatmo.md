---
layout: post
title: "Interrupteur pour volet roulant Legrand Netatmo sous Home Assistant avec la Zigate"
date: 2019-09-27
---

Je n'ai pas vu beaucoup d'information sur le processus complet pour intégrer un <strong>interrupteur de volet roulant Legrand Netatmo</strong> sous <strong>Home Assistant</strong>.

Je pars du principe que vous avez déjà installé votre (Pi)Zigate sous Home Assistant.
Personnellement, j'utilise le [composant de doudz](https://github.com/doudz/homeassistant-zigate) pour intégrer la PiZigate sur un Raspberry Pi 3 B+ avec Hassio.

## Démonter les caches pour avoir accès au bouton reset

La première étape consiste à avoir accès au bouton reset de l'interrupteur. Celui-ci est dissumulé sous le cache plastique du bouton en lui-même.

Pour ce faire, j'ai utilisé un petit tournevis pour déclipser le cache, comme ceci :

{% include youtube_embed.html id="V5dQ91XUmTM" %}  

Une fois que c'est fait, on peut observer un petit trou à côté de la led rouge, c'est notre bouton reset.

## Intégration dans Home Assistant

L'association à la Zigate se fait dans Home Assistant grâce au service `zigate.permit_join` (toujours en utilisant le [composant de doudz](https://github.com/doudz/homeassistant-zigate). Celui-ci met la Zigate en mode association (Permit Join) pour 30 secondes.

Vous pouvez vous créer un raccourci grâce au _custom panel_ comme indiqué dans la doc du composant, ou en ajoutant un simple switch dans lovelace (comme je l'ai fait. Dans le second cas, l'entité s'appelle `input_boolean.zigate_permit_join`.

Les étapes pour l'association sont les suivantes :

1. Mettre la Zigate en mode `Permit Join`
2. Rester appuyé sur le bouton reset `jusqu'à ce que la led clignote` (~ 8 secondes), attendre un peu, puis `rappuyer une fois` sur le bouton reset.
3. La led devient `verte`, puis la nouvelle entité apparait dans Home Assistant.

Ces étapes sont illustées dans la vidéo suivante :

{% include youtube_embed.html id="7GZPuC3sWik" %}  

Vous aurez ensuite des entités `cover` que vous pourrez utiliser très facilement, elles porteront un nom en `cover.zigate_XXXXXXXXXX`.

Ces entités fournissent notamment les informations sur l'ouverture, la fermeture, ou encore le taux d'ouverture du volet :

![détails cover home assistant](/uploads/details_cover_ha.png "Détails des informations sur les volets dans Home Assistant")

