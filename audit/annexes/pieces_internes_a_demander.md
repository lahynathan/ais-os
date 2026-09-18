# Pièces internes à demander

Établi par A00 en phase 2. `ACCES_INTERNES = aucun` : l'agent A10 n'a pas tourné, et aucune preuve interne n'est entrée dans le dossier.

**Ce document n'est pas l'index de la data room.** Il liste ce qu'il faut obtenir en interne pour **fermer les questions que l'audit a laissées ouvertes**. L'index de ce qui sera remis aux investisseurs est `audit/livrables/09_index_data_room.md`.

Chaque pièce indique ce qu'elle débloque. Une pièce qui ne ferme aucune question n'a pas sa place ici.

## P0 — Bloque l'ouverture de la data room

| # | Pièce | Ferme | Détenteur | Effort |
|---|---|---|---|---|
| 1 | K-bis, statuts, pacte d'associés | L'entité, sa forme, son pays, ses dirigeants. Détermine l'applicabilité de la facturation électronique, du représentant UE au sens RGPD art. 27 et DSA art. 13, et l'autorité de contrôle chef de file. Six lignes de la matrice réglementaire en dépendent. | CEO | 1 h |
| 2 | Réponse écrite à trois questions de fait : qui figure sur le reçu et la facture de l'acheteur final ; qui fixe les CGV du tunnel d'achat ; qui décide de la mise à disposition du fichier | La qualification au titre de l'article 9 bis du règlement d'exécution UE 282/2011. C'est le risque le plus coûteux du dossier et il se tranche avec trois réponses. | CTO + CEO | 1 h |
| 3 | Configuration Stripe Connect : type de compte, mécanisme de prélèvement de la commission, porteur des pertes sur litiges et soldes négatifs | A02-Q1, A02-Q2, et la clause de responsabilité financière sur les activités initiées avec les identifiants de la plateforme — critique sur un produit pilotable par IA. | CTO | 30 min |
| 4 | Capture d'une page de paiement réelle, avec la liste des scripts chargés | Le périmètre PCI DSS, et le croisement avec le critère d'éligibilité au SAQ A. | CTO | 15 min |
| 5 | Politique de confidentialité, CGU et CGV réelles de TinyPages, plus le DPA proposé aux créateurs et la liste des sous-traitants | Une quinzaine de lignes de la matrice RGPD, aujourd'hui toutes en « Non observé ». Bloquant en due diligence. | CEO + avocat | 2 h |
| 6 | Réponse écrite : existe-t-il un journal des actions effectuées par l'IA, et une annulation ? | Le point le plus scruté du livrable sur le harnais IA. Si la réponse est non, c'est un chantier P0, pas une pièce. | CTO | 15 min |
| 7 | Cession des droits des fondateurs à la société, contrats des prestataires et stagiaires ayant écrit du code | La titularité du code antérieur à l'association d'août 2024. Point de blocage classique de closing. | CEO + avocat | variable |

## P1 — Avant le closing

| # | Pièce | Ferme | Détenteur | Effort |
|---|---|---|---|---|
| 8 | Schéma d'architecture, inventaire des fournisseurs, région d'hébergement | Le tableau de stack, dont 8 lignes sur 18 sont vides. | CTO | 2 h |
| 9 | Enregistrements DNS des domaines d'envoi : SPF, DKIM, DMARC | A03-Q1 à Q3. Une commande `dig` suffit une fois l'accès ouvert. | CTO | 15 min |
| 10 | Mécanisme d'isolation de la réputation email entre créateurs, politique de suspension en cas de dépassement du seuil de plaintes | A03-Q4, et le risque de retrait groupé de tous les sites clients. | CTO | 30 min |
| 11 | Attribut `sandbox` complet et domaine du `src` de l'iframe de code personnalisé, plus les attributs du cookie de session | A05 le désigne comme l'action la plus rentable du dossier : ces deux relevés décident si trois risques majeurs sont théoriques ou réels. | CTO | 30 min |
| 12 | Factures fournisseurs sur 12 mois, ramenées au compte actif et au plan | Coûts unitaires et marge brute, aujourd'hui entièrement absents. | CEO | 3 h |
| 13 | Métriques : comptes, sites publiés, volumes de paiements et d'emails, adoption du MCP | Toute la traction. A09 n'a trouvé aucune métrique publique exploitable. | CEO | 2 h |
| 14 | Dernier test d'intrusion, s'il existe, et politique de gestion des vulnérabilités | 85 des 97 lignes du questionnaire de sécurité sont en « inconnue ». | CTO | — |
| 15 | Accès en lecture au dépôt de code | Architecture, tests, couverture, CI/CD, revue, part de code généré par IA, SBOM et licences. Tout le volet A10. | CTO | — |
| 16 | Sauvegardes testées, plan de reprise avec RPO et RTO, historique des incidents | Volet résilience, absent du dossier. | CTO | 1 h |
| 17 | Compte Pro de test, distinct et anonyme | Ferme la seule question laissée ouverte par le screening : l'envoi est-il sans contrôle sur un plan Pro ? Et permet le pentest sans toucher au compte du dirigeant. | CTO | 30 min |

## P2 — Après le closing

| # | Pièce | Ferme | Détenteur |
|---|---|---|---|
| 18 | Batterie d'évals du pilotage par IA, si elle existe | Le risque produit numéro un : tous les garde-fous sont exécutés par un modèle tiers mis à jour sans préavis. | CTO |
| 19 | Modèle, fournisseur, coût et données transmises pour l'IA intégrée au produit | Le volet coût et confidentialité de l'IA produit. | CTO |
| 20 | Dépôts de marque INPI et EUIPO, portefeuille de noms de domaine | La propriété intellectuelle. Une marque canadienne antérieure homonyme a été signalée. | CEO |
| 21 | Contrats Stripe et PayPal signés, facture de commission aux créateurs | Le volet contractuel des paiements. | CEO |
| 22 | Procédure de départ, liste des accès à la production, MFA | Le volet gestion des accès. | CTO |

## Ce qu'aucune pièce ne fournira

A02 le relève justement : le taux de litiges consolidé, le montant de la réserve Stripe et le taux d'échec des paiements hors session n'existent que si quelqu'un les a mesurés. Si ces mesures n'existent pas, la réponse honnête en data room est qu'elles n'existent pas encore, accompagnée de la date à laquelle elles existeront. Un investisseur préfère cette réponse à une estimation habillée.

## Effort total estimé

Les sept pièces P0 représentent environ une journée de travail cumulée, hors intervention de l'avocat. C'est le meilleur rapport entre l'effort et la solidité du dossier de tout le plan de remédiation : sept pièces ferment la majorité des questions bloquantes.
