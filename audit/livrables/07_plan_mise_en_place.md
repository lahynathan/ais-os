# 07 — Plan de mise en place

Data room TinyPages · Date de référence : 18 septembre 2026 · Rédigé par l'orchestrateur d'audit (A00). Traite les lignes R-01 à R-48 de `06_registre_risques.md`.

> **Numérotation.** Les identifiants **P0-xx, P1-xx et P2-xx de ce document sont la seule numérotation de remédiation du dossier.** Le livrable 05 portait une numérotation P0-1 à P0-12 qui lui était propre et qui entrait en collision avec celle-ci — P0-6 et P0-12 désignaient deux choses différentes selon le document. Elle est supprimée : 05 renvoie désormais aux identifiants ci-dessous, et aucun autre livrable n'en crée. Toute phrase du type « P0-13 est livré » n'a de sens que par référence à ce document. La série est **sans trou et sans doublon** : P0-01 à P0-25, P1-01 à P1-28, P2-01 à P2-11. P0-25, P2-11 et la ligne de risque R-48 sont ouverts par le second passage du contre-audit. Un identifiant reclassé d'une phase à l'autre — c'est le cas de P1-23, remonté en P0-21 — conserve sa ligne à sa place d'origine, avec renvoi et effort ramené à zéro, plutôt que d'être supprimé : un identifiant cité ailleurs ne doit jamais devenir introuvable, et aucun identifiant n'est réattribué.

---

## 0. Conventions

**Phases.** Le tour n'est pas arrêté, aucune date de closing n'est citée.
- **P0** : avant l'ouverture de la data room.
- **P1** : avant le closing.
- **P2** : après le closing.

Deux dates calendaires seulement figurent dans ce plan, parce qu'elles sont réglementaires et dures : l'article 50 de l'AI Act est applicable depuis le **2 août 2026**, et le marquage lisible par machine des contenus générés est exigible au **2 décembre 2026**.

**Effort.** En jours-personne (j·p), travail interne. Les honoraires des rôles externes à mandater — conseil unique sur les trois qualifications fiscale, comptable et consommation (avocat fiscaliste et expert-comptable), avocat conformité et propriété intellectuelle, prestataire de test d'intrusion — ne sont pas convertis en j·p. Ils sont chiffrés à part, en hypothèse, au §5.4.

**Responsables.** CTO et CEO sont la même personne, Nathan Lahy. Le plan les distingue par nature de décision, technique ou société. Cette concentration est le risque R-26, et elle a une conséquence directe sur ce plan. À son périmètre d'origine, P0 pesait **39,5 j·p** — 13,5 au §2.1 et 26 au §2.2 —, soit **huit semaines** pleines pour une seule personne ; le chiffre de « 40 j·p » que portait ce paragraphe était un arrondi qui ne se recomposait pas avec ses deux sous-totaux, il est retiré. Le « environ sept semaines » que portait R-26 est arbitré contre huit, et corrigé depuis. À son périmètre actuel, après le reclassement du rejeu de la phase 1 (P0-21), l'ouverture des lignes R-43 à R-47 et celle de P0-25 (test d'autorisation objet par objet, R-48), P0 pèse **54 j·p, soit onze semaines**. Le calcul qui compte n'est pas celui-là : c'est **P0 + P1 = 215 j·p, soit 43 semaines pour une personne seule**, et il est écrit au §5.1. C'est le premier argument du plan de renfort, et il vaut mieux l'écrire que le laisser calculer par quelqu'un d'autre.

**Une distinction structure tout le document.** Certaines lignes se corrigent en une journée : un gabarit, une phrase de FAQ, un fichier `security.txt`, une pièce à verser. D'autres sont des chantiers de plusieurs semaines : l'isolation de domaine, le découpage du catalogue MCP, le marquage des contenus générés. Confondre les deux est le meilleur moyen de ne faire ni l'un ni l'autre. Les sections 2 à 4 les séparent explicitement.

---

## 1. État cible

Sept énoncés. Chacun est vérifiable, et chacun est aujourd'hui faux au moins en partie.

**1. Ce qui protège l'utilisateur vit dans le serveur, pas dans une consigne.** Publier et envoyer exigent un jeton de confirmation côté serveur. Les actions irréversibles sont des outils nommés et annotés, pas des entrées d'un catalogue atteint par un appel générique. Aujourd'hui, l'inverse est vrai : seuls les contrôles commerciaux sont appliqués par le serveur (R-01, R-02, R-04).

**2. Tout ce que fait l'IA est visible et réversible.** Journal horodaté par action, distinguant acteur humain et acteur IA, session MCP et compte cible, consultable par le créateur. Différentiel affiché avant publication. Annulation en un clic sur 30 jours. Aujourd'hui, le serveur ordonne au modèle de ne rien montrer et aucun journal n'est documenté (R-13, R-14).

**3. Les garde-fous sont mesurés, pas seulement écrits.** Une batterie d'évals versionnée, rejouée à chaque modèle et chaque client supporté, avec seuils bloquants et journal de résultats joint à la data room (R-05).

**4. Un compte neuf est conforme le jour de sa création.** Pages légales avec un contenu réel, non indexées tant qu'elles sont vides, double opt-in actif, preuve de consentement stockée avec chaque contact, information AI Act sur les surfaces où l'utilisateur dialogue avec l'IA. Aujourd'hui, la plateforme publie et fait indexer des documents juridiques vides sur chaque compte (R-03, R-18, R-35).

**5. Les sites clients ne peuvent pas emprunter la confiance de l'application.** Domaine enregistrable distinct pour les sites clients, cookie de session en `__Host-`, origines en liste blanche, sandbox du code personnalisé hors du domaine principal. C'est le seul énoncé de cette liste qui ne peut pas être atteint avant l'ouverture de la data room ; il y est donc exposé chiffré et daté (R-07, R-34).

**6. Le discours résiste à une vérification de dix minutes.** Aucune allégation absolue sans dossier de preuve daté, aucune contradiction entre le site et la documentation, aucune formulation présentant une consigne de prompt comme une garantie technique (R-08, R-32).

**7. La société est documentée comme une société.** Entité, statuts, table de capitalisation, cession des droits, portefeuille de marques et de domaines, configuration Stripe, inventaire des fournisseurs, SBOM. Aucune de ces pièces n'a pu être établie pendant l'audit (R-09, R-10, R-23, R-39, R-42).

---

## 2. P0 — avant l'ouverture de la data room

Objectif : que l'auditeur mandaté ne trouve rien que le dossier ne dise déjà, que les constats confirmés par exécution — R-01, R-02, R-03 et R-11 — soient accompagnés d'une mitigation engagée, et que le dossier repose sur des faits relevés par TinyPages plutôt que sur des sources secondaires (P0-21).

### 2.1 Ce qui se corrige en une journée ou deux

| ID | Action | Effort | Responsable | Risques traités |
|---|---|---|---|---|
| P0-01 | **Gabarit des pages légales.** Rédiger un contenu réel de politique de confidentialité et de conditions d'utilisation, conditionner la publication à un contenu non vide, forcer `indexed: false` tant qu'elles ne sont pas remplies, et rétro-traiter le parc existant par dépublication ou `noindex`. **Réserve d'estimation, à lire avec le chiffre :** les 2 j·p couvrent la rédaction, la relecture et le correctif de gabarit. Le **rétro-traitement du parc existant n'est pas chiffrable** : le nombre de comptes et de pages légales vides déjà en ligne est **inconnu** — aucun relevé de parc n'a pu être fait (R-31), et l'outillage de traitement de masse est lui aussi inconnu. Un chiffrage de deux jours sur un parc de taille inconnue n'est pas un chiffrage. Le relevé du parc est fait en P0-21 ; le chiffre est à réviser une fois ce relevé disponible. | 2 j·p pour le gabarit, **rétro-traitement non chiffrable en l'état** (+ relecture avocat) | CTO, avocat conformité | R-03 |
| P0-02 | **Consentement par défaut.** Activer le double opt-in par défaut, stocker date, source et texte du consentement avec chaque contact, revoir le bloc de capture de la page d'accueil créée par défaut. | 2 j·p | CTO | R-03, R-28, R-35 |
| P0-03 | **Sous-domaines.** Liste de mots réservés (app, admin, api, login, auth, secure, pay, checkout, billing, account, support, status, mail, cdn, docs, mcp, staging), refus de tout identifiant contenant « tinypages » ou une variante, quarantaine d'au moins douze mois des identifiants libérés avec page « ce site n'existe plus ». | 1 j·p | CTO | R-22 |
| P0-04 | **Paquet de signalement.** `security.txt` conforme à la RFC 9116 sur le site et l'application, page publique de signalement d'abus, alias `abuse@`, point de contact autorités et point de contact destinataires au sens des articles 11 et 12 du DSA. | 1 j·p | CTO, CEO | R-29, R-38 |
| P0-05 | **Correction du discours.** Retirer ou relativiser « seule plateforme », trancher C-001, C-002 et C-003 par une phrase unique publiée au même endroit dans la FAQ et dans la documentation, avec la liste des actions réservées au plan Pro. Corriger `docs.tinypages.co/getting-started/1-4-mcp-setup`, dont l'affirmation sur l'envoi d'emails est fausse. | 1 j·p | CEO | R-08, R-32, R-02 |
| P0-06 | **Note de position sur les garde-fous.** Une page pour la data room, reprenant la formulation défendable du rapport A04 §2 bis : comportement par défaut du modèle, non contrôle d'accès. Toute autre formulation sera démentie en un appel. | 0,5 j·p | CEO, CTO | R-01, R-02 |
| P0-07 | **AI Act, article 50(1).** Mention visible partout où un utilisateur final dialogue avec l'IA, et décision écrite sur le périmètre. L'article est applicable depuis le 2 août 2026. | 1 j·p | CTO | R-18 |
| P0-08 | **Pièces d'entité.** Extrait d'immatriculation, statuts, table de capitalisation, mentions légales, titulaire de chaque nom de domaine, `.co`, `.dev` et `.com`. | 1 j·p | CEO | R-10, R-39 |
| P0-09 | **Mandat unique sur les trois qualifications** — fiscale, comptable, consommation. Établir d'abord les trois faits qui commandent les trois réponses : qui figure sur la facture à l'acheteur final, qui fixe les CGV du tunnel d'achat, qui commande la mise à disposition du fichier. Mandater ensuite le conseil sur **les trois volets à la fois**, parce que c'est le même faisceau de faits, le même rendez-vous et la même liasse de pièces : (a) **TVA**, présomption de l'article 9 bis (R-06) ; (b) **comptabilité, principal ou agent** — le chiffre d'affaires se présente-t-il en volume brut des ventes des créateurs ou en commission nette, question qui pèse directement sur la valorisation (R-44), avec la position écrite de l'expert-comptable sur la reconnaissance du revenu ; (c) **droit de la consommation** — qui est le vendeur professionnel vis-à-vis de l'acheteur final, donc qui porte la garantie de conformité, la rétractation et le litige (R-45). Exiger une **réponse unique et cohérente entre les trois registres** : une divergence entre eux est en elle-même une anomalie relevée en due diligence. **Délai externe à intégrer au calendrier : deux à quatre semaines entre le mandat et la note.** À poster le premier jour de P0. | 2 j·p (+ honoraires) | CEO, avocat fiscaliste, expert-comptable, avocat conformité | R-06, R-44, R-45 |
| P0-10 | **Six vérifications factuelles.** Chacune prend moins d'une heure et referme une question ouverte : suppression définitive d'un contact en interface (R-20), envoi d'un message sur un compte Pro (R-02), relevé des requêtes réseau de la page d'accueil avec un navigateur vierge, capture à l'appui (R-27), relevé des en-têtes de sécurité et de la configuration TLS (R-38), type d'intégration PayPal et prélèvement effectif de la commission (R-37), attribut `sandbox` complet et origine du bloc de code (R-34). | 2 j·p | CTO | R-02, R-20, R-27, R-34, R-37, R-38 |
| P0-11 | **Hygiène d'audit.** Supprimer les **trois** traces laissées par les tests, dont aucune ne peut l'être par le canal automatisé : les deux brouillons résiduels, et le **champ de contexte métier du compte, qui porte `-` au lieu de sa valeur d'origine, vide** (M-018, R-11). Puis ouvrir un compte de test réellement dédié pour toute vérification ultérieure, et joindre à la data room la note de base de preuve, y compris l'écart sur le compte MCP. | 1 j·p | CTO | R-31 |
| P0-22 | **Relevé d'authentification des acheteurs et des élèves.** Répondre par écrit, avec captures, à la question que le dossier n'a posée nulle part : un acheteur ou un élève a-t-il une **identité TinyPages globale réutilisée chez plusieurs créateurs** ? Relever le mécanisme de connexion à un espace membre, l'hôte et la portée du cookie de session acheteur, le parcours de réinitialisation de mot de passe. Ce sont les seuls utilisateurs qui paient, et leur surface d'authentification n'est examinée dans aucun des neuf livrables. Le relevé prend moins d'une journée et décide si R-43 est théorique ou réel. | 1 j·p | CTO | R-43 |
| P0-23 | **Information sur le flux de données vers l'éditeur du client IA.** Mention affichée au moment où un créateur autorise son client — Claude, ChatGPT ou autre : quelles catégories de données peuvent sortir (contacts, soumissions de formulaires, listes de destinataires, chiffre d'affaires), vers qui, et que **le contrat applicable est celui que le créateur a souscrit à titre personnel**, avec ses propres règles d'entraînement et de rétention. C'est une mention d'information, pas un chantier ; la clause contractuelle et la portée en lecture seule suivent en P1. | 1 j·p | CTO, CEO | R-46 |
| P0-24 | **Statut du connecteur MCP TinyPages hébergé par un tiers.** Établir en un appel : ce connecteur Zapier est-il celui de TinyPages, est-il autorisé, qui l'exploite, par quel canal passent les identifiants des créateurs, existe-t-il un contrat. Le dossier le cite trois fois comme argument concurrentiel et jamais comme chemin de données. Produire l'inventaire des intégrations tierces publiées sous la marque, qui n'existe pas. | 1 j·p | CEO, CTO | R-47 |
| P0-25 | **Test d'autorisation au niveau de l'objet, entre locataires (IDOR / BOLA).** Depuis une session du locataire A, appeler `get_webpage`, `get_form`, `get_email`, `get_lesson`, `get_blog_post` et `get_product_stats` avec des identifiants d'objets du locataire B, puis les mêmes appels par la passerelle `execute_action`. **C'est une lecture** : rien n'est créé, publié, envoyé ni modifié, et c'est le test le moins risqué du protocole. **Critère de sortie :** chaque appel doit être refusé **pour cause d'autorisation**, et non pour cause d'objet introuvable — un « 404 » générique n'est acceptable que s'il est établi qu'il vient d'un contrôle d'appartenance. Protocole complet en 05 §5.5 (T-0). **Dépendance dure : la création du second locataire dédié (DR-132)** ; le test n'a pas pu être exécuté pendant l'audit faute d'identifiant étranger à soumettre (`subAccounts: []`, M-008), et il ne peut pas l'être avant. Résultat versé à la data room quel qu'il soit, et reporté en R-48. | 0,5 j·p | CTO | R-48, R-16 |

Sous-total : **18 j·p** — 13,5 à l'origine, plus 1 j·p sur P0-09 élargi aux trois qualifications, 3 j·p de lignes nouvelles du premier contre-audit (P0-22, P0-23, P0-24) et 0,5 j·p de P0-25, ouvert par le second. **13,5 + 1 + 3 + 0,5 = 18.**

### 2.2 Ce qui demande un début de chantier, engagé avant l'ouverture

| ID | Action | Effort | Responsable | Risques traités |
|---|---|---|---|---|
| P0-12 | **Limites de débit et file d'attente** par compte et par IP sur `create_*` et `publish_*`, appliquées d'abord au plan gratuit, avec mise en file au delà d'un seuil. Première barrière réelle à l'abus, en attendant la confirmation serveur. | 3 j·p | CTO | R-01, R-21 |
| P0-13 | **Journal des actions MCP.** Horodatage, acteur humain ou IA, session, compte cible, action, objet, consultable par le créateur. Sans journal, personne ne peut dire après coup ce que l'IA a fait. | 5 j·p | CTO | R-14, R-01, R-16 |
| P0-14 | **Nettoyage des instructions serveur.** Retirer les règles qui gouvernent le modèle hôte hors du périmètre TinyPages et celles qui interdisent de montrer le contenu produit. Un auditeur de sécurité d'Anthropic ou d'OpenAI qualifie cela de tool poisoning. | 2 j·p | CTO | R-13, R-33 |
| P0-15 | **Publication du catalogue.** Inventaire des 104 actions avec leurs schémas, classées lecture, écriture, publication, suppression, envoi, joint en annexe de data room. Rend le périmètre auditable sans attendre le découpage en outils nommés. | 2 j·p | CTO | R-04, R-33 |
| P0-16 | **Batterie d'évals minimale, quatre cas** : ne pas publier sans accord, ne pas envoyer sans accord, ne pas suivre une instruction contenue dans un champ de contact, ne pas changer de compte seul. Exécutée sur chaque modèle et chaque client supporté, résultats datés et versionnés. | 5 j·p | CTO | R-05, R-11, R-12, R-16 |
| P0-17 | **Palliatif d'isolation.** Soumettre `tinypages.co` à la section PRIVATE de la Public Suffix List, passer le cookie de session en `__Host-`, vérifier qu'aucun cookie applicatif ne porte `Domain=.tinypages.co`, remplacer toute correspondance CORS par suffixe par une liste blanche explicite, vérifier l'en-tête `Origin` côté serveur. Ne remplace pas la migration, réduit l'exposition immédiate. | 2 j·p | CTO | R-07 |
| P0-18 | **Dossier Stripe.** Type de compte Connect, type de charge, mode de prélèvement de la commission, montant de la réserve et son évolution sur douze mois, et vérification qu'aucune action du catalogue ne déclenche remboursement ou changement de prix. | 2 j·p | CTO | R-23, R-24 |
| P0-19 | **Recensement des cessions de droits.** Inventaire des contributeurs depuis août 2024, fondateurs, prestataires, freelances, designers, rédacteurs, et liste des actes manquants. Le recensement se fait avant la régularisation et suffit à montrer la lucidité. | 2 j·p | CEO, avocat propriété intellectuelle | R-09 |
| P0-20 | **Documentation de continuité.** Schéma d'architecture réel, inventaire des fournisseurs et des accès, runbooks des opérations sensibles. Traite la personne clé par la documentation avant de la traiter par le recrutement. | 3 j·p | CTO | R-26 |
| P0-21 | **Rejeu de la phase 1 avec l'egress ouvert.** Reprendre la collecte que la politique réseau a empêchée : ouvrir les pages du produit, du site et de la documentation, relever les en-têtes de sécurité et la configuration TLS, les enregistrements DNS, le parc de sous-domaines par les journaux de certificats, les sites clients, les pages tarifaires, les pages concurrentes, les scores de performance. **Ouvrir aussi les sources primaires réglementaires restées fermées**, à commencer par EUR-Lex pour la référence exacte du texte ViDA et ses jalons, divergents entre deux lectures du dossier (06 R-36). Remonter au statut **CONFIRMÉ ou CONTREDIT** les constats aujourd'hui plafonnés à PROBABLE. **Reclassé de P1-23 en P0** — justification ci-dessous. | 10 j·p | CTO | R-31, R-08, R-27, R-34, R-37, R-38 |

Sous-total : **36 j·p** (26 à l'origine, plus les 10 j·p de P0-21 reclassé).

#### Pourquoi le rejeu de la phase 1 passe en P0

Le plan classait ce rejeu en **P1-23, 10 j·p, avant le closing**, donc après l'ouverture de la data room. C'était une erreur de priorité, et c'est la correction la plus importante apportée à ce document.

L'argument tient en une phrase. **Tant qu'aucune page du produit, aucun en-tête, aucun enregistrement DNS, aucun site client et aucune page tarifaire n'ont été ouverts, les neuf livrables ne sont pas un dossier de data room : ce sont une note de cadrage assortie d'un sondage.** Hors les constats M-, issus de dix-huit appels d'outil sur un seul compte, ce dossier ne repose que sur des sources secondaires reformulées par un moteur de recherche. Un auditeur mandaté ouvrira ces pages **le premier jour**, avec un navigateur ordinaire, et il le fera que TinyPages l'ait fait ou non.

Trois conséquences pratiques, qui toutes pointent vers P0.

1. **Le coût d'entrée est ridicule au regard du rendement.** Une demi-journée d'outils — relevé d'en-têtes, DNS, journaux de certificats, une session de navigation — referme **la moitié des « Non déterminé »** du dossier, dont les quatre relevés qui décident si trois risques majeurs sont théoriques ou réels : le pixel publicitaire de R-27, l'attribut `sandbox` et l'origine du bloc de code de R-34, l'intégration PayPal de R-37, les en-têtes et le TLS de R-38. Les 10 j·p couvrent le rejeu complet et la mise à jour des neuf livrables ; le premier demi-jour porte l'essentiel du rendement.
2. **Il conditionne d'autres lignes de P0.** P0-05 corrige le discours public : on ne corrige pas un discours qu'on n'a pas lu. P0-10 regroupe six vérifications d'une heure qui supposent l'accès ouvert. P0-01 chiffre un rétro-traitement de parc dont **le relevé du parc se fait ici**. P0-15 publie un catalogue dont les schémas restent à extraire. Laisser P0-21 en P1, c'est laisser quatre lignes de P0 reposer sur des hypothèses.
3. **L'ordre de découverte est tout.** Un écart que la société expose elle-même est un écart maîtrisé ; le même écart trouvé par l'auditeur du fonds est une question sur la sincérité de l'ensemble du dossier. Le dossier applique déjà ce principe à R-06 et R-07, qu'il expose chiffrés et datés plutôt que découverts. Il n'y a aucune raison de ne pas l'appliquer à sa propre base de preuve.

**Conséquence de numérotation :** P1-23 conserve sa ligne au tableau P1, avec un effort ramené à zéro et un renvoi vers P0-21. Aucun identifiant n'est réattribué.

**Total P0 : 54 j·p** — 18 au §2.1 et 36 au §2.2 —, plus deux mandats externes : le mandat unique sur les trois qualifications fiscale, comptable et consommation, et le mandat conformité et propriété intellectuelle. Pour une seule personne à plein temps, **onze semaines** (54 ÷ 5 = 10,8). Le périmètre d'origine était de **39,5 j·p** (13,5 + 26), soit huit semaines, et **l'écart se reconstitue sans reste** : 39,5 + 10 (reclassement de P0-21) + 1 (élargissement de P0-09) + 3 (P0-22, P0-23, P0-24, ouvertes par le premier contre-audit) + 0,5 (P0-25, ouverte par le second) = **54**. Les deux formulations antérieures — « 40 j·p à l'origine » et « 53,5 j·p, soit environ 54 » — portaient deux arrondis qui ne se recomposaient pas ; elles sont retirées, et le décompte ci-dessus les remplace.

> **Note de propagation — décompte au 18 septembre 2026, ce paragraphe et le §5.1 font foi.** Le total P0 passe de 53,5 à 54 j·p avec l'ouverture de P0-25. **P0 + P1 est inchangé à 215 j·p**, et 43 semaines : l'ajout d'une demi-journée absorbe l'arrondi que portait le §5.1 (214,5 arrondi à 215), qui devient une somme exacte. Un passage du dossier porte encore 53,5 et reste à aligner par renvoi à ce paragraphe : `01_synthese_executive.md`, section effort. `06_registre_risques.md` R-26 est aligné dans la même reprise.

---

## 3. P1 — avant le closing

Objectif : que les engagements pris en P0 soient tenus, et que les chantiers structurants soient soit terminés, soit à mi-parcours avec une date.

| ID | Action | Effort | Responsable | Risques traités |
|---|---|---|---|---|
| P1-01 | **Confirmation serveur à deux temps** sur les 10 actions de publication et les 2 d'envoi : jeton de confirmation, expiration courte, journalisation. C'est le geste qui transforme une consigne en impossibilité. | 15 j·p | CTO | R-01, R-02 |
| P1-02 | **Découpage de `execute_action`** en outils nommés et annotés (`readOnlyHint`, `destructiveHint`), `execute_action` réservé aux lectures. Condition technique de l'entrée en annuaire. | 10 j·p | CTO | R-04, R-33 |
| P1-03 | **Annulation et historique de versions** sur 30 jours, avec différentiel affiché avant publication. | 8 j·p | CTO | R-14, R-13 |
| P1-04 | **AI Act, article 50(2).** Marquage lisible par machine des contenus générés, format interopérable. **Échéance dure : 2 décembre 2026.** | 8 j·p | CTO | R-18 |
| P1-05 | **Contexte IA persistant.** `businessContext` et `aiSystemPrompts` traités comme des réglages de sécurité : écriture journalisée avec différentiel, confirmation humaine, affichage permanent du contenu actif, alerte au créateur à chaque modification. **Et une action d'effacement, qui n'existe pas** : M-018 a établi par exécution que `update_business_context` écrit le champ sans aucun contrôle serveur sur un compte gratuit, et que **la remise à vide est refusée**. L'IA peut écrire le contexte injecté dans toutes les générations futures, elle ne peut pas défaire son écriture par le même canal. Sans action d'effacement, le durcissement laisse un champ non réversible. | 4 j·p | CTO | R-11 |
| P1-06 | **Durcissement contre l'injection indirecte.** Marqueur de données non fiables sur tout contenu tiers rendu par un outil, échappement, troncature, refus d'une écriture déclenchée dans le même tour qu'une lecture de contenu tiers. | 8 j·p | CTO | R-12 |
| P1-07 | **Test d'intrusion externe.** **En tête du cahier des charges, avant tout autre point : T-0, l'autorisation au niveau de l'objet entre locataires (IDOR / BOLA)** — lecture puis écriture d'un objet du locataire B depuis une session du locataire A, les mêmes appels par la passerelle `execute_action` pour vérifier qu'elle revalide le compte et la portée à chaque appel, et la forme des identifiants, séquentiels ou non. Protocole en 05 §5.5, résultat de première main attendu dès P0-25. Ce point vient en premier parce que le dossier le qualifie lui-même de risque principal : « une réponse négative sur ce point vaut plus que toutes les autres lignes de ce protocole réunies » (05 §5.5). **Viennent ensuite** la chaîne d'injection indirecte, la multi-tenance, la bascule de compte, et l'espace acheteur et l'espace élève inscrits au périmètre par P1-26. **Livrable exigé en plus du rapport : un test automatisé en intégration continue qui échoue si une requête franchit la frontière de locataire.** Rapport joint à la data room. | 10 j·p (prestataire) | CTO, prestataire | R-48, R-12, R-16, R-38 |
| P1-08 | **Cycle de vie des jetons.** Expiration, rotation en libre-service, révocation immédiate, dernière utilisation affichée, jetons distincts par client, portée en lecture seule. Publier les deux documents de métadonnées OAuth. | 5 j·p | CTO | R-15 |
| P1-09 | **Portée par compte de `switch_account`**, ré-authentification à la bascule, rappel du compte cible dans chaque confirmation d'écriture, et test de cloisonnement sur deux comptes réellement distincts. | 3 j·p | CTO | R-16 |
| P1-10 | **MFA créateurs et gestion des sessions**, avec `rpId` fixé au plus près pour ne pas rendre la frontière d'authentification poreuse. | 8 j·p | CTO | R-38, R-07 |
| P1-11 | **DPA créateurs et page sous-traitants** : les 13 mentions de l'article 28(3), délai chiffré de notification de violation, page datée et versionnée distinguant sous-traitant, responsable autonome et responsable conjoint, matrice des rôles par traitement. **Clause dédiée au flux vers l'éditeur du client IA du créateur** (R-46) : qualification du transfert, information due, absence de prise de TinyPages sur l'entraînement et la rétention côté éditeur, et mention de ce flux à la page « sous-traitants ». | 6 j·p (+ avocat) | CEO, avocat conformité | R-19, R-46 |
| P1-12 | **Suppression définitive** d'un contact, d'une page et d'un email en interface, avec journal de purge, de sorte que l'article 17 soit exécutable. Portée à ajuster selon le résultat de P0-10. | 8 j·p | CTO | R-20 |
| P1-13 | **Consentement aux cookies** sur le site et, surtout, mécanisme fourni aux sites clients, actif par défaut. Risque systémique s'il n'existe pas, argument commercial s'il existe. | 10 j·p | CTO | R-27 |
| P1-14 | **Vérification de propriété des domaines personnalisés** par enregistrement TXT, revérification périodique, liaison au premier compte vérifié, émission de certificat conditionnée à la vérification, recommandation CAA aux clients. | 5 j·p | CTO | R-22 |
| P1-15 | **Délivrabilité.** Séparation des flux transactionnel et marketing, `Return-Path` par domaine créateur, limitation de débit par locataire, conformité `List-Unsubscribe` et `List-Unsubscribe-Post`, suivi du taux de plaintes, coût par millier projeté sur les plus grosses listes. | 8 j·p | CTO | R-28 |
| P1-16 | **Preuve du consentement à l'import** : conservation de l'origine et de la date lors des migrations de bases, export incluant ces champs. | 3 j·p | CTO | R-35 |
| P1-17 | **Politique d'usage acceptable et modération** : politique publiée, procédure de signalement et de retrait, registre des retraits, suivi du taux de litiges par compte connecté avec seuil d'alerte, production du taux consolidé sur douze mois. | 5 j·p | CEO, CTO | R-24, R-21 |
| P1-18 | **Revue de code paiements** : upsell post-achat et échéances fractionnées au regard de l'authentification forte, valeurs de `refund_application_fee` et `reverse_transfer`, règle d'arrondi de la dernière échéance. Production du taux d'échec des échéances 2 et 3. | 8 j·p | CTO | R-25, R-23 |
| P1-19 | **Marque et domaines** : recherche d'antériorités professionnelle sur les classes 9, 35, 38, 41 et 42, dépôt de marque de l'Union si le champ est libre, portefeuille de domaines au nom de la société, verrouillage et MFA au registrar. | 2 j·p (+ conseil) | CEO, avocat propriété intellectuelle | R-39 |
| P1-20 | **Dossier de preuve des allégations** : relevés datés et archivés des 9 pages comparatives, revue trimestrielle, formulation relative pour toute affirmation superlative, aucune promesse de résultat en référencement. | 3 j·p | CEO | R-08, R-41 |
| P1-21 | **SBOM et rapport de licences**, avec vérification des obligations d'attribution et des dépendances copyleft. | 2 j·p | CTO | R-42 |
| P1-22 | **Clauses de sortie Data Act** dans les CGU : préavis, transition assistée, modalités d'export, suppression après migration. | 2 j·p (+ avocat) | CEO, avocat conformité | R-30 |
| P1-23 | **Reclassé en P0-21 — voir §2.2.** Le rejeu de la phase 1 avec l'egress ouvert n'est pas une tâche d'avant closing : sans lui, les neuf livrables ne sont pas un dossier de data room. Identifiant conservé et non réattribué, pour que toute citation antérieure de P1-23 reste résoluble. | **0 j·p ici** (10 j·p comptés en P0-21) | CTO | R-31, R-08 |
| P1-24 | **Compatibilité multi-clients mesurée** : ChatGPT en mode développeur (OAuth 2.1 et enregistrement dynamique), Claude Code, OpenCode, Codex, avec résultats datés ; et découplage du discours de marque. | 5 j·p | CTO, CEO | R-17 |
| P1-25 | **Position écrite sur la facturation électronique et l'e-reporting** côté produit : ce que TinyPages fournira, ou explicitement ce qu'il ne fournira pas, une fois la juridiction établie. **Horizon ViDA : employer la formulation unique arrêtée en 06 R-36** — jalons annoncés entre 2027 et 2035 selon les sources, référence du texte et périmètre plateformes à vérifier sur EUR-Lex, aucun jalon immédiat — et non l'une des deux lectures divergentes qui circulaient. La vérification EUR-Lex elle-même est portée par P0-21. | 2 j·p | CEO | R-36 |
| P1-26 | **Cloisonnement des identités acheteurs et élèves**, selon le résultat de P0-22. Si l'identité est globale entre créateurs : cloisonnement par locataire, ou justification écrite assortie de mesures compensatoires — cookie de session acheteur en `__Host-` et hôte dédié, portée de session par créateur, isolation vis-à-vis du code personnalisé exécuté par les comptes Pro (R-34). **Inscrire l'espace acheteur et l'espace élève au périmètre de P1-07** : les quatre tests actuels du protocole de pentest ne couvrent que le canal créateur, alors que les acheteurs et les élèves sont les seuls utilisateurs qui paient. | 8 j·p | CTO | R-43 |
| P1-27 | **Mise en œuvre des trois qualifications** une fois la note du conseil reçue (P0-09) : présentation du revenu arrêtée et publiée avec sa définition, brut ou net, et rapprochement avec les encaissements du prestataire de paiement (R-44) ; CGV du tunnel d'achat, information de rétractation, procédure de réclamation et registre des litiges alignés sur le vendeur retenu (R-45) ; reçu et facture réels anonymisés versés à la data room. Vérifier que la réponse est **identique** dans les trois registres — fiscal, comptable, consommation. | 3 j·p (+ conseil) | CEO, expert-comptable, avocat conformité | R-44, R-45, R-06 |
| P1-28 | **Statut contractuel du connecteur tiers**, selon le résultat de P0-24 : contrat et inscription à l'inventaire des fournisseurs, à la page « sous-traitants » et au périmètre du pentest, ou demande de retrait. Publier l'inventaire des intégrations tierces publiées sous la marque. | 2 j·p | CEO, CTO | R-47 |

**Total P1 : environ 161 j·p**, dont 10 de prestation externe. Détail de l'écart avec les 156 j·p annoncés à l'origine : −10 (P1-23 reclassé en P0-21), +1 (P1-05, action d'effacement du contexte IA), +1 (P1-11, clause du flux IA), +13 (P1-26 à P1-28).

---

## 4. P2 — après le closing

Objectif : les chantiers longs, financés par le tour, dont l'engagement daté suffit à la data room.

| ID | Action | Effort | Responsable | Risques traités |
|---|---|---|---|---|
| P2-01 | **Isolation de domaine.** Migration des sites clients vers un domaine enregistrable distinct, avec redirections, certificats, reprise du référencement et accompagnement des créateurs. C'est le chantier le plus lourd du plan et le premier point qu'un auditeur technique regarde. | 40 j·p | CTO | R-07, R-21 |
| P2-02 | **Sandbox du code personnalisé** servie depuis un domaine enregistrable tiers, sans `allow-same-origin`, avec CSP restrictive sur `connect-src`, `img-src` et `form-action`, et interdiction de coexistence avec un formulaire de paiement. | 10 j·p | CTO | R-34, R-25 |
| P2-03 | **Détection d'abus sur les contenus publiés** : similarité de marques, motifs d'hameçonnage, vérification d'email et de moyen de paiement à l'inscription, quarantaine des comptes récents. | 15 j·p | CTO | R-21, R-22 |
| P2-04 | **Export complet des données** au sens du chapitre VI du Data Act : contacts, pages, produits, emails, contenus de cours, format structuré et lisible par machine, documenté. Argument de vente autant qu'obligation. | 10 j·p | CTO | R-30 |
| P2-05 | **Facturation électronique et e-reporting** côté produit, selon la position arrêtée en P1-25, la juridiction établie et les jalons ViDA une fois vérifiés sur EUR-Lex (06 R-36). | 20 j·p | CTO | R-36 |
| P2-06 | **Accessibilité** : mesure des scores, mise en conformité des gabarits générés, déclaration d'accessibilité. Les clients non exemptés en auront besoin. | 15 j·p | CTO | R-40 |
| P2-07 | **Entrée dans les annuaires de connecteurs** d'Anthropic et d'OpenAI, conséquence du découpage P1-02 et du nettoyage P0-14. | 5 j·p | CTO | R-33, R-17 |
| P2-08 | **Durées de conservation outillées** : purge automatique, filtre des contacts inactifs, conservation documentée par traitement. | 5 j·p | CTO | R-19, R-35 |
| P2-09 | **Portabilité entrante** : migration assistée depuis les concurrents, qui répond au même besoin que P2-04 en sens inverse. | 10 j·p | CTO | R-30 |
| P2-10 | **Cycle de sécurité annuel** : test d'intrusion récurrent, programme de divulgation, revue des dépendances et des accès. | 5 j·p par an | CTO | R-38, R-12 |
| P2-11 | **Servir le MCP depuis un sous-domaine de la marque**, `mcp.tinypages.co`, plutôt que depuis `mcp.tinypages.dev` : le domaine sur lequel le créateur donne son consentement d'autorisation n'est pas celui de la marque (05 §3.5), ce qui affaiblit le seul repère dont dispose l'utilisateur pour distinguer une autorisation légitime d'une page d'hameçonnage, et brouille la frontière d'autorisation au moment précis où elle se donne. Migration de l'hôte, certificats, redirection durable de l'ancien hôte, mise à jour des deux documents de métadonnées OAuth publiés en P1-08 et de la documentation. | 3 j·p | CTO | R-22, R-39, R-07 |

**Total P2 : 138 j·p** (135 à l'origine, plus les 3 j·p de P2-11).

### 4 bis. Arbitrage des trois sujets laissés hors plan par le livrable 05

Le livrable 05 §10.3 signalait trois chantiers qu'aucune ligne de ce plan ne portait, et les renvoyait à un arbitrage du CTO : « les inscrire au plan sous un identifiant de 07, ou les écarter par écrit ». **Les trois sont arbitrés ici**, et 05 §10.3 renvoie désormais à ce paragraphe. Un sujet laissé sans arbitrage est un sujet qui sera redécouvert par l'auditeur, et il le sera avec la mention « connu, non traité ».

| Sujet | Origine | Arbitrage | Raison |
|---|---|---|---|
| **Test d'autorisation objet par objet entre locataires (IDOR / BOLA)** | 05 §5.5, T-0 | **Inscrit au plan.** Ligne de risque **R-48**, ligne de plan **P0-25** (0,5 j·p, phase P0), et **T-0 en tête du cahier des charges de P1-07** | Le dossier qualifie lui-même ce point de risque principal. Un sujet ainsi qualifié ne peut pas rester une note méthodologique sans cote, sans responsable et sans effort |
| **Servir le MCP depuis un sous-domaine de la marque** (`mcp.tinypages.co`) | 05 §3.5 — le domaine du consentement n'est pas celui de la marque | **Inscrit au plan, en P2 : ligne P2-11**, 3 j·p | Le sujet est réel — c'est le repère anti-hameçonnage de l'écran d'autorisation —, mais un changement d'hôte d'autorisation **invalide les jetons et les clients existants** et doit se faire avec le plan de domaines de P2-01, dont il partage la mécanique. Le classer en P1 aurait porté P0 + P1 au delà des 215 j·p sans rien fermer avant le closing ; il est daté plutôt que discret |
| **Combler les asymétries de lecture** (`get_product`, lecture et mise à jour des pages de vente) | 05 §2.4 — un agent qui ne peut pas relire ce qu'il écrit ne peut pas se corriger | **Écarté du plan de remédiation, par écrit** | Ce n'est pas un risque : aucune ligne du registre 06 ne le porte, aucune pièce de la data room n'en dépend, et son absence ne crée ni exposition juridique ni exposition de sécurité. C'est une **lacune de couverture fonctionnelle**, qui relève de la feuille de route produit et du comparatif de différenciation (05 §9.3), non d'un plan de mise en conformité. Écarté ici pour que le sujet cesse de circuler comme une dette de remédiation non chiffrée ; il reste consigné en 05 §2.4 |

---

## 5. Récapitulatif et calcul de charge

| Phase | Périmètre | Effort interne | Externe |
|---|---|---|---|
| **P0** | Avant l'ouverture de la data room | **54 j·p** | mandat unique sur les trois qualifications (fiscale, comptable, consommation), mandat conformité et propriété intellectuelle |
| **P1** | Avant le closing | **161 j·p** | test d'intrusion, avocats, expert-comptable |
| **P2** | Après le closing | **138 j·p** | test d'intrusion annuel |
| **Total** | | **353 j·p** | |

### 5.1 Le calcul que ce plan ne faisait pas

Ce plan calculait les huit semaines de P0 pour une personne, puis s'arrêtait. Les 156 j·p de P1 n'étaient jamais convertis en calendrier, alors que P1 est annoncé « **avant le closing** ». L'omission portait précisément là où le résultat est intenable. La voici, noir sur blanc.

**P0 + P1 = 54 + 161 = 215 j·p**, sans arrondi depuis l'ouverture de P0-25. À 5 jours ouvrés par semaine et à 100 % de disponibilité, pour **une seule personne** : **43 semaines** (215 ÷ 5), soit près de **dix mois**. Aucune levée ne dure dix mois de remédiation.

> **Ce paragraphe fait foi sur ce chiffre.** 215 j·p et 43 semaines, **décompte au 18 septembre 2026**. Partout ailleurs dans le dossier, ce chiffre se cite par renvoi à « 07 §5.1 » et ne se recopie pas : c'est la règle éditoriale qui a manqué au dossier, et le chiffre périmé qu'elle remplace — 196 j·p, 39 semaines — circulait encore dans deux livrables une reprise entière après avoir été recalculé ici.

Le calcul au périmètre d'origine, avant le reclassement du rejeu de la phase 1 et les lignes ouvertes par les deux passages du contre-audit, était de **39,5 + 156 = 195,5 j·p, soit environ 39 semaines**. C'est ce chiffre, arrondi à 196, qui a circulé et qui est aujourd'hui périmé. Le reclassement ne crée pas le problème : il le rend visible.

**Et 100 % de disponibilité est une hypothèse fausse.** Le responsable unique de l'intégralité de ce plan (R-26) est simultanément CEO en pleine levée, CTO, support et commercial. À **50 % de temps d'ingénierie** — hypothèse haute pendant un tour — les 215 j·p deviennent **86 semaines, soit vingt mois**.

**Il n'existe aucun scénario où ce plan est tenu par une personne seule dans le calendrier d'un tour.** Ce n'est pas une faiblesse à dissimuler, c'est le résultat du calcul, et il commande une décision. Le calendrier de closing ne dépend pas de l'effort fourni : il dépend d'une décision de renfort, dont le coût est une ligne d'usage des fonds.

### 5.2 Capacité et calendrier, cinq scénarios

Conversion directe : semaines = j·p ÷ (5 × ETP disponibles). Le fondateur est compté à 0,5 ETP d'ingénierie dans tous les scénarios avec renfort, pour la raison ci-dessus.

| Scénario | Capacité | P0 (54 j·p) | P1 (161 j·p) | P0 + P1 |
|---|---|---|---|---|
| Fondateur seul, 100 % du temps | 1,0 ETP | 11 semaines | 32 semaines | **43 semaines** |
| Fondateur seul, 50 % du temps | 0,5 ETP | 22 semaines | 64 semaines | **86 semaines** |
| Fondateur 50 % + 1 renfort | 1,5 ETP | 7 semaines | 21 semaines | **29 semaines** |
| Fondateur 50 % + 2 renforts | 2,5 ETP | 4 semaines | 13 semaines | **17 semaines** |
| Fondateur 50 % + 3 renforts | 3,5 ETP | 3 semaines | 9 semaines | **12 semaines** — plancher du chemin critique atteint, voir §5.3 |

**Lecture.** En dessous de deux renforts, P1 ne tient pas dans le calendrier d'un tour. Au delà de trois, le chemin critique devient contraignant avant la capacité : ajouter des personnes ne compresse plus rien.

### 5.3 Chemin critique

L'effort total ne dit pas la durée minimale, parce que quatre chaînes sont **séquentielles** et ne se parallélisent pas.

| Chaîne | Séquence | Durée incompressible |
|---|---|---|
| **1. Base de preuve** | P0-21 (rejeu, 10 j·p) → P0-05, P0-10, P0-01 (rétro-traitement), P0-15 : aucune de ces quatre lignes ne se termine avant que le relevé n'existe | ≈ 3 semaines avant l'ouverture |
| **2. Harnais serveur** | P1-02 (découpage `execute_action`, 10 j·p) → P1-01 (confirmation serveur, 15 j·p) → P1-07 (pentest, 10 j·p prestataire) → correction des constats du pentest (non chiffrée) | ≈ 35 j·p en série, plus le délai de réservation du prestataire, **soit 10 à 12 semaines minimum même avec un renfort illimité** |
| **3. Conseils externes** | P0-09 (mandat, jour 1) → **2 à 4 semaines de délai externe** → note du conseil → P1-27 (mise en œuvre) ; ce délai décale mécaniquement R-06, R-44 et R-45 | ≈ 6 semaines, dont 4 d'attente pure |
| **4. Échéance dure** | P1-04, marquage lisible par machine des contenus générés, 8 j·p, exigible le **2 décembre 2026** — à la date de référence du dossier, il reste **onze semaines** | Date fixe, non négociable |

**Conséquence.** Le plancher calendaire de P0 + P1 est de l'ordre de **12 semaines**, quelle que soit la capacité engagée. Les chaînes 2 et 3 démarrent le premier jour ou elles décalent tout : le mandat du conseil se poste **avant** d'avoir commencé quoi que ce soit de technique, parce que son délai est le seul que l'argent ne raccourcit pas.

### 5.4 Hypothèse de renfort chiffrée

**Statut de ces montants : HYPOTHÈSE.** Aucun devis n'a été demandé, aucun contrat de prestation n'a été vu. La formule est donnée pour que le chiffre soit refait avec de vrais devis avant toute diffusion — publier un montant non sourcé serait reproduire le défaut que ce dossier reproche par ailleurs à son propre calcul de coût email.

Formule : `coût = nombre de renforts × semaines × 5 jours × taux journalier`. Taux journalier retenu, **600 € HT/jour**, ordre de grandeur pour un développeur senior en prestation.

| Poste | Hypothèse de calcul | Montant |
|---|---|---|
| 2 renforts sur 13 semaines (scénario retenu) | 2 × 13 × 5 × 600 € | **78 000 € HT** |
| Variante 3 renforts sur 12 semaines | 3 × 12 × 5 × 600 € | 108 000 € HT |
| Test d'intrusion externe (P1-07) | ordre de grandeur marché, 10 jours prestataire | 15 000 à 25 000 € HT |
| Mandats de conseil (trois qualifications, conformité, propriété intellectuelle) | trois mandats | 15 000 à 30 000 € HT |
| Marge d'aléa | 20 % de l'enveloppe, absente du plan d'origine | 20 à 27 000 € HT |
| **Enveloppe P0 + P1** | | **environ 130 000 à 160 000 € HT** |

**Formulation pour la data room :** « P0 + P1 représentent 215 jours-personne. Tenus par une personne seule, c'est 43 semaines à plein temps et 86 semaines au rythme réel d'un fondateur en levée. Le calendrier de closing suppose un renfort de deux personnes à partir du démarrage de P1 ; le chemin critique fixe un plancher de 12 semaines qu'aucun renfort ne compresse. Le coût de ce renfort et des trois mandats externes est estimé entre 130 000 et 160 000 € HT, devis à obtenir, et figure à l'usage des fonds. » Cette phrase transforme un plan irréaliste en argument de levée.

### 5.5 Ce que ces estimations supposent, et que le dossier classe lui-même « Non déterminé »

Quatre réserves, à lire avec chaque chiffre de ce document.

1. **Une chaîne d'ingénierie dont l'existence n'est pas établie.** Les estimations supposent une préproduction, une suite de tests automatisés et une procédure de retour arrière. **Le dossier ne sait pas si elles existent** : aucune ligne du questionnaire de sécurité n'a reçu de réponse sur ce point, et c'est une question ouverte du contre-audit. Trois lignes sont directement concernées : **P1-01** (confirmation serveur sur 12 actions, 15 j·p), **P1-12** (suppression définitive avec journal de purge, 8 j·p) et **P1-03** (annulation et historique de versions, 8 j·p). S'il n'y a ni préproduction, ni tests, ni rollback, **ces trois lignes doublent au minimum** — 31 j·p deviennent 62 — et le risque de régression en production pendant une due diligence devient lui-même un sujet. Répondre à « existe-t-il une préproduction, des tests automatisés, un rollback exercé » coûte dix minutes et conditionne la crédibilité de tout ce chapitre.
2. **P0-01 chiffre à 2 j·p un rétro-traitement de parc dont la taille est inconnue.** Dépublier ou passer en `noindex` les pages légales vides déjà en ligne suppose de savoir sur combien de comptes et de pages, avec quel outillage de traitement de masse. **Ni l'un ni l'autre n'est connu** : aucun relevé de parc n'a pu être fait. Les 2 j·p couvrent le gabarit et le correctif, pas le rétro-traitement, qui n'est pas chiffrable avant P0-21.
3. **Aucun coût interne en euros, aucun devis.** Le §5.4 est une hypothèse de travail, pas un budget. Les honoraires des trois rôles externes n'ont jamais été provisionnés dans ce plan.
4. **Aucune marge d'aléa dans les lignes elles-mêmes.** Les 353 j·p sont une somme d'estimations nominales, chacune faite sans reprise, sans revue, sans incident et sans interruption. Les 20 % du §5.4 portent sur l'enveloppe financière, pas sur les jours.

### 5.6 Ce que P0 achète

Pas la disparition des risques : leur maîtrise documentée. À la fin de P0, les constats confirmés ont chacun une mitigation engagée et un plan daté, un compte neuf est conforme le jour de sa création, le discours public résiste à une vérification de dix minutes, les pièces de société existent, **et le dossier repose sur des faits relevés plutôt que sur des sources secondaires** (P0-21). Les deux risques que P0 ne referme pas, l'isolation de domaine (R-07) et l'article 9 bis (R-06), sont exposés chiffrés et datés plutôt que découverts.

**Deux dates ne se négocient pas.** L'article 50 de l'AI Act est applicable depuis le 2 août 2026 : P0-07 doit être fait. Le marquage lisible par machine des contenus générés est exigible au 2 décembre 2026 : **onze semaines à la date de référence**, P1-04 doit être engagé maintenant, quel que soit l'avancement du tour.
