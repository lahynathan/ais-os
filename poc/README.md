# POC TinyPages — génération de contenu et manuel d'administration

18 septembre 2026. Compte `nathan-lahy.tinypages.co`, **plan gratuit**. Quatre passes de génération, exécutées pour partie par l'orchestrateur et pour partie par des sous-agents disposant du canal MCP.

## Ce qui a été produit sur la plateforme

| Type | Nombre | État final |
|---|---|---|
| Produit espace membre | 1 | gratuit, commentaires activés |
| Leçons | 10 | **brouillon** (voir l'incident) |
| Modules de cours | 3 | plus le conteneur `standalone` |
| Articles de blog | 2 à 3 | brouillon |
| Pages web | 3 | brouillon |
| Formulaire multi-pages | 1 | brouillon, 3 pages |
| Destination de formulaire | 1 | — |
| Emails d'automatisation | 3 | brouillon, jamais envoyés |
| Étiquette | 1 | — |
| Coupon | 1 | code serveur `XLYK19KN` |
| Contact fictif | 1 | `@example.invalid` |

Aucun envoi d'email. Aucune donnée personnelle réelle.

## Les documents

| Fichier | Contenu |
|---|---|
| `00_plan_generation.md` | Le plan des quatre passes, écrit avant exécution |
| `passe1_orchestrateur.md` | Produit, leçons, modules, page, formulaire, coupon — 8 constats |
| `passe2_cours.md` | Cours long, blog, contrôle de version optimiste éprouvé en 3 scénarios |
| `passe3_acquisition.md` | Formulaires, automatisations, cycle étiquette — 20 appels, aucun échec |
| `passe4_cas_limites.md` | Bornes de schéma, identifiants invalides, quotas, formats |
| `incident_publication_non_sollicitee.md` | Dix leçons publiées sans appel identifié, état rétabli |
| **`MANUEL_ADMINISTRATION.md`** | **Le livrable : le manuel à l'attention des agents administrateurs** |

## Les cinq enseignements qui comptent

1. **Le code d'un coupon n'est pas son nom.** Un lien de paiement bâti sur le nom ne déclenche aucune remise et ne produit aucune erreur. Le visiteur paie plein tarif.
2. **Un brouillon n'est pas un état protégé.** Dix leçons se sont publiées sans qu'aucun agent n'admette l'avoir demandé, et aucun journal ne permet de remonter à l'appel responsable.
3. **Rien ne se supprime par le canal automatisé.** Ni page, ni leçon, ni produit, ni contact. Une convention de préfixe n'est pas une bonne pratique, c'est une obligation.
4. **Les quotas du plan gratuit bloquent avant les tests qu'on veut faire.** Un compte Pro dédié est indispensable à toute campagne UAT sérieuse.
5. **Le schéma annoncé diverge parfois du schéma réel.** Le message d'erreur nomme le champ attendu : c'est lui qu'il faut lire, pas la documentation.

## Ce que le POC n'a pas établi

- Le comportement sur un **compte Pro** : tous les quotas rencontrés sont ceux du plan gratuit.
- L'**isolation entre comptes** : un seul compte était disponible, le test d'accès croisé n'a pas pu être fait.
- Le **déclenchement réel** d'une destination de formulaire : sa configuration a été créée, aucune soumission n'a été faite.
- Le comportement de `schedule_email`, jamais appelé par consigne.

## Nettoyage restant, manuel

Tous les objets créés portent le préfixe `POC`. Ils sont à supprimer dans l'interface, aucun ne pouvant l'être par le canal automatisé. S'y ajoutent les trois traces laissées par l'audit technique antérieur, listées dans `audit/annexes/actions_mcp_executees.md`.
