# Couverture des menus — synthèse

18 septembre 2026. Compte `nathan-lahy.tinypages.co`, **plan gratuit**. Les neuf entrées du menu de l'application ont été éprouvées, plus les volets qui n'apparaissent pas dans la navigation mais existent au catalogue.

## Tableau de couverture

| Menu | Actions éprouvées | État | Document |
|---|---|---|---|
| Accueil / compte | `get_account`, `list_accounts`, `get_business_context`, `search_docs` | Couvert | `menu_membres_modeles_compte.md` |
| Pages web | création, lecture, mise à jour, publication, dépublication, thèmes, SEO | Couvert | `passe1`, `passe4` |
| Produits | création, mise à jour, conversion de type, quotas, statistiques | Couvert | `passe1`, `menu_ventes_stats.md` |
| Emails | broadcast, emails d'automatisation, statistiques, destinataires | Couvert sauf envoi réel | `passe3`, `menu_ventes_stats.md` |
| Automatisations | étiquette, nouveau contact, résolution d'identifiant | Couvert | `passe3_acquisition.md` |
| Formulaires | création multi-pages, destinations, `nodeId` | Couvert sauf soumission réelle | `passe3_acquisition.md` |
| Blog | création, contrôle de version | Couvert | `passe2_cours.md` |
| Contacts | création, étiquetage, retrait, listage | Couvert | `passe3`, `passe4` |
| **Ventes** | ventes, résumé, statistiques produit et email, destinataires | Couvert sur compte vide | `menu_ventes_stats.md` |
| Espace membre | leçons, modules, membres, accès | Couvert | `passe2`, `menu_membres_modeles_compte.md` |
| Médias | images, vidéos | Couvert | `menu_medias_liens.md` |
| Liens | liens courts | **Bloqué en plan gratuit** | `menu_medias_liens.md` |
| Monétisation | upsells, order bumps, deadlines evergreen, coupons | Couvert | `menu_monetisation.md` |
| Modèles | lecture, création, mise à jour | Couvert | `menu_membres_modeles_compte.md` |

## Les deux constats les plus lourds

### C-01 — Une IA connectée peut offrir un produit payant, et ne peut pas reprendre son geste

`add_member_to_product` **accorde l'accès à un espace membre sans aucune transaction**. L'appel a réussi à l'identique sur le produit gratuit et sur le produit payant à 100 €. Le serveur ne regarde ni le prix, ni le mode de paiement. Vérifié par relecture indépendante : `isAllowed: true`.

**Et aucune action de retrait n'existe au catalogue.** Une recherche explicite de révocation ne remonte rien. La fonction existe pourtant côté interface humaine, documentée sous le nom « Restrict access ».

Le schéma diverge en outre de l'attendu : l'action exige un `email`, pas un `contactId`.

**Conséquence.** C'est un contournement de monétisation à sens unique : exécutable en un appel, sans confirmation, et non réversible par le même canal. Sur un compte client réel, une IA qui se tromperait de produit offrirait un accès payant sans moyen de le reprendre autrement qu'à la main.

**Action requise sur le compte de test** : l'accès accordé au contact fictif sur le produit « Test » doit être retiré manuellement. Tableau de bord → Produits → Test → membres → ligne `poc-test-01@example.invalid` → *Restrict access*.

### C-02 — Le quota de produits payants ne s'applique qu'à la création

Le plan gratuit n'autorise qu'un produit payant, et le compte en avait déjà un. `create_product` refuse en `402`. Mais **`update_product` a porté le produit gratuit à 15 € sans le moindre contrôle**.

Le quota est donc vérifié à la création et pas à la modification. Un agent bloqué par le quota le contourne en une ligne. Prix restauré à 0 € après le test.

## Les autres constats du jour

| # | Constat | Portée |
|---|---|---|
| C-03 | Le quota de 10 pages web **inclut les 5 pages système** créées d'office. Le compte était à 11 sur 10, donc déjà en dépassement avant tout test. | Quota |
| C-04 | `create_link` est **réservé au plan Pro**. Quatrième quota commercial identifié. | Quota |
| C-05 | **Aucun téléversement n'existe** au catalogue : ni image, ni vidéo, ni fichier. Les blocs image exigent une URL de la bibliothèque, qui ne peut être alimentée que par génération IA. | Limite structurante |
| C-06 | Les images générées par `<ai-img>` **rejoignent la bibliothèque du créateur** et y restent : aucune action ne les supprime. Délai d'indexation d'environ 10 à 15 secondes, piège pour qui vérifie trop tôt. | Usage |
| C-07 | Un produit à 0 € ne peut **jamais** servir d'addon à un upsell ou un order bump : la contrainte « remise inférieure au prix » est insatisfiable à prix nul. | Règle produit |
| C-08 | `delete_upsell` et `delete_order_bump` répondent **en texte brut**, pas en JSON. Un agent qui teste un champ de succès JSON conclut à tort à l'échec. Suppression immédiate, non réversible, **non idempotente** — un second appel renvoie 404, ce qui est plus sûr que le faux succès de `remove_tag_from_contact`. | Usage |
| C-09 | Le contrôle de version s'étend à `update_upsell_content` et `update_template`. Restent sans aucun contrôle : `update_product`, `update_form`, `update_coupon`, `update_evergreen_deadline`. | Concurrence |
| C-10 | `allowedPageTypes` d'un modèle n'est **jamais un paramètre d'entrée** : il est recalculé d'après les blocs présents. Ajouter une bannière a réduit la compatibilité de 5 types à 3. | Usage |
| C-11 | Les statistiques d'un email **jamais envoyé sont refusées**, avec un message **identique** à celui d'un identifiant inexistant. Impossible de distinguer les deux cas. | Observabilité |
| C-12 | `get_product_stats` renvoie `null` pour certains champs et `0` pour d'autres. Un test `=== 0` se trompe. | Usage |
| C-13 | Une période **inversée ou entièrement future** est acceptée en silence et renvoie des zéros. | Observabilité |
| C-14 | Deux familles de formats de date incompatibles selon l'action : date simple, ou ISO 8601 complet avec `Z`. | Usage |
| C-15 | Le MCP renvoie des statistiques de produit **que l'interface ne montre pas** pour un produit gratuit. L'IA voit plus que l'humain. | Écart interface / API |
| C-16 | `aiSystemPrompts` est **lisible mais non écrivable** par le catalogue. Aucune action ne le modifie. | Limite |
| C-17 | Une date limite evergreen est **limitée à une par étiquette**. Un même produit ne peut être à la fois upsell et order bump du même parent. | Règle produit |
| C-18 | Le paramètre `order` d'un upsell déclare des bornes énormes au schéma mais le serveur n'accepte que 1 ou 2. | Écart schéma |
| C-19 | Un `code` de coupon explicite **est accepté** s'il respecte le motif, et il est unique au compte : un doublon renvoie `409`. Cela tranche une divergence laissée ouverte par le manuel. | Correction |
| C-20 | **Aucun export** des ventes ni des statistiques n'existe. Seules les soumissions de formulaire s'exportent, depuis l'interface. | Limite |

## Volet données personnelles

Trois actions exposent à toute IA connectée des données sensibles, **sans aucune granularité de permission** plus fine que l'accès au compte entier :

- `get_account` expose l'**adresse email personnelle du titulaire**, sans masquage possible.
- `list_product_members` expose l'identité complète d'un membre : email, nom, téléphone, statut d'abonnement, montant dépensé, étiquettes.
- `get_analytics_sales` exposerait le registre nominatif des transactions, et `list_email_recipients` associerait identité et comportement individuel d'ouverture et de clic. Non vérifié faute de vente et d'envoi réels, mais déclaré au schéma.

## Ce que cette campagne n'a pas établi

Le comportement sur un **compte Pro**. L'**isolation entre comptes**. Le **déclenchement réel** d'une destination de formulaire. Le comportement des statistiques **après une vente et un envoi réels** : plusieurs schémas restent déclaratifs.

## Nettoyage manuel restant

Aucun de ces objets ne peut être supprimé par le canal automatisé.

1. **Retirer l'accès du contact fictif au produit payant « Test »** — c'est le plus important.
2. Tous les objets préfixés `POC` : produit, leçons, pages, formulaire, blog, coupons, étiquettes, deadline, modèle, contact.
3. L'image `ai-image-poc-media-test-assiette-ifar7r` dans la bibliothèque.
4. Le champ de contexte métier, qui porte `-` au lieu d'être vide.
5. Les trois traces de l'audit technique antérieur.
