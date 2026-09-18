# POC — Passe 2 « Cours structuré » — journal d'exécution (suite)

Rédigé le 18 septembre 2026, dans la continuité du travail déjà réalisé par l'orchestrateur sur le produit espace membre `POC Cadrage Net` (id `85dd81bd-14f5-4850-92a4-ff5ceab3c0c2`, gratuit, commentaires activés). Les quatre premières leçons et les deux premiers modules (« Module 1 — Regarder », « Module 2 — Construire ») existaient déjà en entrant dans cette session ; ce document ne couvre que les actions exécutées ici : deux leçons supplémentaires sur la composition photographique, une leçon volontairement très longue pour tester une limite de contenu, la réorganisation en trois modules, deux articles de blog, et une épreuve ciblée du contrôle de version optimiste de `update_lesson`.

**Aucune publication n'a été déclenchée.** `publish_lesson`, `publish_all_lessons` et `publish_blog_post` n'ont jamais été appelés. Tous les objets créés ou modifiés ci-dessous restent à l'état `draft`.

---

## 1. Constat préalable : les titres de leçons existants divergent légèrement du contexte transmis

Avant toute écriture, un `get_lesson` sur la leçon 1 puis un `get_modules` ont montré que les titres réels en base diffèrent de ceux indiqués dans le message de mission :

| Titre annoncé dans la mission | Titre réel constaté en base |
|---|---|
| « 2. Où poser le sujet » | « 2. Où poser le sujet dans le cadre » |
| « 3. Les lignes » | « 3. Les lignes conduisent le regard » |
| « 4. sans contenu » | « 4. Exercice pratique à compléter » (et la leçon 4 contient en réalité un titre, pas nécessairement du contenu — non vérifié plus avant, hors périmètre de cette passe) |

**Ce qu'un administrateur doit en retenir :** ne jamais recopier de mémoire le nom d'un objet déjà créé dans un tour précédent — toujours le relire avant d'agir dessus, exactement comme le prescrit la règle « lecture avant écriture » du plan de génération. Un résumé transmis d'un agent à l'autre (ou d'un tour à l'autre) peut légèrement s'écarter de l'état réel.

---

## 2. Tableau détaillé des appels

| # | Action | Voie | Résolution préalable | Résultat au 1er essai | Message d'erreur exact | Limite de plan | Champ « optionnel » en réalité obligatoire | Piège de format | Objet créé / modifié (ID) |
|---|---|---|---|---|---|---|---|---|---|
| 1 | `get_lesson` (leçon 1) | Outil direct | — | Succès | — | Non | — | — | Lecture de `2896330e-1c52-43e6-8ddf-c28ca70d98b4`, `updatedAt` = `2026-09-18T23:08:40.693Z` |
| 2 | `get_modules` (via catalogue) | `search_actions` + `execute_action` | — | Succès | — | Non | — | — | Lecture : 2 modules nommés + `standalone` vide |
| 3 | `create_lesson` — « 5. La lumière comme matière » (~780 mots) | Outil direct | Aucune (productId déjà connu) | Succès brut | — | Non | — | — | `7367bbb3-993d-4bf3-aa30-06e59578ac8f` |
| 4 | `create_lesson` — « 6. Trier ses images après la prise de vue » (~830 mots) | Outil direct | Aucune | Succès brut | — | Non | — | — | `0c81a2b5-cc71-4e4a-8813-4710c44dbcfc` |
| 5 | `create_lesson` — « 7. Manuel complet des outils de composition » (contenu volontairement > 1500 mots, ~1900 mots / environ 12 500 caractères HTML) | Outil direct | Aucune | **Succès brut, sans aucun avertissement ni troncature** | — | Non | — | Aucune borne de longueur rencontrée — voir §3 | `bd57784e-3823-4991-a1b2-04eb789a0f1f` |
| 6 | `get_lesson` (leçon 7, relecture de contrôle) | Outil direct | — | Succès | — | Non | — | — | Confirme le contenu intégral stocké, caractère pour caractère, aucune coupe |
| 7 | `get_modules` (relecture avant réorganisation, car 3 `create_lesson` ont eu lieu depuis la lecture #2) | Catalogue | Obligatoire d'après la description même de `update_modules` (« race condition ») | Succès | — | Non | — | — | Confirme : les 3 nouvelles leçons atterrissent automatiquement dans `standalone` |
| 8 | `update_modules` — 3 modules nommés + `standalone` | Catalogue | `get_modules` (#7) obligatoire pour les IDs de leçons | **Succès dès le premier essai** | — | Non | — | Voir §4 (module `standalone` obligatoire même non vide dans ce cas, et ID de module libre, pas forcément un UUID) | Modules : `module-1-regarder`, `module-2-construire`, `module-3-lumiere-et-tri` (créé), `standalone` |
| 9 | `create_blog_post` — « POC — 5 habitudes de composition à corriger cette semaine » (~640 mots) | Outil direct | Aucune | Succès brut | — | Non rencontrée | — | — | `550d174d-ba30-45d9-bf03-e208e5ebfad7` |
| 10 | `create_blog_post` — « POC — Le tri photo, l'étape que tout le monde bâcle » (~620 mots) | Outil direct | Aucune | Succès brut | — | Non rencontrée | — | — | `1efd04ca-86c9-4f0b-9b88-a070515d0ee9` |
| 11 | `update_lesson` (leçon 1) avec `version` = la valeur exacte lue en #1 (courante à ce moment) | Outil direct | `get_lesson` (#1) | **Succès** | — | Non | `version` **est bien obligatoire** (confirmé en #13) | — | Leçon `2896330e...` mise à jour, nouvelle `updatedAt` = `2026-09-18T23:15:46.105Z` retournée directement dans le texte de la réponse |
| 12 | `update_lesson` (même leçon) avec la **même `version` qu'en #11, désormais périmée** puisque #11 a déjà fait avancer `updatedAt` | Outil direct | — (test négatif volontaire) | **Échec, refus propre** | *« The user edited this in the editor since you last read it, so your update was NOT applied — this protects their changes. Call get_lesson to fetch the current content, re-apply your intended changes on top of it, then call update_lesson again with the new version from that response. »* | Non (ce n'est pas une limite de plan, c'est un contrôle de version) | — | Voir §5 : le message ne contient **aucun code explicite du type `STALE_VERSION`**, c'est un texte pédagogique, pas un code d'erreur machine-lisible | Aucun (rejet confirmé sans effet de bord — voir #14) |
| 13 | `update_lesson` (même leçon) **sans le paramètre `version`** | Outil direct | — (test négatif volontaire) | Échec, rejeté **avant tout appel réseau** | `MCP error -32602: Input validation error: Invalid arguments for tool update_lesson: Invalid input: expected string, received undefined at version` | Non | `version` confirmé obligatoire côté schéma MCP, pas seulement côté serveur | Validation JSON-Schema locale, message assez clair pour un administrateur mais pas pour un utilisateur final | Aucun |
| 14 | `get_lesson` (leçon 1, relecture de contrôle après #12 et #13) | Outil direct | — | Succès | — | Non | — | — | Confirme que le nom et le contenu de la leçon sont restés strictement identiques à l'état issu de #11 — les deux tentatives ratées n'ont produit **aucun effet de bord**, ni partiel ni total |
| 15 | `update_lesson` (leçon 1) avec la `version` correcte obtenue en #14, pour retirer le paragraphe de test ajouté en #11 et laisser la leçon dans un état propre | Outil direct | `get_lesson` (#14) | Succès | — | Non | — | — | Nouvelle `updatedAt` = `2026-09-18T23:16:21.466Z` |

---

## 3. Ce que le test de leçon longue (> 1500 mots) établit

La leçon 7 (« Manuel complet des outils de composition ») a été rédigée à environ 1 900 mots (~12 500 caractères de HTML). **Elle a été créée sans erreur, sans avertissement, et sans troncature apparente.** La relecture (`get_lesson`, appel #6) a permis de comparer le `draftContent` retourné à ce qui avait été envoyé : le texte est identique caractère pour caractère, jusqu'au dernier paragraphe (« Ce qu'il faut retenir »).

**Conclusion pour le manuel d'administration :** ni le schéma de `create_lesson` ni le comportement observé du serveur ne documentent ou n'appliquent de borne de longueur sur le contenu HTML d'une leçon. La limite de 600 à 900 mots mentionnée dans les instructions de l'outil est une **consigne de rédaction adressée au modèle**, pas une contrainte technique du serveur. Un administrateur ne doit donc pas compter sur le produit pour empêcher un contenu disproportionné : c'est une discipline éditoriale à faire respecter en amont, pas un garde-fou technique en aval. Cela dit, cette passe n'a testé qu'une seule taille (~1 900 mots) ; elle ne permet pas d'exclure l'existence d'une limite bien plus haute (par exemple au niveau de la base de données ou d'un CDN), qui resterait à chercher avec un contenu délibérément massif (plusieurs dizaines de milliers de mots) si le sujet devient critique pour l'audit.

---

## 4. Ce que la réorganisation en modules a révélé

`get_modules` (appel #7) a confirmé, avant toute écriture, que les trois nouvelles leçons créées dans cette session (5, 6 et 7) atterrissent **automatiquement et sans action explicite** dans le module `standalone`, dès leur création — comportement cohérent avec la description de l'outil (« The standalone module … holds lessons not assigned to any named module »).

Pour cette passe, un choix délibéré a été fait, distinct d'une simple exécution littérale de la consigne : les leçons 5 et 6 (composition photo, contenu normal) ont été regroupées dans un nouveau « Module 3 — Lumière et tri », tandis que la leçon 7 (le test de longueur) a été **laissée volontairement dans `standalone`** plutôt que rattachée à un module. Ce choix sert un double objectif : respecter la formulation de la consigne (« range les six leçons en trois modules », qui ne mentionne pas la 7e leçon de test), et produire en prime une observation utile — un administrateur peut vouloir savoir ce qu'il advient d'une leçon non classée. Réponse observée : elle reste normalement accessible et listée, simplement hors de toute structure de modules nommée, sous l'intitulé technique `standalone` (à traduire côté interface, selon la documentation de l'outil, par « Leçons sans module »).

Points techniques confirmés par l'appel #8 (`update_modules`), réussi du premier coup :

- **Le module `standalone` doit obligatoirement figurer dans la charge utile**, même s'il ne contient qu'une seule leçon (ou zéro) — comportement déjà connu du contexte transmis, reconfirmé ici.
- **Chaque leçon du produit doit apparaître dans exactement un module** de la charge utile envoyée ; aucune leçon ne peut être omise ni dupliquée (documenté explicitement dans la description de l'action, et vérifié : les 7 leçons ont bien été réparties sans erreur).
- **L'identifiant d'un nouveau module n'a pas besoin d'être un UUID.** L'identifiant `module-3-lumiere-et-tri` (une chaîne lisible, façon slug) a été accepté sans réserve, alors que la plupart des identifiants d'objets dans le reste du catalogue MCP TinyPages sont des UUID stricts avec validation de format. C'est une incohérence de convention à signaler dans le manuel : un administrateur habitué aux UUID pourrait supposer, à tort, qu'un identifiant de module doit lui aussi être un UUID.
- Les identifiants des deux modules préexistants (`module-1-regarder`, `module-2-construire`) ont été repris **exactement tels que renvoyés par `get_modules`**, conformément à la consigne de l'outil (« Preserve module IDs exactly as returned by get_modules »). Aucun test négatif n'a été fait ici pour voir ce qui se passerait si un ID de module existant était modifié à la main — piste laissée ouverte pour une passe ultérieure si le sujet du renommage de module devient pertinent.

---

## 5. Ce que le test du contrôle de version optimiste (`update_lesson`) établit

Trois scénarios ont été enchaînés sur la même leçon (leçon 1) :

1. **`version` correcte, correspondant exactement au `updatedAt` renvoyé par le dernier `get_lesson`** → mise à jour acceptée. La réponse de l'outil ne se contente pas de confirmer le succès : elle **renvoie directement dans son texte la nouvelle valeur de version à réutiliser** pour l'appel suivant (« New version (pass as "version" for your next update): … »). C'est un détail d'ergonomie notable, qui réduit le risque d'erreur d'un administrateur ou d'un agent enchaînant plusieurs modifications : il n'est pas nécessaire de refaire un `get_lesson` entre deux `update_lesson` consécutifs si l'on conserve la valeur renvoyée.
2. **`version` périmée (l'ancienne valeur, devenue obsolète après le succès du scénario 1)** → rejet propre, aucune exception brute ni code HTTP exposé côté MCP. Le message est **rédigé pour un humain, pas pour une machine** : « The user edited this in the editor since you last read it, so your update was NOT applied — this protects their changes. » Il n'expose aucun identifiant technique du type `STALE_VERSION` ou `409 Conflict` — contrairement à ce que le catalogue MCP laisse attendre pour `update_webpage` (où le nom `STALE_VERSION` est mentionné explicitement dans la documentation d'outil). **Ce point est à vérifier spécifiquement pour chaque type d'objet** (leçon, page, article, email) : le message de refus de version périmée n'est peut-être pas uniformisé dans tout le catalogue, ce qui aurait un impact sur la façon dont un futur manuel d'administration doit décrire (ou faire détecter automatiquement par un agent) ce cas d'erreur.
3. **`version` absente du tout** → rejet **avant tout appel réseau**, par la validation de schéma du serveur MCP lui-même (`MCP error -32602`), avec un message explicite (« expected string, received undefined at version »). Ce comportement confirme que `version` est un paramètre réellement obligatoire pour `update_lesson`, malgré son statut de paramètre technique peu visible dans la description generale de l'outil pour un non-développeur.

Une relecture de contrôle (`get_lesson`, appel #14) après les deux tentatives ratées (#12 et #13) a confirmé **l'absence de tout effet de bord** : ni le nom, ni le contenu, ni la date de mise à jour de la leçon n'avaient bougé depuis le dernier succès (#11). Le contrôle de version optimiste est donc, sur cet objet, **strictement atomique et sûr** : une tentative rejetée ne laisse aucune trace partielle.

**Ce qu'un administrateur doit en retenir :** le mécanisme protège correctement contre l'écrasement silencieux d'une modification faite entretemps dans l'éditeur (par l'utilisateur humain, ou par un autre agent) — exactement l'usage annoncé par la documentation de l'outil. Mais le message de rejet est un texte libre en anglais destiné à guider un agent conversationnel, pas un code d'erreur stable et documenté : un système qui voudrait détecter ce cas par programme devra le faire par correspondance de texte (fragile) plutôt que par un code d'erreur (robuste), sauf si un canal d'erreur plus structuré existe ailleurs dans la réponse brute de l'API REST sous-jacente — point qui resterait à vérifier hors MCP, en inspectant directement les réponses HTTP de `app.tinypages.co`, hors du périmètre de cette session.

---

## 6. Ce que les deux articles de blog ont établi

`create_blog_post` a réussi du premier coup pour les deux articles (~640 et ~620 mots chacun, tous deux inférieurs aux ~800 mots visés par la consigne de rédaction de l'outil — écart mineur, non bloquant, à noter pour la prochaine session si le respect strict de la cible de longueur devient un point de contrôle). **Aucune limite de plan gratuit n'a été rencontrée sur la création d'un deuxième article de blog en brouillon.** Ce résultat ne permet cependant pas de conclure à l'absence de toute limite de plan sur le nombre total d'articles de blog publiables ou stockables : seuls deux articles ont été créés dans cette session, et rien n'indique qu'un plafond bas existe avant ce nombre. Si le sujet devient pertinent pour l'audit (existence d'un quota de blog sur le plan gratuit), il faudrait soit consulter la documentation commerciale (page tarifs), soit créer délibérément un grand nombre d'articles jusqu'à obtenir un refus — ce qui sort du périmètre resserré de cette passe.

---

## 7. Synthèse pour le manuel d'administration

| Question | Réponse établie par cette session |
|---|---|
| Une leçon peut-elle dépasser largement 900 mots sans erreur serveur ? | Oui, testé jusqu'à ~1 900 mots sans aucune erreur ni troncature. La limite de longueur documentée est éditoriale, pas technique. |
| `update_modules` exige-t-il de reconnaître le module `standalone` même s'il est vide ou non concerné par le changement ? | Oui, confirmé à nouveau : il doit toujours figurer dans la charge utile complète. |
| Un identifiant de nouveau module doit-il être un UUID ? | Non : une chaîne lisible de type slug a été acceptée sans réserve. |
| Une leçon fraîchement créée est-elle automatiquement classée quelque part ? | Oui, automatiquement dans `standalone`, jamais dans un module nommé existant. |
| `update_lesson` exige-t-il réellement `version` ? | Oui, de façon stricte : absence rejetée côté validation MCP avant tout appel réseau (`-32602`), valeur périmée rejetée côté serveur avec un message clair mais non structuré (pas de code `STALE_VERSION` explicite dans le texte reçu). |
| Un rejet de version périmée laisse-t-il un effet de bord partiel ? | Non, confirmé par relecture : aucun champ n'a bougé lors des deux tentatives ratées. |
| La création d'un deuxième article de blog en brouillon rencontre-t-elle une limite de plan gratuit ? | Non rencontrée dans cette session, mais échantillon trop faible (2 articles) pour conclure à l'absence de tout quota. |

---

## 8. Limites de cette session

- Le test de longueur de leçon ne couvre qu'une seule taille (~1 900 mots) ; il ne permet pas d'établir où se situerait une éventuelle limite réelle si elle existe à une échelle beaucoup plus grande.
- Le message de rejet de version périmée n'a été observé que sur `update_lesson`. Sa cohérence avec le comportement de `update_webpage`, `update_blog_post` ou `update_automation_email` (qui documentent tous un paramètre `version` similaire) n'a pas été vérifiée ici et resterait à confronter, notamment parce que le catalogue MCP mentionne littéralement `STALE_VERSION` pour `update_webpage`, alors que ce texte n'est pas apparu ici pour `update_lesson` — écart de terminologie à investiguer.
- Aucune tentative n'a été faite pour renommer un module existant en changeant son `id` (test négatif non joué), ni pour omettre un module existant de la charge utile de `update_modules` (comportement attendu : erreur 400, mais non redéclenché ici puisque déjà établi par le contexte transmis en amont).
- Le nombre total d'articles de blog autorisés sur le plan gratuit n'a pas été atteint ni cherché.

---

## 9. Objets créés ou modifiés dans cette session (pour nettoyage ultérieur, préfixe `POC`)

| Type | Nom / titre | ID |
|---|---|---|
| Leçon | 5. La lumière comme matière | `7367bbb3-993d-4bf3-aa30-06e59578ac8f` |
| Leçon | 6. Trier ses images après la prise de vue | `0c81a2b5-cc71-4e4a-8813-4710c44dbcfc` |
| Leçon | 7. Manuel complet des outils de composition | `bd57784e-3823-4991-a1b2-04eb789a0f1f` |
| Module | Module 3 — Lumière et tri | `module-3-lumiere-et-tri` |
| Article de blog | POC — 5 habitudes de composition à corriger cette semaine | `550d174d-ba30-45d9-bf03-e208e5ebfad7` |
| Article de blog | POC — Le tri photo, l'étape que tout le monde bâcle | `1efd04ca-86c9-4f0b-9b88-a070515d0ee9` |
| Leçon modifiée (contenu restauré à l'identique après tests de version) | 1. Voir avant de déclencher | `2896330e-1c52-43e6-8ddf-c28ca70d98b4` |

Rappel : aucune action de suppression n'existe dans le catalogue MCP pour les leçons, modules nommés ou articles de blog — le nettoyage, s'il est décidé, devra se faire à la main dans l'interface `https://app.tinypages.co`, en filtrant sur le préfixe `POC`.
