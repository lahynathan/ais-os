# Registre des risques (travail)

Tenu par l'orchestrateur uniquement. **La version qui fait foi est `audit/livrables/06_registre_risques.md`**, qui porte 42 risques cotés R-01 à R-42 avec leurs échelles, mitigations, responsables et échéances.

Compte tenu du calendrier resserré de cet audit, la consolidation des risques a été faite directement dans le livrable plutôt qu'en deux temps. Ce registre de travail ne conserve donc que les risques **établis par l'orchestrateur lui-même** lors du screening MCP, qui n'existaient dans aucun rapport d'agent et qui ont alimenté le livrable.

| ID | Catégorie | Description | Probabilité | Impact | Criticité | Mitigation | Responsable | Échéance | Statut |
|---|---|---|---|---|---|---|---|---|---|
| T-01 | IA | Publier une page ne passe par aucun contrôle serveur. Établi par test : la consigne « ne pas publier » a été volontairement ignorée, la page est partie en ligne en un appel depuis un compte gratuit. | 5 — avéré | 5 | 25 | Contrôle serveur sur les actions de publication et d'envoi, limites de débit (P0-12) | CTO | P0 | Ouvert |
| T-02 | IA | Les seuls contrôles serveur observés sont commerciaux. Le bloc de code et l'envoi sont refusés faute de plan Pro, la publication ne l'est pas. Le point d'application existe donc déjà : le chantier est une extension, pas une création. | 5 — avéré | 4 | 20 | Étendre la vérification par action au-delà du critère de facturation | CTO | P0 | Ouvert |
| T-03 | CONF | La plateforme publie sur chaque compte une politique de confidentialité et des conditions d'utilisation vides, indexées, pendant que la page d'accueil par défaut collecte des emails sans double opt-in. | 5 — avéré | 4 | 20 | Gabarit réel, publication conditionnée, `noindex`, rétro-traitement du parc (P0-01, P0-02) | CTO | P0 | Ouvert |
| T-04 | SEC | `businessContext` (10 000 caractères) et `aiSystemPrompts` sont injectés dans toutes les générations futures, modifiables par l'IA, sans contrôle d'écriture observé. Injection persistante qui survit à la session et n'apparaît dans aucune sortie. | 3 | 4 | 12 | Contrôle d'écriture, journalisation, revue humaine sur modification | CTO | P0 | Ouvert |
| T-05 | IA | Aucune action ne supprime une page, un email, un contact ou un produit. L'IA ne peut pas revenir sur ce qu'elle a créé. Deux brouillons de test en sont la démonstration : l'audit n'a pas pu les effacer. | 5 — avéré | 3 | 15 | Vérifier si la limite vaut aussi en interface ; si oui, traiter le volet RGPD article 17 | CTO | P1 | Ouvert |
| T-06 | RISK | Le compte utilisé pour le screening n'est pas un banc d'essai anonyme : nom réel et adresse personnelle du dirigeant, usage actif pendant l'audit. Les tests restants et tout pentest exigent un locataire dédié. | 5 — avéré | 2 | 10 | Créer un compte de test dédié et anonyme (P0-11, pièce interne 17) | CTO | P0 | Ouvert |
| T-07 | IA | Le comportement de `send_email` sur un compte Pro n'a pas été testé. Le message d'erreur observé suggère qu'un compte Pro enverrait sans autre contrôle, ce qui étendrait T-01 à l'envoi. | 3 | 4 | 12 | Rejouer le test sur un compte Pro dédié | CTO | P0 | Ouvert |

Ces sept risques sont repris et cotés dans le livrable 06, où ils apparaissent en tête du registre sous les identifiants R-01 à R-05 et suivants.
