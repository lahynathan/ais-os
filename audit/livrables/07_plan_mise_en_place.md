# 07 — Plan de mise en place

Data room TinyPages · Date de référence : 18 septembre 2026 · Rédigé par l'orchestrateur d'audit (A00). Traite les lignes R-01 à R-42 de `06_registre_risques.md`.

---

## 0. Conventions

**Phases.** Le tour n'est pas arrêté, aucune date de closing n'est citée.
- **P0** : avant l'ouverture de la data room.
- **P1** : avant le closing.
- **P2** : après le closing.

Deux dates calendaires seulement figurent dans ce plan, parce qu'elles sont réglementaires et dures : l'article 50 de l'AI Act est applicable depuis le **2 août 2026**, et le marquage lisible par machine des contenus générés est exigible au **2 décembre 2026**.

**Effort.** En jours-personne (j·p), travail interne. Les honoraires des trois rôles externes à mandater (avocat fiscaliste, avocat conformité et propriété intellectuelle, prestataire de test d'intrusion) ne sont pas convertis en j·p et sont signalés à part.

**Responsables.** CTO et CEO sont la même personne, Nathan Lahy. Le plan les distingue par nature de décision, technique ou société. Cette concentration est le risque R-26, et elle a une conséquence directe sur ce plan : les 40 j·p de P0 représentent environ huit semaines pleines pour une seule personne. C'est le premier argument du plan de renfort, et il vaut mieux l'écrire que le laisser calculer.

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

Objectif : que l'auditeur mandaté ne trouve rien que le dossier ne dise déjà, et que les quatre constats confirmés soient accompagnés d'une mitigation engagée.

### 2.1 Ce qui se corrige en une journée ou deux

| ID | Action | Effort | Responsable | Risques traités |
|---|---|---|---|---|
| P0-01 | **Gabarit des pages légales.** Rédiger un contenu réel de politique de confidentialité et de conditions d'utilisation, conditionner la publication à un contenu non vide, forcer `indexed: false` tant qu'elles ne sont pas remplies, et rétro-traiter le parc existant par dépublication ou `noindex`. | 2 j·p (+ relecture avocat) | CTO, avocat conformité | R-03 |
| P0-02 | **Consentement par défaut.** Activer le double opt-in par défaut, stocker date, source et texte du consentement avec chaque contact, revoir le bloc de capture de la page d'accueil créée par défaut. | 2 j·p | CTO | R-03, R-28, R-35 |
| P0-03 | **Sous-domaines.** Liste de mots réservés (app, admin, api, login, auth, secure, pay, checkout, billing, account, support, status, mail, cdn, docs, mcp, staging), refus de tout identifiant contenant « tinypages » ou une variante, quarantaine d'au moins douze mois des identifiants libérés avec page « ce site n'existe plus ». | 1 j·p | CTO | R-22 |
| P0-04 | **Paquet de signalement.** `security.txt` conforme à la RFC 9116 sur le site et l'application, page publique de signalement d'abus, alias `abuse@`, point de contact autorités et point de contact destinataires au sens des articles 11 et 12 du DSA. | 1 j·p | CTO, CEO | R-29, R-38 |
| P0-05 | **Correction du discours.** Retirer ou relativiser « seule plateforme », trancher C-001, C-002 et C-003 par une phrase unique publiée au même endroit dans la FAQ et dans la documentation, avec la liste des actions réservées au plan Pro. Corriger `docs.tinypages.co/getting-started/1-4-mcp-setup`, dont l'affirmation sur l'envoi d'emails est fausse. | 1 j·p | CEO | R-08, R-32, R-02 |
| P0-06 | **Note de position sur les garde-fous.** Une page pour la data room, reprenant la formulation défendable du rapport A04 §2 bis : comportement par défaut du modèle, non contrôle d'accès. Toute autre formulation sera démentie en un appel. | 0,5 j·p | CEO, CTO | R-01, R-02 |
| P0-07 | **AI Act, article 50(1).** Mention visible partout où un utilisateur final dialogue avec l'IA, et décision écrite sur le périmètre. L'article est applicable depuis le 2 août 2026. | 1 j·p | CTO | R-18 |
| P0-08 | **Pièces d'entité.** Extrait d'immatriculation, statuts, table de capitalisation, mentions légales, titulaire de chaque nom de domaine, `.co`, `.dev` et `.com`. | 1 j·p | CEO | R-10, R-39 |
| P0-09 | **Mandat fiscal.** Mandater l'avocat fiscaliste sur l'article 9 bis, en établissant au préalable les trois faits qui commandent la réponse : qui figure sur la facture à l'acheteur final, qui fixe les CGV du tunnel d'achat, qui commande la mise à disposition du fichier. | 1 j·p (+ honoraires) | CEO | R-06 |
| P0-10 | **Six vérifications factuelles.** Chacune prend moins d'une heure et referme une question ouverte : suppression définitive d'un contact en interface (R-20), envoi d'un message sur un compte Pro (R-02), relevé des requêtes réseau de la page d'accueil avec un navigateur vierge, capture à l'appui (R-27), relevé des en-têtes de sécurité et de la configuration TLS (R-38), type d'intégration PayPal et prélèvement effectif de la commission (R-37), attribut `sandbox` complet et origine du bloc de code (R-34). | 2 j·p | CTO | R-02, R-20, R-27, R-34, R-37, R-38 |
| P0-11 | **Hygiène d'audit.** Supprimer les deux brouillons résiduels des tests, ouvrir un compte de test réellement dédié pour toute vérification ultérieure, et joindre à la data room la note de base de preuve, y compris l'écart sur le compte MCP. | 1 j·p | CTO | R-31 |

Sous-total : **13,5 j·p**.

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

Sous-total : **26 j·p**.

**Total P0 : 39,5 j·p**, soit environ **40 jours-personne**, plus deux mandats externes (fiscaliste, conformité et propriété intellectuelle). Pour une seule personne, environ huit semaines pleines.

---

## 3. P1 — avant le closing

Objectif : que les engagements pris en P0 soient tenus, et que les chantiers structurants soient soit terminés, soit à mi-parcours avec une date.

| ID | Action | Effort | Responsable | Risques traités |
|---|---|---|---|---|
| P1-01 | **Confirmation serveur à deux temps** sur les 10 actions de publication et les 2 d'envoi : jeton de confirmation, expiration courte, journalisation. C'est le geste qui transforme une consigne en impossibilité. | 15 j·p | CTO | R-01, R-02 |
| P1-02 | **Découpage de `execute_action`** en outils nommés et annotés (`readOnlyHint`, `destructiveHint`), `execute_action` réservé aux lectures. Condition technique de l'entrée en annuaire. | 10 j·p | CTO | R-04, R-33 |
| P1-03 | **Annulation et historique de versions** sur 30 jours, avec différentiel affiché avant publication. | 8 j·p | CTO | R-14, R-13 |
| P1-04 | **AI Act, article 50(2).** Marquage lisible par machine des contenus générés, format interopérable. **Échéance dure : 2 décembre 2026.** | 8 j·p | CTO | R-18 |
| P1-05 | **Contexte IA persistant.** `businessContext` et `aiSystemPrompts` traités comme des réglages de sécurité : écriture journalisée avec différentiel, confirmation humaine, affichage permanent du contenu actif, alerte au créateur à chaque modification. | 3 j·p | CTO | R-11 |
| P1-06 | **Durcissement contre l'injection indirecte.** Marqueur de données non fiables sur tout contenu tiers rendu par un outil, échappement, troncature, refus d'une écriture déclenchée dans le même tour qu'une lecture de contenu tiers. | 8 j·p | CTO | R-12 |
| P1-07 | **Test d'intrusion externe** portant en priorité sur la chaîne d'injection indirecte, la multi-tenance et la bascule de compte. Rapport joint à la data room. | 10 j·p (prestataire) | CTO, prestataire | R-12, R-16, R-38 |
| P1-08 | **Cycle de vie des jetons.** Expiration, rotation en libre-service, révocation immédiate, dernière utilisation affichée, jetons distincts par client, portée en lecture seule. Publier les deux documents de métadonnées OAuth. | 5 j·p | CTO | R-15 |
| P1-09 | **Portée par compte de `switch_account`**, ré-authentification à la bascule, rappel du compte cible dans chaque confirmation d'écriture, et test de cloisonnement sur deux comptes réellement distincts. | 3 j·p | CTO | R-16 |
| P1-10 | **MFA créateurs et gestion des sessions**, avec `rpId` fixé au plus près pour ne pas rendre la frontière d'authentification poreuse. | 8 j·p | CTO | R-38, R-07 |
| P1-11 | **DPA créateurs et page sous-traitants** : les 13 mentions de l'article 28(3), délai chiffré de notification de violation, page datée et versionnée distinguant sous-traitant, responsable autonome et responsable conjoint, matrice des rôles par traitement. | 5 j·p (+ avocat) | CEO, avocat conformité | R-19 |
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
| P1-23 | **Rejeu de la phase 1 avec egress ouvert** : remonter au statut CONFIRMÉ ou CONTREDIT les constats aujourd'hui plafonnés à PROBABLE, relever le parc de sous-domaines, mesurer les scores et vérifier les affirmations concurrentes. Le faire avant que l'auditeur du fonds ne le fasse à la place de TinyPages. | 10 j·p | CTO | R-31, R-08 |
| P1-24 | **Compatibilité multi-clients mesurée** : ChatGPT en mode développeur (OAuth 2.1 et enregistrement dynamique), Claude Code, OpenCode, Codex, avec résultats datés ; et découplage du discours de marque. | 5 j·p | CTO, CEO | R-17 |
| P1-25 | **Position écrite sur la facturation électronique et l'e-reporting** côté produit : ce que TinyPages fournira, ou explicitement ce qu'il ne fournira pas, une fois la juridiction établie. | 2 j·p | CEO | R-36 |

**Total P1 : environ 156 j·p**, dont 10 de prestation externe.

---

## 4. P2 — après le closing

Objectif : les chantiers longs, financés par le tour, dont l'engagement daté suffit à la data room.

| ID | Action | Effort | Responsable | Risques traités |
|---|---|---|---|---|
| P2-01 | **Isolation de domaine.** Migration des sites clients vers un domaine enregistrable distinct, avec redirections, certificats, reprise du référencement et accompagnement des créateurs. C'est le chantier le plus lourd du plan et le premier point qu'un auditeur technique regarde. | 40 j·p | CTO | R-07, R-21 |
| P2-02 | **Sandbox du code personnalisé** servie depuis un domaine enregistrable tiers, sans `allow-same-origin`, avec CSP restrictive sur `connect-src`, `img-src` et `form-action`, et interdiction de coexistence avec un formulaire de paiement. | 10 j·p | CTO | R-34, R-25 |
| P2-03 | **Détection d'abus sur les contenus publiés** : similarité de marques, motifs d'hameçonnage, vérification d'email et de moyen de paiement à l'inscription, quarantaine des comptes récents. | 15 j·p | CTO | R-21, R-22 |
| P2-04 | **Export complet des données** au sens du chapitre VI du Data Act : contacts, pages, produits, emails, contenus de cours, format structuré et lisible par machine, documenté. Argument de vente autant qu'obligation. | 10 j·p | CTO | R-30 |
| P2-05 | **Facturation électronique et e-reporting** côté produit, selon la position arrêtée en P1-25 et la juridiction établie. | 20 j·p | CTO | R-36 |
| P2-06 | **Accessibilité** : mesure des scores, mise en conformité des gabarits générés, déclaration d'accessibilité. Les clients non exemptés en auront besoin. | 15 j·p | CTO | R-40 |
| P2-07 | **Entrée dans les annuaires de connecteurs** d'Anthropic et d'OpenAI, conséquence du découpage P1-02 et du nettoyage P0-14. | 5 j·p | CTO | R-33, R-17 |
| P2-08 | **Durées de conservation outillées** : purge automatique, filtre des contacts inactifs, conservation documentée par traitement. | 5 j·p | CTO | R-19, R-35 |
| P2-09 | **Portabilité entrante** : migration assistée depuis les concurrents, qui répond au même besoin que P2-04 en sens inverse. | 10 j·p | CTO | R-30 |
| P2-10 | **Cycle de sécurité annuel** : test d'intrusion récurrent, programme de divulgation, revue des dépendances et des accès. | 5 j·p par an | CTO | R-38, R-12 |

**Total P2 : environ 135 j·p.**

---

## 5. Récapitulatif

| Phase | Périmètre | Effort interne | Externe |
|---|---|---|---|
| **P0** | Avant l'ouverture de la data room | **40 j·p** | 2 mandats d'avocat |
| **P1** | Avant le closing | **156 j·p** | test d'intrusion, avocats |
| **P2** | Après le closing | **135 j·p** | test d'intrusion annuel |
| **Total** | | **environ 330 j·p** | |

**Ce que P0 achète.** Pas la disparition des risques : leur maîtrise documentée. À la fin de P0, les quatre constats confirmés ont chacun une mitigation engagée et un plan daté, un compte neuf est conforme le jour de sa création, le discours public résiste à une vérification, et les pièces de société existent. Les deux risques que P0 ne referme pas, l'isolation de domaine et l'article 9 bis, sont exposés chiffrés et datés plutôt que découverts.

**Le calendrier réel dépend d'une décision, pas d'un effort.** 40 jours-personne tenus par une seule personne font huit semaines ; répartis sur deux personnes, quatre. La ligne R-26 n'est donc pas une ligne de forme : c'est elle qui fixe la date d'ouverture de la data room.

**Deux dates ne se négocient pas.** L'article 50 de l'AI Act est applicable depuis le 2 août 2026 : P0-07 doit être fait. Le marquage lisible par machine des contenus générés est exigible au 2 décembre 2026 : P1-04 doit être engagé maintenant, quel que soit l'avancement du tour.
