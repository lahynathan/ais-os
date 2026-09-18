# Vidéo dans les leçons — ce qui marche et ce qui casse en silence

Test direct du 18 septembre 2026, 23:50 UTC. Compte en **plan gratuit**.

## Réponse courte

**Oui, on peut intégrer une vidéo dans une leçon par un lien externe.** YouTube, Vimeo et Loom sont reconnus et fonctionnent, y compris en plan gratuit.

**Non, on ne peut pas héberger une vidéo sur la plateforme** par le canal automatisé : la vidéothèque native est réservée au plan Pro, et aucune action de téléversement n'existe de toute façon.

## Les deux blocs, et ce qui les sépare

| Bloc | Usage | Plan gratuit |
|---|---|---|
| `videoEmbedBlock` avec `data-url` | Lien vers une vidéo hébergée ailleurs | **Accepté** |
| `videoBlock` avec `videoId` | Vidéo de la bibliothèque du créateur | **Refusé — `402 PRO_PLAN_REQUIRED`** |

Message exact du refus : `These blocks require a Pro plan: videoBlock.`

C'est le **cinquième quota commercial** identifié, après le bloc de code, l'envoi d'emails, les produits et les liens courts.

## Ce que la plateforme fait réellement du lien

Elle ne se contente pas de stocker l'URL : **elle l'analyse et en extrait un identifiant de vidéo et un type de fournisseur.** Relecture du contenu stocké après création :

| URL envoyée | `data-type` extrait | `videoId` extrait | Vignette |
|---|---|---|---|
| `youtube.com/watch?v=...` | `youtube` | oui | **oui**, depuis `img.youtube.com` |
| `youtu.be/...` (format court) | `youtube` | oui | **oui** |
| `vimeo.com/76979871` | `vimeo` | oui | non |
| `loom.com/share/...` | `loom` | oui | non |
| `dailymotion.com/video/...` | **aucun** | **aucun** | non |
| `.../demo.mp4` (fichier direct) | **aucun** | **aucun** | non |

Les deux formats YouTube, long et court, sont reconnus indifféremment.

## Le piège, et il est sérieux

**Un fournisseur non reconnu ne produit aucune erreur.** L'appel réussit, la leçon est créée, la réponse est un succès ordinaire.

Mais le bloc stocké est vide de toute substance : ni type, ni identifiant, ni vignette. Il ne reste qu'une URL dans un attribut, sans lecteur pour l'exploiter.

**Conséquence pour un agent.** Insérer un lien Dailymotion, Twitch, un fichier `.mp4` ou n'importe quel autre hébergeur produit une leçon qui semble correcte côté API et qui ne montrera rien à l'élève. Rien ne le signale, ni à la création, ni à la relecture superficielle.

**La vérification obligatoire** : après insertion, relire la leçon par `get_lesson` et contrôler que le bloc porte bien un attribut `data-type`. Son absence est le seul indice de l'échec.

## Un point de conformité à signaler

Seul YouTube reçoit une vignette automatique, et elle est servie depuis `img.youtube.com`. **Le navigateur de l'élève émet donc une requête vers Google dès l'affichage de la leçon**, avant toute lecture et sans interaction.

Cela croise directement le constat A06 sur le consentement préalable aux traceurs : un créateur qui intègre une vidéo YouTube dans son espace membre déclenche une connexion à un tiers, sans qu'aucun mécanisme de consentement ne soit fourni par la plateforme. À verser au volet cookies du dossier de conformité.

## Ce qui reste non établi

- Le **rendu réel** du bloc pour l'élève : l'egress de cet environnement ne permet pas d'ouvrir la page.
- Le comportement de `videoBlock` **sur un compte Pro**, et la manière dont une vidéo entre dans la bibliothèque, aucune action de téléversement n'existant au catalogue.
- Si la plateforme utilise le mode **« nocookie »** de YouTube pour le lecteur. La vignette, elle, vient du domaine standard.

## Objets créés par ce test

| Leçon | Identifiant | État |
|---|---|---|
| 10. POC VIDEO lien YouTube | `44c6ca09-b483-4107-8741-97515e31424f` | brouillon |
| 11. POC VIDEO fournisseurs varies | `41fd72a7-79d3-4288-ac50-f9daff164c98` | brouillon |

La leçon 12, qui testait la vidéothèque native, n'a pas été créée : refusée par le quota de plan.

## Deux anomalies d'état relevées au passage

**Le produit avait changé de type.** Les trois premières tentatives ont échoué sur `Lessons can only be added to member space products` : un agent de la campagne précédente avait laissé le produit en type `download` au lieu de `member`. Remis en `member` avant les tests.

C'est une illustration du risque déjà consigné : **un compte partagé entre plusieurs agents ne garantit aucune stabilité d'état.** Relire avant d'agir n'est pas une précaution, c'est une nécessité.

**Le produit payant « Test » n'existe plus.** `list_product_members` sur son identifiant renvoie `404 Product not found`. Aucune action de suppression de produit n'existant au catalogue, la suppression vient de l'interface. Conséquence utile : **l'accès payant octroyé au contact fictif, signalé au nettoyage, est devenu sans objet.**
