# Passe 3 — « Acquisition » — exécution

18 septembre 2026, 23:12 à 23:14 UTC. Compte en **plan gratuit** (`nathan-lahy.tinypages.co`, locale `fr`, `doubleOptin: false`). Tout en brouillon, aucune publication (`publish_*` jamais appelé), aucun envoi ni programmation d'email (`send_email`/`schedule_email` jamais appelés).

Repris tels quels, sans recréation, les objets déjà posés par l'orchestrateur avant cette passe : page d'inscription « POC Les 7 erreurs de cadrage » (`94ede07c-a02d-4dd6-968b-6aba321f3e65`), formulaire « POC Diagnostic cadrage » (`17c02675-ebc6-464d-86e4-5ce641644252`, 3 pages), étiquette « POC — prospect diagnostic » (`952b281d-b748-4b3b-8825-adba66f8e7a3`), contact fictif `poc-test-01@example.invalid` (`05447cbc-e5c3-4688-a05b-19b7fc0705ff`), produit espace membre « POC Cadrage Net » (`85dd81bd-14f5-4850-92a4-ff5ceab3c0c2`, non touché ici — aucun produit créé, quota gratuit déjà saturé d'après la passe 1).

## Objets créés dans cette passe

| Objet | Identifiant | État |
|---|---|---|
| Page « POC Merci diagnostic cadrage » | `5d288e61-e5ed-4436-967e-14d37edc55b9` | brouillon, slug `poc-merci-diagnostic-cadrage`, sans bannière |
| Destination de formulaire « POC — Débutant vers page de remerciement » | `ba7cbd86-4a70-44c0-b698-7a0af68295ae` | sur le formulaire `17c02675-...` |
| Email automatisation étiquette, J0 « Votre diagnostic de cadrage est arrivé » | `6901546f-6258-4770-9c8d-6aeb3e04af5e` | brouillon |
| Email automatisation étiquette, J3 « L'exercice pour votre cadrage » | `15696ae4-ddce-42cc-82f6-d882dc2b9ff2` | brouillon |
| Email automatisation « nouveau contact », J0 « Bienvenue, avant toute chose » | `7c07e5d5-1999-4a6c-ac11-8cb8af1cf420` | brouillon |

Aucun produit, aucune étiquette et aucun contact supplémentaires créés : les objets existants ont suffi pour tout le reste de la passe.

## Séquence des appels, dans l'ordre réel d'exécution

| # | Action | Voie | Résolution préalable d'identifiant | Résultat | Message d'erreur exact | Piège à éviter |
|---|---|---|---|---|---|---|
| 1 | `create_webpage` — page de remerciement | Outil direct | Aucune (page nouvelle) | Succès au premier essai | — | — |
| 2 | `search_actions` (×7, en parallèle) — localiser `get_form`, `add_form_destination`, `list_automations`, `list_automation_emails`, `add_tag_to_contact`, `remove_tag_from_contact`, `list_contacts` | Recherche de catalogue | — | Succès, schémas complets renvoyés | — | Le catalogue renvoie souvent plusieurs actions voisines par requête (ex. `add_form_destination` est revenu dans 2 recherches différentes avec, la deuxième fois, `update_form_destination` en prime) : lire le schéma renvoyé plutôt que supposer qu'un seul candidat existe |
| 3 | `get_form` sur le formulaire existant | Catalogue (`execute_action`) | Aucune — l'ID du formulaire était déjà connu (fourni par le contexte) | Succès. Renvoie les 3 pages avec, pour chaque champ, son `nodeId` (ex. `fJu3TrwrZI` pour le choix unique de la page 2) et une destination par défaut nommée « Everyone » (`type: "success"`, déjà présente sans action de notre part) | — | Les `nodeId` ne sont **pas** dans les métadonnées `fields[].nodeId` seulement : ils sont aussi encodés en attribut HTML dans `draftContent` (`<formSingleChoice nodeId="fJu3TrwrZI" ...>`). Les deux sources concordent ici, mais c'est `fields[].nodeId` qui est directement exploitable, pas la relecture manuelle du HTML |
| 4 | `search_tags` — résoudre l'étiquette « POC — prospect diagnostic » | Catalogue | Recherche par nom exact | Succès, 1 résultat, ID confirmé identique à celui donné en contexte (`952b281d-b748-4b3b-8825-adba66f8e7a3`) | — | Résoudre quand même par recherche, même quand l'ID semble déjà connu : c'est la seule façon de garantir qu'il n'a pas changé entre deux tours d'agent |
| 5 | `search_contacts` (email exact) — résoudre le contact fictif | Catalogue | Recherche par email exact | Succès, ID confirmé identique à celui du contexte, `tags: []` (aucune étiquette au départ) | — | — |
| 6 | `list_automations` (baseline, avant toute création d'email d'automatisation) | Catalogue | Aucune | Succès, `{"automations":[]}` — **liste vide alors que l'étiquette existe déjà** | — | Voir constat P3-01 ci-dessous : une étiquette sans email attaché n'apparaît pas dans `list_automations` |
| 7 | `get_webpage` sur la page de remerciement créée à l'étape 1 | Outil direct | ID renvoyé par l'étape 1 | Succès, `slug: "poc-merci-diagnostic-cadrage"` | — | L'URL publique n'est **pas** renvoyée par `create_webpage` (seul un `editorUrl` interne l'est) ; il faut un second appel pour obtenir le `slug` |
| 8 | `get_account` (catalogue, via `search_actions` implicite puis appel direct) | Catalogue | Aucune | Succès, `domain: "nathan-lahy.tinypages.co"` | — | Le domaine du compte n'est documenté nulle part dans les schémas des outils de page : il faut le lire sur `get_account` pour construire une URL publique réelle |
| 9 | `add_form_destination` — règle de routage sur le formulaire | Catalogue | `nodeId` du champ (étape 3), ID de l'étiquette (étape 4), URL réelle reconstruite (étapes 7-8) | Succès au premier essai | — | Voir constat P3-02 : `type` renvoyé est `"disqualification"` même si l'intention métier est de qualifier un segment, pas de le disqualifier |
| 10 | `create_automation_email` — email J0, automatisation étiquette | Outil direct | `automationId` = ID de l'étiquette (étape 4), **pas** un identifiant d'automatisation distinct | Succès au premier essai | — | Voir constat P3-03 sur la résolution de `automationId` pour une automatisation d'étiquette |
| 11 | `create_automation_email` — email J3, même automatisation | Outil direct | Même `automationId`, aucune nouvelle résolution nécessaire | Succès au premier essai | — | — |
| 12 | `create_automation_email` — email J0, automatisation « nouveau contact » | Outil direct | `automationId` = chaîne littérale `"newContact"`, aucune recherche | Succès au premier essai | — | Voir constat P3-04 : comparaison des deux modes de résolution |
| 13 | `list_automations` (après création des 3 emails) | Catalogue | Aucune | Succès, les deux automatisations apparaissent désormais (`type: "tag"`, `emailCount: 2` ; `type: "event"`, `id: "newContact"`, `emailCount: 1`) | — | — |
| 14 | `list_contacts` (`tagId` = étiquette) — baseline avant application | Catalogue | ID de l'étiquette (étape 4) | Succès, `data: []` | — | — |
| 15 | `add_tag_to_contact` | Outil direct | ID du contact (étape 5) + ID de l'étiquette (étape 4) | Succès au premier essai, renvoie la liste des étiquettes du contact avec horodatage | — | — |
| 16 | `list_contacts` (`tagId` = étiquette) — vérification après application | Catalogue | — | Succès, le contact apparaît avec l'étiquette et son `addedAt` | — | — |
| 17 | `remove_tag_from_contact` | Outil direct | Mêmes IDs qu'à l'étape 15 | Succès au premier essai, renvoie `tags: []` | — | — |
| 18 | `list_contacts` (`tagId` = étiquette) — vérification après retrait | Catalogue | — | Succès, `data: []` — retour exact à l'état de l'étape 14 | — | — |
| 19 | `list_automation_emails` (`automationId` = étiquette) | Catalogue | ID de l'étiquette | Succès, les 2 emails renvoyés avec leur `metadata` complète | — | Voir constat P3-05 sur `sendHour` |
| 20 | `list_automation_emails` (`automationId` = `"newContact"`) | Catalogue | Chaîne littérale | Succès, le seul email renvoyé | — | — |

**Aucun échec n'a été rencontré dans cette passe.** Les 20 appels ont tous réussi au premier essai — un résultat en soi à consigner : la passe 3 n'a produit aucun message d'erreur verbatim à documenter, contrairement à la passe 1 (coupon, `update_modules`). Cela ne prouve pas l'absence de pièges ailleurs dans le module « Formulaires » et « Automatisations », seulement qu'ils ne sont pas apparus sur ce périmètre précis.

## Constats

### P3-01 — Une automatisation « fantôme » n'existe pas tant qu'aucun email n'y est attaché

Avant la création du premier email, `list_automations` renvoyait `{"automations":[]}` **alors que l'étiquette « POC — prospect diagnostic » existait déjà** et que le compte a un contact créé depuis plus d'une minute (donc éligible à l'automatisation « nouveau contact »). Après la création d'un email sur chacune des deux automatisations, les deux apparaissent immédiatement avec le bon `emailCount`.

Conséquence pour le manuel : `list_automations` ne liste pas « les déclencheurs possibles », mais « les automatisations qui ont au moins un email configuré ». Un administrateur qui chercherait à confirmer qu'une étiquette a bien un mécanisme d'automatisation actif avant d'y avoir ajouté un email ne trouvera rien — ce n'est pas une absence de fonctionnalité, c'est que l'objet « automatisation » n'a pas d'existence propre indépendamment de ses emails.

Ceci répond à la question posée par le plan de génération (§3.3) : *« une automatisation déclenchée par une étiquette se crée-t-elle exactement comme une automatisation déclenchée par un achat, ou le comportement diverge-t-il selon le type de déclencheur ? »* — le comportement est identique entre étiquette et achat sur ce point ; il diverge en revanche pour « nouveau contact », qui est un événement (`type: "event"`) plutôt qu'une ressource (`type: "tag"` / `type: "product"`), voir P3-04.

### P3-02 — Le type `"disqualification"` d'une destination de formulaire ne dit rien de l'intention métier

Le schéma de `add_form_destination` ne propose qu'un seul type de destination à créer : le champ `type` renvoyé par le serveur vaut systématiquement `"disqualification"` pour toute destination ajoutée par cet appel, quelle que soit la logique voulue. Dans cette passe, la règle ajoutée route en réalité un segment qualifié (les débutants) vers une page de suivi, ce qui est plus proche d'une « qualification » que d'une disqualification au sens strict — mais l'objet créé porte quand même l'étiquette technique `"disqualification"`, en miroir de la seule destination `"success"` préexistante (nommée « Everyone », créée automatiquement à la création du formulaire, non modifiable via `add_form_destination`).

Conséquence pour le manuel : ne pas se fier au champ `type` pour documenter l'intention d'une règle de routage auprès d'un client — seul le nom donné à la destination (`name`) porte l'information utile, et il est entièrement libre, donc à discipliner par convention interne.

### P3-03 — Résolution de `automationId` pour une automatisation d'étiquette

Il n'existe **aucun identifiant d'automatisation distinct de l'identifiant de l'étiquette**. Le paramètre `automationId` de `create_automation_email`, `list_automation_emails`, `add_automation_stop_condition`, etc. attend directement l'UUID de l'étiquette elle-même (`952b281d-b748-4b3b-8825-adba66f8e7a3`), obtenu par `search_tags` ou `list_automations` (une fois qu'au moins un email existe — voir P3-01, donc au moment de la toute première configuration, seul `search_tags` fonctionne). Il n'y a pas d'appel du type « créer une automatisation » distinct de la création du premier email : la première invocation de `create_automation_email` sur un `automationId` = ID d'étiquette **crée l'automatisation implicitement**, sans étape préalable dans l'interface ni appel dédié.

### P3-04 — Résolution de `automationId` pour la automatisation « nouveau contact » : comparaison

Contrairement au cas de l'étiquette, `automationId` vaut ici la **chaîne littérale `"newContact"`**, documentée directement dans le schéma des outils (`list_automations`, `create_automation_email`, etc.) : *« "newContact" for the new-contact automation »*. Aucune recherche, aucun UUID, aucun appel `search_*` ou `list_*` n'est nécessaire pour l'obtenir — c'est une constante de la plateforme, valable pour tout compte.

Autre différence observée après coup : `list_automations` restitue cette automatisation avec `"type": "event"` (contre `"type": "tag"` pour l'étiquette), et l'`editorUrl` renvoyé par `create_automation_email` encode ce même type dans son paramètre de requête (`trg=event&trgv=newContact` contre `trg=tag&trgv=<uuid>` pour l'étiquette). La distinction « ressource nommée par un UUID à résoudre » vs « constante littérale déjà connue » est donc le seul écart réel de résolution d'identifiant entre les deux mécanismes ; la mécanique de création de l'email elle-même (mêmes paramètres, même comportement d'implicite « création de l'automatisation au premier email ») est identique.

### P3-05 — `sendHour` par défaut sur un email à `sendDay: 0` : incohérent avec la documentation du schéma

La description du paramètre `sendHour` dans le schéma de `create_automation_email` indique : *« sendHour is ignored when sendDay=0 »*, laissant entendre qu'aucune valeur n'est stockée ou que sa valeur est sans effet. En pratique, `list_automation_emails` restitue tout de même une valeur pour les deux emails créés à `sendDay: 0` (celui de l'étiquette et celui de « nouveau contact ») : `"sendHour": "02:00"` dans les deux cas — une valeur différente du `"09:00"` par défaut obtenu, lui, sur l'email à `sendDay: 3` (envoyé sans que `sendHour` ait été fourni explicitement dans aucun des trois appels).

Deux lectures possibles, non tranchées ici faute d'accès aux journaux serveur :
1. `"02:00"` est un artefact de stockage sans effet réel (cohérent avec « ignored »), et seule sa valeur pour `sendDay > 0` (`"09:00"`, conforme à la valeur par défaut documentée dans le schéma d'`update_automation_email`) compte réellement.
2. `"02:00"` est bel et bien utilisé quelque part (fuseau de stockage en UTC d'un défaut serveur non documenté), ce qui contredirait la mention « ignored ».

Conséquence pour le manuel : ne pas confier cette zone grise à un agent sans supervision humaine si un client demande un jour un envoi immédiat (`sendDay: 0`) suivi d'une lecture d'heure exacte dans un tableau de bord — la valeur affichée pourrait surprendre l'utilisateur (« pourquoi 2h du matin ? ») même si elle n'a, en théorie, aucun effet sur le moment réel d'envoi.

### P3-06 — Cycle complet d'étiquette confirmé par lecture, pas seulement par le code de retour

`add_tag_to_contact` et `remove_tag_from_contact` renvoient tous deux un corps de réponse cohérent avec l'action demandée (liste des étiquettes du contact, vide après retrait). Mais conformément à la consigne de cette passe, l'effet a été revérifié de façon indépendante par `list_contacts(tagId=...)` avant application (liste vide), après application (le contact apparaît), et après retrait (liste de nouveau vide) — les trois lectures concordent exactement avec les réponses des deux appels d'écriture, sans divergence observée. Ce contrôle croisé est ce qui permet d'affirmer avec certitude que l'effet est réel et pas seulement déclaré par un code de retour optimiste.

### P3-07 — La description de `add_tag_to_contact` avertit d'un effet de bord qu'il faut relayer

Le schéma de `add_tag_to_contact` porte cet avertissement explicite : *« Warn the user: if an automation is attached to this tag, its emails will start sending to this contact immediately »*. Dans cette passe, les deux emails de l'automatisation d'étiquette étant restés à l'état `draft` (jamais publiés via `publish_automation_email`), l'application de l'étiquette au contact fictif à l'étape 15 n'a déclenché aucun envoi réel — comportement attendu et non vérifiable autrement que par l'absence d'erreur ou de notification, `list_automation_email_recipients` n'ayant pas été interrogé faute d'objet à y trouver (aucun envoi n'a eu lieu). Point de vigilance pour la suite : sur un compte où l'automatisation serait déjà publiée, ce même appel `add_tag_to_contact` aurait un effet observable et irréversible (email réellement envoyé) — à ne jamais reproduire sur un compte client.

## Pour le manuel

Les points à retenir de cette passe, par ordre d'importance pour un administrateur :

1. Une automatisation (étiquette ou « nouveau contact ») n'a pas d'existence observable avant qu'un premier email lui soit attaché : ne pas s'étonner qu'elle soit absente de `list_automations` au moment de sa toute première configuration.
2. `automationId` pour une étiquette est directement l'UUID de l'étiquette (à résoudre par `search_tags`) ; pour le contact nouveau, c'est la constante littérale `"newContact"`, sans résolution. Ce sont les deux seuls schémas rencontrés dans le catalogue MCP pour ce paramètre, plus le cas `productId` documenté en passe 1.
3. Le champ `type` d'une destination de formulaire créée via `add_form_destination` vaut toujours `"disqualification"` : ne pas s'en servir pour documenter l'intention métier auprès d'un client, utiliser le champ `name` à la place.
4. `sendHour` sur un email à `sendDay: 0` n'est pas réellement ignoré au niveau du stockage malgré la documentation du schéma — une valeur (`"02:00"` observée ici) est bien enregistrée, sans certitude sur son usage réel.
5. Toute lecture de vérification (`list_contacts` avec `tagId`) doit être faite **avant et après** chaque appel d'écriture, jamais en se fiant au seul corps de réponse de l'appel d'écriture : c'est la seule façon de distinguer un effet réellement appliqué d'un accusé de réception optimiste.
6. `add_tag_to_contact` sur une étiquette dont l'automatisation est publiée déclenche un envoi réel et immédiat — jamais à reproduire sur un compte client, uniquement sur le compte de test avec des automatisations laissées en brouillon.

## Limites de cette passe

- Aucune soumission réelle du formulaire n'a été effectuée (interdit par les contraintes) : la destination créée à l'étape 9 n'a donc pas pu être observée en fonctionnement (redirection effective, application de l'étiquette au moment de la soumission). Seule la configuration de la règle a été vérifiée, pas son déclenchement.
- `list_form_submissions` n'a pas été appelé dans cette passe (déjà couvert par le plan comme test négatif ; la liste est nécessairement vide puisqu'aucune soumission n'a eu lieu et que le formulaire n'a jamais été publié).
- L'hypothèse 2 du constat P3-05 (usage réel de `sendHour` pour `sendDay: 0`) n'a pas pu être tranchée sans accès aux journaux d'envoi côté serveur, qui n'existent de toute façon pas ici puisqu'aucun envoi n'a eu lieu.
