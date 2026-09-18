# Actions MCP réellement exécutées pendant l'audit

Extrait versable. Établi le 18 septembre 2026. Ce document est la pièce justificative des affirmations portant sur ce que l'audit a réellement exercé du serveur MCP TinyPages.

Il est volontairement expurgé : il ne porte ni le nom du compte, ni son sous-domaine, ni aucune adresse. L'annexe complète, qui contient ces éléments, est strictement interne.

## Décompte qui fait foi

**17 actions distinctes, appelées en 22 appels, sur un catalogue de 104.**

Les 87 autres actions du catalogue n'ont **pas** été appelées. Elles sont connues par la description que le serveur publie de lui-même — source primaire, mais **déclarative** : c'est TinyPages qui décrit TinyPages. Aucune n'a été confrontée à son schéma ni à son comportement réel.

La formulation à employer partout, et nulle part une autre : **« inventaire relevé par lecture de la description publiée par le serveur, non exécuté action par action »**.

## Les 17 actions, et la réponse obtenue

| # | Action | Famille | Résultat |
|---|---|---|---|
| 1 | `get_account` | lecture | Succès |
| 2 | `list_accounts` | lecture | Succès — un compte, aucun sous-compte |
| 3 | `list_contacts` | lecture | Succès |
| 4 | `list_webpages` | lecture | Succès — 7 pages |
| 5 | `list_products` | lecture | Succès — un produit |
| 6 | `list_emails` | lecture | Succès — un brouillon |
| 7 | `get_webpage` | lecture | Succès — trois appels |
| 8 | `get_business_context` | lecture | Succès — champ vide à l'origine |
| 9 | `get_analytics_summary` | lecture | Succès |
| 10 | `list_forms` | lecture | Succès — aucun formulaire |
| 11 | `list_templates` | lecture | Succès — 15 modèles |
| 12 | `create_webpage` avec bloc de code | écriture | **Refus `402 PRO_PLAN_REQUIRED`** |
| 13 | `create_webpage` sans bloc de code | écriture | Succès — brouillon créé |
| 14 | `publish_webpage` | publication | **Succès — page mise en ligne, sans confirmation ni contrôle** |
| 15 | `unpublish_webpage` | publication | Succès |
| 16 | `create_email` | écriture | Succès — brouillon créé |
| 17 | `send_email` | envoi | **Refus `402 PRO_PLAN_REQUIRED`** |
| 18 | `update_business_context` | écriture | Trois appels : **succès en écriture sans aucun contrôle**, **refus** sur la remise à vide, succès sur la réduction à un caractère |

Le tableau compte 18 lignes pour 17 actions distinctes, `create_webpage` y figurant deux fois. Le nombre total d'appels est de 22, `get_webpage` et `update_business_context` ayant été appelés trois fois chacun.

## Ce que ces 22 appels établissent, et ce qu'ils n'établissent pas

**Établi :** la publication d'une page publique ne passe par aucun contrôle serveur ; les deux seuls refus serveur observés sont des refus commerciaux liés au plan ; le champ de contexte de génération persistant s'écrit sans contrôle et ne peut pas être remis à vide par ce canal ; la plateforme publie sur chaque compte des pages légales vides et indexées à côté d'un formulaire de collecte d'emails sans double opt-in.

**Non établi :** le comportement des 87 autres actions ; le comportement d'un compte sur un plan payant ; l'autorisation objet par objet entre locataires, faute d'un second compte ; tout ce qui relève de l'interface.

## Réserve de méthode

Ces appels ont été exécutés sur un compte de production réel, sous une dérogation accordée oralement et consignée par écrit a posteriori. **Le rejeu horodaté de ce protocole sur un locataire de test dédié reste à produire**, et c'est lui qui deviendra la pièce de référence. Jusque-là, la recevabilité de ces relevés est attaquable, quand bien même leur contenu ne l'est pas.

Trois traces subsistent sur le compte utilisé et **aucune n'est effaçable par le canal automatisé** : deux brouillons et le champ de contexte métier, qui porte un caractère au lieu d'être vide.
