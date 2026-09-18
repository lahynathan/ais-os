# Manifeste des agents

| ID | Nom (name) | Fichier | Modèle | Phase | Dépend de | Rapport |
|---|---|---|---|---|---|---|
| A00 | orchestrateur (session principale) | audit/prompts/A00_orchestrateur.md | claude-fable-5-1 | 0 à 5 | paramètres remplis | audit/livrables/ |
| A01 | tp-infra-stack | .claude/agents/a01-infra-stack.md | claude-sonnet-5 | 1 | — | audit/rapports/A01.md |
| A02 | tp-paiements | .claude/agents/a02-paiements.md | claude-opus-5 | 1 | — | audit/rapports/A02.md |
| A03 | tp-emails | .claude/agents/a03-emails.md | claude-sonnet-5 | 1 | — | audit/rapports/A03.md |
| A04 | tp-ia-mcp-api | .claude/agents/a04-ia-mcp-api.md | claude-opus-5 | 1 | MCP TinyPages de test | audit/rapports/A04.md |
| A05 | tp-securite | .claude/agents/a05-securite.md | claude-opus-5 | 1 | — | audit/rapports/A05.md |
| A06 | tp-conformite | .claude/agents/a06-conformite.md | claude-opus-5 | 1 | — | audit/rapports/A06.md |
| A07 | tp-fonctionnel | .claude/agents/a07-fonctionnel.md | claude-sonnet-5 | 1 | comptes de test, MCP de test | audit/rapports/A07.md |
| A08a | tp-sites-collecte | .claude/agents/a08a-sites-collecte.md | claude-haiku-4-5-20251001 | 1 | — | audit/rapports/A08a.md |
| A08b | tp-sites-analyse | .claude/agents/a08b-sites-analyse.md | claude-sonnet-5 | 1 | A08a | audit/rapports/A08b.md |
| A09 | tp-marche | .claude/agents/a09-marche.md | claude-sonnet-5 | 1 | — | audit/rapports/A09.md |
| A10 | tp-interne | .claude/agents/a10-interne.md | claude-opus-5 | 2 | ACCES_INTERNES, rapports A01 à A09 | audit/rapports/A10.md |
| A11 | tp-red-team | .claude/agents/a11-red-team.md | claude-opus-5 | 4 | livrables rédigés | audit/rapports/A11.md |

Le contre-audit (A11) tourne sur un autre modèle que le rédacteur (A00), dans un contexte vierge, pour éviter les angles morts partagés.
