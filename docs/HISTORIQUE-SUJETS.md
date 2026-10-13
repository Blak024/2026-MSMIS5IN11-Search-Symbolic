# Sujets réalisés lors des promotions précédentes

Cette annexe évite de reproposer sous un autre nom un projet déjà réalisé. Elle recense des **thèmes**, sans les personnes ni les évaluations. Les sujets ci-dessous ne font pas partie du catalogue actif 2026-2027.

## Filière Ingénierie et santé (MSMIS5IN10 / MSMIS5IN11) — partie exploratoire et symbolique

| Promotion | Thème réalisé | Technique centrale |
|---|---|---|
| 2023-2024 | Dossiers médicaux sur blockchain | Smart contracts |
| 2023-2024 | Systèmes experts (pathologies de l'épaule, diabète) | Règles, programmation logique |
| 2023-2024 | Maintenance de vérité | TMS |
| 2023-2024 | Menus diététiques (régime de Stigler) | Programmation par contraintes, Z3 |
| 2024-2025 | Effet des médicaments | Maintenance de vérité (TMS) |
| 2024-2025 | Programmation logique appliquée à la santé | Prolog |
| 2024-2025 | Planification du personnel | Programmation par contraintes |
| 2024-2025 | Cancer du sein | Métaheuristiques |
| 2024-2025 | Menu diététique sous contraintes | Programmation par contraintes |
| 2025-2026 | Diagnostic médical (contrôle continu) | Agent rationnel, A*, algorithme génétique, Z3 |

## Thèmes santé déjà réalisés dans les autres cursus (EPF, ECE, EPITA)

| Thème réalisé | Technique centrale |
|---|---|
| Plannings infirmiers (deux fois) | Programmation par contraintes, CP-SAT |
| Échange de reins | Appariement, programmation linéaire en nombres entiers |
| Système expert médical | Programmation logique |
| Planification nutritionnelle (problème DIET) | Programmation par contraintes |
| Ordonnancement des blocs opératoires | Programmation par contraintes |
| Optimisation stochastique de protocoles pharmaceutiques | Optimisation sous incertitude |
| Planification oncologique symbolique (protocoles de chimiothérapie) | Ontologies, Z3, raisonnement probabiliste |
| Aide au diagnostic du diabète (exercice de contrôle continu) | Règles et logique |

## Autres thèmes très souvent réalisés — à ne pas reprendre tels quels

| Famille | Thèmes |
|---|---|
| Jeux et puzzles CSP | Sudoku, Wordle, Démineur, mots-croisés, Picross, Quoridor, Puissance 4, puzzles logiques par SMT, génération procédurale de niveaux ou de villes |
| Ordonnancement et emploi du temps | Job-shop, RCPSP, équilibrage de chaîne d'assemblage, emploi du temps universitaire, calendrier sportif, missions satellites, ordonnancement ferroviaire |
| Tournées et logistique | VRP classique et électrique, livraison par drones, robots d'entrepôt, itinéraires touristiques, conception de chaîne logistique, bin packing 3D |
| Affectation et appariement | Mariages stables, affectation d'étudiants à des projets, allocation multicritère, enchères combinatoires |
| Configuration et ressources | Configuration de produit, allocation de VM cloud, placement en edge computing, dispatch électrique, planification urbaine |
| Logique et vérification | Démonstration par SAT, bounded model checking de programmes C, cryptanalyse par SAT/CP, covering arrays, model checking d'un robot de trading |
| Représentation des connaissances | GraphRAG, questions-réponses sur graphe de connaissances, agents sémantiques RDF, maintenance de vérité (JTMS) |
| Argumentation | Sémantiques de Dung, détection de sophismes, génération de contre-arguments, qualité argumentative, système de débat assisté par IA |
| Finance | Portefeuille sous contraintes, trading par algorithmes génétiques, détection de fraude, market making, exécution TWAP/VWAP, robo-advisor |
| Hybrides LLM + solveur | Assistant de planification conversationnel, explication de solutions CP par LLM, modélisation CP assistée par LLM, vérificateur symbolique de LLM |

## Règle de différenciation

Un sujet proche d'un thème historique doit changer son **verrou technique central**, pas seulement son nom, son secteur ou son interface. Exemples tirés du catalogue :

- A1 (tournée de soins à domicile) n'est pas un VRP classique : le verrou est la **planification temporelle** avec actions durables et fenêtres de soins.
- A2 (robots hospitaliers) n'est pas l'entrepôt multi-robots : le verrou est la **recherche multi-agents sans collision** (CBS) dans des couloirs partagés.
- B1 (radiothérapie) n'est pas l'ordonnancement des blocs opératoires : le verrou est la **régularité des séances** sur plusieurs semaines et la gestion des machines.
- D1 (déprescription) n'est pas un système expert médical : le verrou est l'**optimisation MaxSAT** et l'**explication par sous-ensembles minimaux insatisfiables**.
- F1 (recommandation de bonne pratique) n'est pas la planification oncologique : le verrou est la **formalisation d'un texte** et la **détection d'incohérences** par SMT.

Un sujet libre proche d'un thème listé ici doit expliciter ce qui le différencie et être validé avant de commencer.
