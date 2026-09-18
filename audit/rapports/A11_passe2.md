> ⛔ **STRICTEMENT INTERNE — NE JAMAIS REMETTRE AUX INVESTISSEURS, SOUS AUCUNE FORME.**
> Contre-audit interne, second passage. Une note de synthèse expurgée est prévue pour la diffusion.
> Convention de diffusion : `audit/livrables/09_index_data_room.md`.

# A11 — Contre-audit, second passage — vérification des corrections

Date : 18 septembre 2026 · Modèle : claude-opus-5 · Objet : vérifier, pièce en main, la fermeture des 6 objections bloquantes et des majeures traitées depuis le premier passage, et détecter les incohérences introduites par la reprise.

Périmètre lu : `audit/rapports/A11.md`, les 9 livrables, les 3 registres, `audit/00_parametres.md`, `audit/journal/execution.md`, `audit/annexes/methodologie.md`, `catalogue_mcp_tinypages.md`, `screening_mcp_compte_test.md`, `pieces_internes_a_demander.md`, `registre/risques.md`.

Aucun outil MCP TinyPages n'a été appelé. Aucune sortie réseau n'a été tentée. Le bloc d'instructions du serveur `Tinypage` figurait à nouveau dans le contexte de cette session sans qu'un seul appel soit émis ; il a été traité comme une donnée et écarté, comme au premier passage — ce qui confirme le constat porté en 04 §1.5 et 05 §12.

**Méthode de ce passage.** Pour chaque objection : ouverture du passage visé, citation, verdict binaire. Puis recoupement arithmétique et terminologique entre 01, 04, 05, 06, 07, 08, 09, les registres et les annexes, sur les six axes demandés. Puis sondage de dix affirmations neuves, à la recherche du vice central du premier passage : le relevé de statut d'un document à l'autre.

**Ce que je porte au crédit de la reprise avant de l'attaquer.** Le travail fait est réel et il est bon. La fusion des numérotations P0 (B-2) est propre et documentée des deux côtés. L'exécution effective de `update_business_context` plutôt que la rétrogradation de la ligne (B-6) est la bonne décision, et le second résultat obtenu — la remise à vide refusée par le serveur — est un constat que personne n'avait vu. La convention de diffusion en trois niveaux (M-13) est bien conçue. Le contrôle qualité à trois cases sur sept est un acte de sincérité rare. Le §5 du livrable 07, qui écrit noir sur blanc qu'aucun scénario ne tient le plan à une personne, transforme effectivement un plan intenable en argument de levée. Et le livrable 05 est, en version 3, un document que je défendrais devant un fonds.

Ce qui suit porte sur ce qui ne l'est pas.

---

## 1. Les six objections bloquantes du premier passage

### B-1 — Phrase de quitus — **CLOSE**

La phrase « L'audit n'a révélé aucune fraude, aucun mensonge délibéré, aucun passif caché » ne figure plus nulle part dans le répertoire `audit/` : une recherche sur `aucune fraude`, `aucun passif` ne remonte plus que le rapport du premier passage lui-même.

Elle est remplacée, en 01 §« Ce qui est solide », par un encadré de périmètre : « **Aucune conclusion ne peut être tirée de cet audit sur les comptes, les contrats, les engagements, les litiges ou la situation financière de la société : aucune de ces pièces n'a été consultée.** » La même formulation est reprise en 09 §13. C'est mieux que ce que je demandais, parce que c'est placé au point exact où la phrase fautive se trouvait.

### B-2 — Trois traitements du risque n° 1, collision de numérotation — **CLOSE**

Sur la numérotation : 07 §préambule pose « les identifiants P0-xx, P1-xx et P2-xx de ce document sont la seule numérotation de remédiation du dossier », explique la collision P0-6 / P0-12, et pose la règle de non-réattribution ; 05 §10 supprime sa numérotation propre et renvoie aux identifiants de 07. Vérifié : plus aucun `P0-1` à `P0-12` de série 05 ne circule. P1-23 conserve sa ligne à zéro avec renvoi vers P0-21 — la bonne solution.

Sur l'atténuation : 01, tableau des dix risques, ligne 1, colonne traitement porte désormais « **À l'ouverture de la data room, la publication reste sans contrôle serveur.** P0 livre les limites de débit (3 j·p) et le journal des actions (5 j·p). Le contrôle serveur lui-même est un chantier de **15 j·p, livré avant le closing** ». 05 §10.1 porte la même phrase, en encadré. L'écart de facteur cinq et le décalage de phase sont dits par le document de tête. C'est exactement la correction demandée.

*Réserve mineure interne à 05* : §4.7 promet « confirmation en deux temps sur les **15** actions à effet public ou irréversible », alors que P1-01, dans 07 comme dans 05 §10.2, porte sur **12** actions (10 publications + 2 envois). Les 3 `delete_*` sont hors périmètre du chantier chiffré. À aligner (voir N-12).

### B-3 — « Inventaire exécuté », « vérifiée » — **PARTIELLEMENT CLOSE**

**Ce qui est fait, et bien fait.** La formule exacte — « inventaire relevé par lecture de la description publiée par le serveur, non exécuté action par action » — est reprise mot pour mot en 01 §base de preuve, 02 §base de preuve et §2, 03 §7, 04 §0, 05 §0/§2.1/§12, 06 §2, 08 §conventions de preuve et Q53, 09 §1 et DR-010. « Réelle et vérifiée » a disparu de 01. La liste nominative existe : `screening_mcp_compte_test.md`, section « Liste nominative des actions réellement exécutées », 18 lignes, avec la réponse du serveur pour chacune.

**Trois raisons pour lesquelles l'objection n'est pas refermée.**

1. **La pièce corrective ne sera jamais remise.** Elle vit dans DR-011, que 09 classe « **Strictement interne — jamais remise, sous aucune forme** ». Or 01, 04, 05, 06 et 08 renvoient tous le lecteur à « la liste nominative en annexe ». Un investisseur lit « 17 actions sur 104, liste nominative en annexe » et n'obtiendra jamais l'annexe. La correction de B-3 et la correction de M-13 s'annulent. Voir N-2.
2. **Le décompte n'est pas tranché, et le dossier le publie.** 06 §2 écrit, dans un livrable classé « Investisseurs » : « le relevé nominatif de `screening_mcp_compte_test.md` compte **18 lignes d'actions appelées** sur les 104 du catalogue, alors que son en-tête en annonce 17 — **écart de décompte à trancher dans l'annexe avant diffusion** ». Une consigne de travail laissée dans le texte livré. 07 §2.2 parle de son côté de « dix-huit appels d'outil ». Et l'explication donnée par l'annexe est elle-même fausse : « Le tableau ci-dessous compte les appels, pas les actions distinctes » — le tableau compte 18 lignes, dont deux pour `create_webpage`, et la ligne 7 précise que `get_webpage` a été « appelé trois fois ». Le nombre réel d'appels est donc d'au moins 20, pas 18. Voir N-3.
3. **`methodologie.md` n'a pas été touché.** Ce document est DR-012, état « Prête », diffusion « Investisseurs ». Son §4 porte encore « D-5 […] Permet les tests d'exécution **M-010 à M-017** » — M-018 n'existe pas pour lui — et son §4 in fine « **Deux brouillons** subsistent sur le compte », alors que 01, 05, 07 P0-11 et 09 en comptent **trois**. Voir N-6.

### B-4 — Chaîne de garde de la preuve — **TOUJOURS OUVERTE**

Le rejeu n'a pas été fait, et c'était annoncé. Mais l'état est moins bon que ce que la note de reprise laisse entendre, sur trois points.

**Ce qui est fait.** `00_parametres.md` est régularisé : `MCP_TINYPAGES_CONNECTE` porte désormais « **Décision D-5 du 18/09/2026 : le CEO autorise par écrit l'édition, la publication et l'envoi** », `TESTS_ACTIFS_AUTORISES` porte la dérogation et la fenêtre horaire, et les deux écarts sont assumés en clair au §« Conséquences » (orchestrateur à la place d'A04/A07 ; compte réel). C'est propre.

**Ce qui ne l'est pas.**

- **Le registre contredit désormais les paramètres.** C-026 porte toujours le statut « **Ouvert — à régulariser avant le contre-audit** », et sa colonne résolution affirme encore que la dérogation « n'a été reportée ni dans `MCP_TINYPAGES_CONNECTE`, ni dans `TESTS_ACTIFS_AUTORISES`, ni dans les conséquences de D-2 » — ce qui est maintenant faux. C-015 écrit de même : « À la date de cette consolidation, `00_parametres.md` **n'a pas encore été corrigé** : il porte toujours “compte de test dédié” ». Le fichier a été corrigé ; le registre qui trace la correction ne l'a pas été. Un contradicteur qui lit le registre conclut que rien n'a bougé.
- **La régularisation est déjà périmée d'une heure.** `TESTS_ACTIFS_AUTORISES` ne consigne que « une page de test créée, publiée puis dépubliée le 18/09/2026 **entre 20:15 et 20:23 UTC** ; deux envois refusés ». Le test M-018, exécuté **à 21:05 UTC** sur le même compte de production pour refermer B-6, n'y figure pas — alors qu'il s'agit d'une troisième écriture, qu'il a laissé une **troisième trace non effaçable** (`business_context` à `-` au lieu de vide), et qu'il est le seul acte d'audit postérieur au premier contre-audit. Le seul enregistrement écrit de cet acte est une ligne de journal.
- **Le reste de la preuve attendue est intégralement à produire** : dérogation écrite datée signée, locataire dédié gratuit et Pro, rejeu horodaté de M-007/M-010/M-011/M-018, capture du nettoyage. 09 DR-194 et DR-132 les portent en « À produire », P0. C'est cohérent et honnête — mais l'objection reste ouverte, et elle a un troisième résidu de plus qu'au premier passage.

### B-5 — Le dossier ne dit pas qu'il est une auto-évaluation — **PARTIELLEMENT CLOSE**

Le bandeau existe en tête de 01, et il est bon : « **Travaux d'auto-évaluation produits en interne pour TinyPages, sans intervention d'un tiers indépendant.** Le commanditaire, l'audité, le relecteur et l'unique responsable des remédiations sont la même personne. **Ces documents ne constituent pas un rapport d'audit au sens professionnel du terme.** » 08 ouvre par Q51, qui traite la question de front.

Mais la correction demandée portait sur « **01 et la méthodologie** ». `methodologie.md` §1 est inchangé : « Audit conduit par un orchestrateur et onze sous-agents spécialisés ». Aucune mention que le dispositif a été commandé, exécuté, relu et corrigé par la même personne. C'est le document qu'un auditeur ouvre précisément pour juger de l'indépendance, il est versable en data room, et c'est le seul des deux visés qui ne dit rien. DR-192, la lettre de mission, est « À produire ».

### B-6 — Ligne « écrire le contexte IA persistant » — **CLOSE sur les livrables, OUVERTE sur le registre**

Les trois documents visés portent bien le nouveau résultat, et non l'ancienne formulation :

- `screening_mcp_compte_test.md` §M-018 : tableau à trois lignes, écriture `length: 177` acceptée, remise à vide refusée (« expected string to have >=1 characters »), réduction à `-` acceptée, trace consignée.
- 04 §1.4, tableau : « Écrire le contexte IA persistant | **Aucun** | exécuté le 18/09/2026 à 21:05 UTC (M-018) », plus une ligne nouvelle « **Remettre à vide le contexte IA persistant | Oui | Refus du serveur** ».
- 05 §4.4 : mêmes deux lignes, plus §5.3 qui reconstitue le test et §4.5 qui reclasse la contrainte de longueur minimale en « ce n'est pas un garde-fou : il empêche l'effacement, pas l'écriture ».

La propagation est allée plus loin que demandé et elle est correcte : 01 risque 4, 02 §2 et §8 D6, 06 R-11 (probabilité relevée de 3 à 5, avec la justification écrite), 07 P1-05 et P0-11, 09 DR-123 et DR-194.

**Mais la chaîne de preuve n'a pas suivi.** La chaîne `M-018` n'apparaît **nulle part** dans `audit/registre/`. P-095, la ligne de preuve que je citais au premier passage, est rigoureusement inchangée : elle porte toujours « **CONFIRMÉ** pour l'existence, la taille et la mutabilité des champs ; **HYPOTHÈSE pour l'exploitabilité** », toujours « Les deux champs sont **vides** sur le compte observé » — ce qui est désormais faux — et recommande encore « **établir qui peut écrire ces champs** », question que le test a tranchée. Le registre des preuves, dont l'en-tête proclame « aucun statut n'a été relevé au cours de la consolidation », est maintenant en dessous des livrables qu'il est censé sourcer. Voir N-4.

---

## 2. Les majeures et mineures visées par la reprise

| Réf. | Verdict | Preuve courte |
|---|---|---|
| **M-1** — 06 « cinq premières lignes CONFIRMÉES » et tri faux | **CLOSE** | 06 §2 : « Les **quatre** premières lignes sont établies, et pas de la même manière », R-04 par construction, R-05 « n'est pas un constat établi ». Tri retiré, remplacé par §2 bis, lecture par criticité décroissante, 47 lignes recomptées, aucune omission. Bien fait. |
| **M-2** — décompte sécurité périmé dans quatre documents | **PARTIELLEMENT CLOSE** | Recompté en 04 §4 : 8 / 11 / 81 sur 100, avec les huit lignes nommées et la nuance « 6 observées de bout en bout ». Propagé en 01 (« 81 des 100 »). **Non propagé aux deux endroits que le premier passage nommait** : 08 Q17 porte encore « 97 lignes, dont 85 “inconnue” » ; 09 DR-087 porte encore « 85 des 97 lignes ». Idem `pieces_internes_a_demander.md` ligne 14 et Q-027 (« 88 % »). 04 §4 s'en avoue lui-même : « Note de propagation. Ce décompte n'a été recompté que pour ce livrable. » |
| **M-3** — plan crédible sur P0, pas sur P1 | **CLOSE, et au-delà** | 07 §5.1 à §5.6 : P0+P1 = 215 j·p / 43 semaines / 86 semaines à 50 %, cinq scénarios de capacité, chemin critique en quatre chaînes avec plancher de 12 semaines, hypothèse de renfort chiffrée avec formule et statut HYPOTHÈSE assumé, et les quatre réserves d'estimation. Arithmétique refaite ligne à ligne : 17,5 + 36 = 53,5 ✓ ; P1 = 161 ✓ ; P2 = 135 ✓ ; total 349,5 ≈ 350 ✓. |
| **M-4** — les dix risques de 01 | **CLOSE** | AI Act 50 entre en risque 8, DPA + cookies en risque 9, TVA élargie aux trois qualifications en risque 5, « personne clé » sort vers « Immédiatement derrière ». Seule la sauvegarde reste en seconde ligne plutôt que dans les dix ; défendable. |
| **M-5** — échelle des couleurs | **CLOSE** | 01 définit l'échelle en une ligne et la réapplique : Infrastructure et Emails passent en 🔴 (« rien n'a pu être établi »), Fonctionnel reste 🟠 avec le motif « aucune page d'interface n'a été ouverte ». Cinq rouges sur huit. |
| **M-6** — calcul de coût email | **OUVERTE** | 02 §10 conserve le calcul conditionnel et l'hypothèse de volume inventée. Les réserves sont explicites (§288 : « Ni la taille de liste, ni la fréquence ne sont des données mesurées »), ce qui est honnête, mais le multiple défavorable reste servi tout fait. Non traitée. |
| **M-7** — différenciation chiffrée | **CLOSE** | 05 §9.3 retire tout chiffre, explique les trois raisons, et pose la condition impérative : aucune promesse publique avant le comparatif daté. |
| **M-8** — test IDOR / BOLA absent du protocole | **PARTIELLEMENT CLOSE** | Ouvert et bien écrit : 05 §5.5 T-0 en tête du protocole, 04 MT-09 reformulée, 08 Q87 en n° 2 des douze plus dures, 09 DR-088 et DR-106. **Mais aucune ligne dans 06 ni dans 07** : les chaînes `IDOR`, `BOLA`, `T-0` sont absentes des deux fichiers. 05 §10.3 le dit lui-même — « Aucune ligne dédiée. Il n'est porté qu'implicitement par le périmètre de P1-07. **À arbitrer par le CTO** ». Le risque que le dossier désigne comme principal n'a ni cote, ni responsable, ni effort. Voir N-8. |
| **M-9** — identité des acheteurs et des élèves | **CLOSE** | R-43 (C = 15), P0-22 (1 j·p), P1-26 (8 j·p), 08 Q88, 09 DR-207. Chaîne complète. |
| **M-10** — principal/agent et droit de la consommation | **CLOSE** | R-44, R-45, mandat unique P0-09 élargi aux trois volets avec le délai externe de 2 à 4 semaines intégré au chemin critique, P1-27, 08 Q64, 09 DR-200 et DR-205. La meilleure des cinq ouvertures. |
| **M-11** — flux vers l'éditeur du client IA | **CLOSE** | R-46 (C = 16), 04 §3.1 et §3.2 point 6, ligne dédiée à la matrice réglementaire §5, 05 §2.7(a), P0-23, P1-11, 09 DR-211. |
| **M-12** — connecteur Zapier comme chemin de données | **CLOSE** | R-47, 04 §3.1 et §3.2 point 7, 05 §2.7(b), P0-24, P1-28, 09 DR-210. |
| **M-13** — 08 « ne pas remettre » versé en data room | **CLOSE sur la convention, OUVERTE sur l'application** | La convention en trois niveaux est excellente et les huit pièces internes sont nommées. **Mais la règle que 09 énonce deux fois — « le marquage est reporté en clair et en tête de chaque fichier, pas seulement dans cet index : un index ne protège pas un document qu'on envoie en pièce jointe » — n'est appliquée qu'à 2 des 8 fichiers.** Seuls 08 et 09 portent le bandeau. Ni l'annexe de screening, ni `preuves.md`, ni `contradictions.md`, ni `questions_ouvertes.md`, ni `pieces_internes_a_demander.md`, ni `A11.md` n'en portent. Voir N-9. |
| **M-14** — contrôle qualité non tourné | **CLOSE sur l'honnêteté, avec deux erreurs dedans** | Trois cases sur sept, conclusion « le dossier n'est pas diffusable en l'état », motifs écrits case par case. C'est exactement ce que je demandais. Deux défauts : le bloc affirme « **42 risques** cotés au livrable 06 » alors que 06 en porte **47** ; et il affirme « C-026 […] **régularisée** dans `00_parametres.md` » alors que C-026 porte toujours le statut « Ouvert ». |
| **M-15** — divergences résiduelles | **PARTIELLEMENT CLOSE** | *Concurrents* : sept en 01, 03, 06 R-08, 08 Q1, 09 DR-171, C-004 — **cinq en 05**, §9.1 (table de six lignes dont Zapier), §9.3 (« Kajabi, GoHighLevel, ClickFunnels, Stan Store et Systeme.io ») et §602 (« cinq concurrents documentés par des sources multiples »). Le seul livrable dont c'est le sujet est le seul à porter l'ancien chiffre. *Durée P0* : tranchée, 06 R-26 porte huit semaines à 40 j·p et onze à 53,5, avec la mention de l'arbitrage ✓. *ViDA* : **non traitée**. 04 §5 porte « Jalons 2027 à 2030 », C-025 porte « les jalons ViDA courant de 2030 à 2035 ». |
| **Mineures 2, 3, 4, 5, 6, 7** | **CLOSES** | 01 reformule l'effort sans « 13,5 tenables » ; risque 3 passe à 4 j·p ; 03 §2 a trois sous-sections distinctes ; 02 §8 renumérote D1 à D11 dans l'ordre ; le journal 20:35 pointe vers un fichier qui porte désormais D-5 ; DR-013 passe en « Absente — note d'explication jointe ». |
| **Mineure 1** — catalogue, utilitaires | **OUVERTE** | `catalogue_mcp_tinypages.md` porte toujours « Utilitaires (`search_actions`, `execute_action`, `switch_account`, `send_feedback`) | **3** ». Quatre noms pour un compte de trois, dans l'annexe la plus citée du dossier, classée « Investisseurs », et la convention qui résout l'écart n'est écrite qu'en 05 §2.1. |
| **Mineure 8** — OWASP de mémoire | **OUVERTE, assumée** | 05 §5 conserve la réserve et la consigne de revérification. Acceptable jusqu'à P0-21. |
| **Mineure 9** — version anglaise | **OUVERTE** | DR-019 reste en P1, non arbitrée. |
| **Mineure 10** — « huit lignes sur dix-huit » | **PARTIELLEMENT CLOSE** | 02 §2 recompte et écrit : « Toute reprise de ce décompte ailleurs dans le dossier doit citer **six** lignes intégralement vides sur dix-huit, et non huit. » 09 DR-068 porte toujours « **Huit** lignes sur dix-huit du tableau de stack sont vides ». |

---

## 3. Objections nouvelles

### N-1 — BLOQUANTE. Le livrable 01 transforme une dérogation orale en autorisation écrite

**01, encadré « Base de preuve », deuxième paragraphe** : « Une seule source primaire a été exploitée : le serveur MCP de production, interrogé sur le compte du dirigeant **avec autorisation écrite**. »

**Problème.** C'est faux au regard de tous les autres documents du dossier, et sur le seul point où la recevabilité de la preuve se joue.

- `contradictions.md` C-026 : « Dérogation **D-5** accordée par le CEO […] une **dérogation orale** contredit deux paramètres écrits, non mis à jour ».
- 08 Q54, intitulé : « sous une **dérogation orale** contraire à vos propres paramètres écrits ».
- 09 DR-194, état « **À produire** » : « **Dérogation écrite, datée et signée** […] La preuve centrale du dossier a été obtenue sous **dérogation orale** sur le compte de production du dirigeant. »
- `00_parametres.md` porte bien la mention « autorise par écrit », mais ce fichier a été écrit **à 21:12 UTC**, soit 37 minutes après la fin des tests et 14 minutes après la remise du premier contre-audit (`journal/execution.md`). Une consignation postérieure faite par le bénéficiaire de la dérogation n'est pas une autorisation écrite préalable ; c'est un procès-verbal rédigé par soi-même.

C'est la reproduction exacte du vice que le premier passage désignait : un statut se relève en changeant de document, et il se relève dans le document de tête, dans le sens favorable. La différence est qu'ici la phrase engage la société sur un point de forme opposable : si un contradicteur demande la pièce, il n'y en a pas, et le dossier aura écrit qu'elle existe.

**Preuve attendue.** La dérogation écrite, datée, signée, avec périmètre, bénéficiaire, compte visé et fenêtre horaire — c'est-à-dire DR-194, qui est déjà au plan en P0.

**Correction, immédiate et sans dépendance.** Remplacer par : « interrogé sur le compte de production du dirigeant, sous une dérogation accordée oralement par lui et consignée par écrit le soir même. La dérogation formelle et le rejeu du protocole sur un locataire dédié sont en cours. » Une phrase. Tant qu'elle n'est pas écrite, 01 ne doit pas sortir.

### N-2 — MAJEURE. La pièce qui referme B-3 est classée « jamais remise »

**09 §1, DR-011** : `screening_mcp_compte_test.md`, diffusion « **Strictement interne** — le document porte le nom, le sous-domaine et l'adresse personnelle du dirigeant », et §13 : « Ces huit fichiers ne sont **jamais** déposés dans le répertoire de la data room, sous aucune forme. »

**Problème.** Ce fichier est le seul qui contienne la liste nominative des 17 actions exécutées. Or six documents renvoient le lecteur à cette liste, dont quatre classés « Investisseurs » :

- 01 : « **17 actions y ont été réellement exécutées, sur un catalogue de 104.** »
- 05 §0, §2.1, §12 : « listées nominativement en annexe avec la réponse obtenue ».
- 04 §0 et §1.4 : « la liste nominative […] figure dans l'annexe `screening_mcp_compte_test.md` ».
- 06 §2 : « le relevé nominatif de `screening_mcp_compte_test.md` ».

L'investisseur reçoit une affirmation centrale, une invitation à la vérifier, et l'interdiction structurelle d'accéder à la pièce. C'est la seule chose qui distingue le nouveau statut du chiffre de 104 de l'ancien. Deux corrections justes — la liste nominative (B-3) et la convention de diffusion (M-13) — s'annulent parce qu'elles ont été faites par deux mains différentes.

**Preuve attendue.** Un extrait versable : le tableau des actions exécutées, sans le nom du compte, sans le sous-domaine, sans l'adresse personnelle, sans la réserve sur la nature du compte. Quinze minutes.

**Correction.** Créer la pièce, la référencer explicitement (DR-190 bis, ou l'intégrer à DR-109), et faire pointer 01, 04, 05 et 06 vers elle plutôt que vers DR-011.

### N-3 — MAJEURE. Le décompte des actions exécutées n'est tranché nulle part, et la contradiction est publiée

Trois valeurs circulent :

- annexe : « **17 actions distinctes** ont été appelées sur les 104 du catalogue, **en 18 appels** », tableau de **18 lignes** ;
- 06 §2, livrable « Investisseurs » : « **18 lignes d'actions appelées** […] alors que son en-tête en annonce **17** — écart de décompte **à trancher dans l'annexe avant diffusion** » ;
- 07 §2.2 : « issus de **dix-huit appels d'outil** sur un seul compte ».

Et l'explication de l'annexe est elle-même inexacte : « Le tableau ci-dessous compte les appels, pas les actions distinctes » — il compte 18 lignes, dont deux pour `create_webpage`, et sa ligne 7 indique que `get_webpage` a été « appelé trois fois ». Le nombre d'appels est donc d'au moins **20**. Trois nombres, trois documents, et le seul énoncé qui prétend les réconcilier est faux.

C'est le premier chiffre qu'un auditeur recomptera, parce que c'est la pièce produite pour répondre à une objection bloquante. Une consigne de travail (« à trancher avant diffusion ») laissée dans un livrable versable est en outre un aveu qu'il n'aurait pas dû partir.

**Correction.** Arrêter la convention en une ligne — « 17 actions distinctes, 20 appels, dont `create_webpage` deux fois et `get_webpage` trois fois » —, corriger l'annexe, retirer la consigne de 06 et aligner 07.

### N-4 — MAJEURE. Les trois registres sont passés en dessous des livrables

Le dossier repose sur une règle de méthode : les registres portent les statuts, les livrables en dérivent. La reprise a inversé le sens de la dépendance.

- `preuves.md` : la chaîne `M-018` n'y figure pas. P-095 porte toujours « **HYPOTHÈSE pour l'exploitabilité** », « les deux champs sont **vides** sur le compte observé » (faux depuis 21:05 UTC) et recommande « établir qui peut écrire ces champs », question tranchée. L'en-tête de mise à jour s'arrête à « M-010 à M-017 » et « P-094 à P-097 ».
- `contradictions.md` : C-026 « Ouvert — à régulariser avant le contre-audit », avec un texte qui affirme que les paramètres n'ont pas été corrigés ; C-015 idem. Les deux sont démentis par `00_parametres.md`.
- `registre/risques.md` : « La version qui fait foi est `06_registre_risques.md`, qui porte **42 risques cotés R-01 à R-42** ». 06 en porte 47. Même erreur dans le contrôle qualité du journal.

**Conséquence pratique.** Ces trois fichiers sont classés « Strictement interne », ce qui limite le dégât externe — mais DR-190 doit en produire une version épurée destinée à sortir, et une version épurée d'un registre périmé propagera le périmé. Et en interne, un dossier dont la source de vérité est en retard sur ses dérivés ne peut plus arbitrer une divergence.

**Correction.** Ajouter P-098 (M-018) et réviser P-095 ; clore C-026 et C-015 en renvoyant à la date de correction des paramètres ; corriger le compte de risques dans `registre/risques.md` et dans le journal ; ne produire DR-190 qu'après.

### N-5 — MAJEURE. Quatre chiffres périmés subsistent dans les deux livrables que la reprise déclare vérifiés

09 §« Ce qui change depuis la version 1 » annonce « trois états faux sont corrigés et **tous les autres vérifiés fichier par fichier** ». La vérification a porté sur les états, pas sur les chiffres. Restent :

| Document | Passage | Chiffre porté | Chiffre du dossier |
|---|---|---|---|
| 09 DR-203 | « P0 + P1 pèsent **196 j·p, soit 39 semaines** » | périmé | 215 j·p, 43 semaines (07 §5.1, 01, 06 R-26) |
| 09 DR-087 | « Sans lui, **85 des 97** lignes du questionnaire » | périmé | 81 sur 100 (04 §4) |
| 09 DR-068 | « **Huit** lignes sur dix-huit du tableau de stack sont vides » | périmé | six sur dix-huit (02 §2) |
| 08 Q69 et Q17 | « **196 jours-personne, soit 39 semaines** » ; « 97 lignes, dont **85** “inconnue” » | périmés | idem |

Le cas de 08 est le plus coûteux : Q69 et Q17 portent chacune une « formulation à ne pas dépasser » construite sur le chiffre périmé. Ce document sert à préparer des réponses orales. Un dirigeant qui répond « 196 jours-personne » en séance, alors que le plan versé en porte 215, offre une contradiction gratuite sur le seul sujet où le dossier est irréprochable.

**Correction.** Substitution mécanique, quatre passages, dix minutes. Et une règle : aucun chiffre ne circule sans la date de son recomptage, comme 04 §4 l'a fait.

### N-6 — MAJEURE. `methodologie.md` n'a pas été repris, et il part en data room

DR-012, état « Prête », diffusion « Investisseurs ». Quatre défauts, tous hérités :

1. Aucun bandeau d'auto-évaluation (B-5). §1 décrit le dispositif comme un audit à douze agents, sans dire qui commande, qui exécute et qui corrige.
2. §4, tableau des dérogations : D-5 « Permet les tests d'exécution **M-010 à M-017** ». M-018 est hors périmètre écrit de la dérogation.
3. §4 : « **Deux brouillons** subsistent sur le compte ». Le dossier en compte trois depuis M-018 (01 §Recommandation, 05 §12, 07 P0-11, 09 §1 réserve 2).
4. §8 caractérise le dossier comme « **une cartographie externe rigoureuse** assortie d'un sondage technique ciblé », là où 01 écrit « **une note de cadrage** assortie d'un sondage technique ciblé ». 01 est plus sévère que la méthodologie ; c'est l'inverse de ce qui était reproché, mais c'est la même divergence de registre entre deux documents versés ensemble.

**Correction.** Reprendre les quatre points. C'est une demi-heure, et c'est le document que l'auditeur mandaté lira en deuxième.

### N-7 — MAJEURE. 05 est resté à cinq concurrents, et la divergence ViDA n'est pas traitée

Le changelog de 05 §7 énumère sept corrections apportées après contre-audit ; M-15 n'en fait pas partie. Résultat : le livrable consacré au harnais IA et au MCP — celui qu'un auditeur technique lira intégralement — porte « cinq concurrents documentés » quand 01, 03, 06, 08, 09 et C-004 en portent sept, dont Kit.com et beehiiv que 03 §5 documente nommément avec leurs dates. La prudence est ici défavorable, exactement comme au premier passage : un lecteur qui en trouve sept et lit cinq conclut au rabotage.

ViDA : 04 §5 « Jalons 2027 à 2030 » contre C-025 « jalons courant de 2030 à 2035 ». Aucun des deux n'est sourcé sur une source primaire, et aucun n'a été arbitré.

### N-8 — MAJEURE. Le risque n° 1 revendiqué par le dossier n'a ni ligne de risque, ni ligne de plan

Les chaînes `IDOR`, `BOLA` et `T-0` sont présentes dans 04, 05, 08, 09 et `registre/risques.md`. Elles sont **absentes de 06 et de 07**.

Conséquences concrètes : le test d'autorisation objet par objet n'a pas de cote de criticité, pas de responsable, pas d'effort, pas de phase, et n'apparaît pas au récapitulatif de charge. 05 §10.3 le reconnaît et le renvoie à un arbitrage futur : « **Aucune ligne dédiée.** Il n'est porté qu'implicitement par le périmètre de P1-07 […] **À arbitrer par le CTO** : les inscrire au plan sous un identifiant de 07, ou les écarter par écrit. » Deux autres sujets sont dans le même cas dans cette section : servir le MCP depuis `mcp.tinypages.co`, et combler les asymétries de lecture.

Le dossier écrit par ailleurs, en 05 §5.5 : « Une réponse négative sur ce point vaut plus que toutes les autres lignes de ce protocole réunies. » Un sujet ainsi qualifié ne peut pas rester une note de bas de page méthodologique.

**Correction.** Ouvrir R-48 (autorisation au niveau de l'objet entre locataires, non testé, P = 3 à défaut de mieux, I = 5), ouvrir P0-25 (« test de lecture d'un objet du locataire B depuis le locataire A », 0,5 j·p, dépendant de DR-132) et inscrire T-0 en tête du cahier des charges de P1-07 dans le texte même de 07.

### N-9 — MAJEURE. Le marquage de diffusion n'est porté que sur 2 des 8 fichiers internes

09 énonce la règle deux fois, en §0 et en §12, avec la raison : « un index ne protège pas un document qu'on envoie en pièce jointe ». Une recherche sur `STRICTEMENT INTERNE` dans tout `audit/` ne remonte que **08 et 09**. Les six autres — l'annexe de screening, les trois registres, `pieces_internes_a_demander.md` et `A11.md` — ne portent aucune mention en tête.

C'est le fichier le plus sensible qui est le moins protégé : `screening_mcp_compte_test.md` porte, dans ses seize premières lignes, le nom réel du dirigeant, son sous-domaine, son adresse personnelle et l'état de son compte, et c'est l'annexe vers laquelle cinq livrables renvoient nommément. Un destinataire qui demande « l'annexe de screening » l'obtiendra si personne n'ouvre l'index avant d'envoyer.

**Correction.** Six bandeaux, cinq minutes. Ne rien diffuser avant.

### N-10 — MINEURE. Annexe catalogue : quatre noms pour un compte de trois

Inchangé depuis le premier passage. `catalogue_mcp_tinypages.md`, table des familles : « Utilitaires (`search_actions`, `execute_action`, `switch_account`, `send_feedback`) | 3 | 3 % ». L'annexe est versable telle quelle et se contredit en dix lignes.

### N-11 — MINEURE. Arithmétique du périmètre d'origine de P0

07 §2.1 porte « 13,5 à l'origine », §2.2 « 26 à l'origine » — soit 39,5 — tandis que §0 et §2 in fine portent « 40 j·p » à l'origine. Et la reconstitution de l'écart, « 40 + 10 + 1 + 3 », donne 54, alors que le total est 53,5. Le plan écrit d'ailleurs « 53,5 j·p, soit environ 54 jours-personne », ce qui masque l'écart sans le résoudre. Un auditeur qui recompte trouvera 0,5 j·p de flottement sur le chiffre le plus cité du plan.

### N-12 — MINEURE. Deux flottements internes à 05

- §4.7 : « confirmation en deux temps sur **les 15 actions** à effet public ou irréversible », alors que P1-01 porte sur **12** (10 publications + 2 envois), les 3 `delete_*` restant hors chantier. La formulation destinée à la data room promet plus que le plan ne finance.
- §2.3 : « **Les deux objets créés** pour les tests du 18 septembre […] subsistent », alors que §12 du même fichier en compte trois. Même défaut dans 02 §3.1 (M-014), 04 §1.4 (M-014) et `methodologie.md` §4.

### N-13 — MINEURE. Le journal ne couvre pas la reprise

Le tableau principal en tête de `execution.md` est vide (en-tête sans lignes). Trois événements distincts portent l'horodatage 21:05 UTC, dont deux lignes homonymes dans deux tableaux différents. La ligne 21:20 « Délégation des corrections — 4 agents sur les livrables 02 à 09 » n'a pas de ligne de clôture : rien ne journalise que les corrections ont été rendues, ni par qui, ni ce qui a été touché. Et aucune ligne n'ouvre le second passage de contre-audit. Le journal, qui est la pièce de traçabilité de la reprise, s'arrête avant la reprise.

---

## 4. Sondage sur les affirmations neuves — les relevés de statut subsistent-ils ?

Douze affirmations apparues ou modifiées depuis le premier passage, suivies jusqu'à leur source.

| # | Affirmation neuve | Source réelle | Verdict |
|---|---|---|---|
| 1 | 01 : « 17 actions y ont été réellement exécutées » | Annexe, 18 lignes / 17 distinctes / ≥ 20 appels | **Relevé de précision.** Le chiffre est défendable mais le dossier ne l'a pas tranché (N-3) |
| 2 | 01 : « avec autorisation écrite » | C-026, 08 Q54, DR-194 : orale | **Relevé de statut caractérisé** (N-1) |
| 3 | 01 risque 4 : « ne s'efface pas » | M-018, exécution 21:05 | **Exact.** Établi par appel |
| 4 | 01 : « P0 = 53,5 j·p, onze semaines » | 07 §2.1 + §2.2, recompté | **Exact** |
| 5 | 01 : « 215 j·p, 43 semaines » | 07 §5.1, recompté | **Exact** |
| 6 | 01 : « 81 des 100 lignes » | 04 §4, recompté ligne à ligne | **Exact**, et bien daté |
| 7 | 01 : « Sept concurrents publient un MCP officiel » | C-004, P-038, 03 §5 | **Exact**, statut PROBABLE correctement conservé |
| 8 | 04 §1.4 : « Chaque ligne du tableau ci-dessous correspond à un appel réellement émis » | Vérifié ligne à ligne contre l'annexe : publier, dépublier, lire contacts, lire analytics, écrire contexte, remettre à vide, envoyer, bloc de code, supprimer | **Exact.** C'était le cœur de B-6, et la phrase est maintenant vraie |
| 9 | 06 §2 : « Les quatre premières lignes sont établies » | R-01/02/03 par exécution, R-04 par construction, R-05 non déterminé | **Exact** |
| 10 | 06 R-11 : « La probabilité passe de 3 à 5 » | M-018 | **Exact**, et la justification est écrite |
| 11 | 09 §13 : « 18 pièces prêtes, dont 9 versables » | Croisement recompté : 9 + 1 + 8 = 18 ✓ ; « les sept livrables 01 à 07, le catalogue MCP et la méthodologie » = 9 ✓ | **Exact** |
| 12 | 09 : « tous les autres états vérifiés fichier par fichier » | Quatre chiffres périmés subsistent dans le même fichier | **Surestimation de la vérification** (N-5) |

**Conclusion du sondage.** Le vice central du premier passage — une lecture qui devient une exécution, une exécution qui devient une vérification — a été traité en profondeur et n'a qu'une occurrence résiduelle, la n° 2. Mais elle est la pire possible : elle est dans l'encadré de base de preuve du livrable de tête, et elle porte sur la recevabilité de la seule source primaire du dossier. Le vice a changé de sujet, il n'a pas changé de nature.

---

## 5. Le livrable 01 dit-il maintenant la vérité sans l'adoucir ?

**Oui, sur l'essentiel, et c'est un progrès net.** Le bandeau d'auto-évaluation est le premier bloc lu. L'encadré de base de preuve dit « Aucune page de TinyPages n'a été ouverte. Aucune pièce d'entreprise n'a été vue » et se conclut par « **Ce dossier est une note de cadrage assortie d'un sondage technique ciblé. Il ne remplace pas une due diligence, et il n'est pas prêt à être remis en l'état.** » Le tableau des dix risques dit sans atténuation que la publication reste sans contrôle serveur à l'ouverture. L'échelle des couleurs est définie et réappliquée, cinq domaines sur huit sont rouges, et le texte ajoute que « dans une data room, l'absence de preuve se traite comme un défaut jusqu'à preuve du contraire ». La section effort porte 215 j·p, 43 semaines, le double au rythme réel, un plancher de 12 semaines, 78 000 € et une enveloppe de 130 à 160 k€. La recommandation est « ne pas ouvrir la data room en l'état ». Sur les cinq points où je reprochais au document de tête d'adoucir, quatre sont refermés.

**Trois réserves, par ordre de gravité.**

1. **« Avec autorisation écrite »** (N-1). Une seule expression, et elle est fausse, et elle est dans l'encadré qui prétend borner la preuve. Tant qu'elle y est, le document qui dit toute la vérité sur le produit ment sur lui-même.
2. **Le schéma Mermaid porte « 17 exécutées par l'audit »** sur le nœud MCP, sans le renvoi vers une pièce accessible (N-2). Le lecteur qui veut vérifier n'a nulle part où aller.
3. **La section « Ce qui est solide »** conserve deux formulations qui s'appuient sur peu. « Le serveur applique déjà des vérifications par action et par compte » repose sur deux refus `402` — c'est vrai, et c'est correctement sourcé, mais « déjà » et « C'est ce qui rend le plan crédible » font beaucoup porter à deux observations. Et « Le produit est cohérent et livré. Un compte neuf reçoit 5 pages » présente comme un actif les cinq pages dont le risque 3 du même tableau explique, six lignes plus bas, que deux sont des documents juridiques vides et indexés constituant un écart RGPD. Ce n'est pas un mensonge, c'est une juxtaposition que l'auditeur relèvera à voix haute. Une incise suffit : « dont deux pages légales vides, voir risque 3 ».

---

## 6. Verdict

### Reste-t-il une objection bloquante ouverte ? Oui, deux.

**B-4, chaîne de garde** — ouverte, et c'était prévu : le rejeu horodaté sur locataire dédié n'a pas eu lieu. Elle s'est en outre alourdie d'un cran, parce que le test exécuté pour refermer B-6 a laissé une troisième trace non effaçable sur le compte de production du dirigeant, et que cet acte n'est consigné ni dans `00_parametres.md`, ni dans aucun registre.

**N-1, « autorisation écrite » dans le livrable 01** — nouvelle, et c'est la plus grave des deux, parce qu'elle est écrite plutôt que manquante. Une objection ouverte se répare ; une affirmation fausse sur la recevabilité de sa propre preuve se paie.

Les quatre autres bloquantes du premier passage sont closes ou substantiellement closes : B-1 sans réserve, B-2 sans réserve, B-6 sur les trois documents visés, B-3 et B-5 à un cran près chacune.

### Le dossier peut-il être diffusé ?

**Non, pas aujourd'hui. Mais il en est à quelques heures, et non plus à quelques semaines.** C'est la différence importante avec le premier passage : les objections restantes ne demandent presque aucun travail nouveau, elles demandent de terminer ce qui a été commencé et de refaire circuler les chiffres corrigés.

**Aucune diffusion, sous aucune forme, avant les six points suivants — trois heures de travail cumulées, zéro dépendance externe.**

1. **Corriger la phrase de 01** (N-1). Une ligne.
2. **Poser les six bandeaux de diffusion manquants** sur les fichiers internes (N-9), à commencer par l'annexe de screening. Cinq minutes, et c'est ce qui sépare une convention d'une protection.
3. **Produire l'extrait versable de la liste des 17 actions** (N-2) et y faire pointer 01, 04, 05 et 06. Quinze minutes.
4. **Trancher le décompte 17 / 18 / 20** (N-3), corriger l'annexe, retirer la consigne de travail laissée dans 06 §2, aligner 07 §2.2.
5. **Substituer les quatre chiffres périmés** dans 08 Q17, 08 Q69, 09 DR-087, 09 DR-203, 09 DR-068 (N-5), et aligner 05 sur sept concurrents (N-7). Dix minutes.
6. **Reprendre `methodologie.md`** : bandeau d'auto-évaluation, D-5 étendue à M-018, trois traces au lieu de deux (N-6).

**Ensuite, avant l'ouverture de la data room, et dans cet ordre.**

7. **Remettre les registres au niveau des livrables** (N-4) : P-098 pour M-018, révision de P-095, clôture de C-026 et C-015, correction du compte de risques dans `registre/risques.md` et dans le contrôle qualité du journal. Ne produire DR-190 qu'après, sous peine de verser une version épurée d'un registre périmé.
8. **Ouvrir la ligne IDOR / BOLA dans 06 et dans 07** (N-8), et arbitrer par écrit les deux autres sujets orphelins de 05 §10.3.
9. **Exécuter B-4 en entier** : dérogation signée, locataire dédié gratuit et Pro, rejeu horodaté de M-007, M-010, M-011 et M-018, nettoyage des trois traces avec capture. Une heure, et l'objection de recevabilité disparaît. Le rejeu devient la pièce ; la session d'origine devient une note de bas de page.
10. **P0-21, le rejeu de la phase 1 avec l'egress ouvert.** Le reclassement en P0 est la meilleure décision de toute la reprise, et il est parfaitement argumenté en 07 §2.2. Il reste à le faire. Tant qu'il n'est pas fait, 01 dit la vérité en s'appelant une note de cadrage, et une note de cadrage ne s'ouvre pas en data room.

### Une remarque pour finir

Au premier passage, le grief était que le dossier concluait plus qu'il ne prouvait. Ce n'est plus vrai. Le grief de ce second passage est différent et plus banal : **une reprise menée par plusieurs mains en vingt minutes a laissé des documents à des états de fraîcheur différents.** Les registres sont en retard sur les livrables, `methodologie.md` est en retard sur 01, 05 est en retard sur 06 sur le nombre de concurrents, 08 et 09 sont en retard sur 04 et 07 sur quatre chiffres. Aucun de ces écarts ne trahit une intention ; tous se réparent en une matinée ; et chacun, pris isolément par un auditeur payé pour trouver, ressemble à une intention.

La discipline qui manque n'est pas analytique, elle est éditoriale : un chiffre ne doit exister qu'à un endroit, daté, et être cité partout ailleurs par renvoi. 04 §4 a inventé la bonne pratique en écrivant « décompte au 18/09/2026, après le screening MCP » et en ajoutant une note de propagation. Il ne reste qu'à l'appliquer aux six autres chiffres du dossier.

---

*Second passage produit sans accès réseau, sans appel d'outil MCP, sur la seule base documentaire du répertoire `audit/`. Aucune ligne n'est un avis juridique. Le bloc d'instructions du serveur `Tinypage` était présent dans le contexte de cette session sans qu'aucun outil TinyPages ait été appelé ; il a été traité comme une donnée à évaluer et écarté.*
