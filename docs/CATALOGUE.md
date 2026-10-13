# Catalogue des sujets 2026-2027 — IA exploratoire et symbolique

Ce catalogue est propre à la promotion **MSMIS5IN11 2026-2027 (Ingénierie et santé)**. Tous les sujets ont été vérifiés contre les projets réalisés depuis 2023 à l'EPF, à l'ECE et à l'EPITA (voir [l'historique](HISTORIQUE-SUJETS.md)). Aucun ne reprend le verrou technique d'un projet déjà rendu, et tous portent sur la santé.

Chaque fiche donne une difficulté indicative, un **MVP** (ce qui doit fonctionner le jour de la soutenance), un critère de **réussite**, des **extensions** possibles et une analyse de **faisabilité** avec un plan B. Les liens **CoursIA** pointent vers les notebooks du dépôt [jsboige/CoursIA](https://github.com/jsboige/CoursIA) qui servent de point de départ.

> **Exigence commune.** Toute solution produite par un solveur ou une heuristique doit être contrôlée par un **vérificateur indépendant** (une fonction qui recompte les contraintes violées). Comparez toujours votre méthode à une baseline simple (glouton, aléatoire, règle métier).
>
> **Données.** Instances publiques, open data ou générateurs synthétiques documentés uniquement ; aucune donnée réelle de patient.

## Index

| Code | Sujet | Difficulté |
|---|---|:---:|
| **A** | **Recherche heuristique et planification** | |
| A1 | Planification temporelle d'une tournée de soins à domicile | 3/5 |
| A2 | Robots de livraison de médicaments à l'hôpital : recherche multi-agents | 4/5 |
| A3 | Itinéraires accessibles en fauteuil roulant : A* multicritère | 3/5 |
| A4 | Parcours de soins par planification hiérarchique (HTN) | 3/5 |
| **B** | **Programmation par contraintes et optimisation combinatoire** | |
| B1 | Planification des séances de radiothérapie | 3/5 |
| B2 | Logistique du don du sang : collectes et péremption | 3/5 |
| B3 | Dépistage groupé : concevoir les pools de tests | 4/5 |
| B4 | Gestion des lits et isolement infectieux | 3/5 |
| B5 | Qui tester en priorité ? Optimisation sur un graphe de contacts | 4/5 |
| **C** | **Métaheuristiques et calcul évolutionnaire** | |
| C1 | Implanter des défibrillateurs publics : couverture maximale | 3/5 |
| C2 | Optimisation multi-objectif des faisceaux en radiothérapie | 4/5 |
| C3 | Conception évolutionnaire de molécules candidates | 4/5 |
| C4 | Calibrer un modèle pharmacocinétique sans gradient | 3/5 |
| **D** | **Logique, SAT/SMT et vérification formelle** | |
| D1 | Déprescription chez le patient polymédiqué : MaxSAT et explications minimales | 4/5 |
| D2 | Vérification formelle d'une pompe à perfusion | 4/5 |
| D3 | Attracteurs des réseaux de régulation génétique booléens | 4/5 |
| **E** | **Représentation des connaissances et web sémantique** | |
| E1 | Diagnostic différentiel des maladies rares par ontologies | 3/5 |
| E2 | Valider des données cliniques FHIR avec SHACL | 3/5 |
| E3 | Éligibilité aux essais cliniques : graphe de connaissances et SPARQL | 4/5 |
| **F** | **IA symbolique hybride et argumentation** | |
| F1 | Formaliser une recommandation de bonne pratique et vérifier sa cohérence | 4/5 |
| F2 | Argumentation à valeurs pour la décision médicale partagée | 4/5 |

---

## A — Recherche heuristique et planification

### A1 — Planification temporelle d'une tournée de soins à domicile
**Difficulté : 3/5** · Mots-clés : `PDDL 2.1`, `planification temporelle`, `fenêtres de temps`, `compétences`

Des infirmiers visitent des patients à domicile : certains soins exigent une compétence particulière, d'autres doivent s'enchaîner dans un délai précis (prise de sang à jeun, puis injection). Le groupe modélise le problème comme un domaine de planification temporelle et compare un planificateur générique à une approche par contraintes.
- **MVP** : domaine et générateur d'instances PDDL 2.1 (durées, fenêtres, compétences) → résolution avec un planificateur temporel (OPTIC, TFD ou via `unified-planning`), plan vérifié.
- **Réussite** : comparaison sur des instances de taille croissante avec un modèle CP-SAT équivalent : temps de résolution, qualité (durée totale, attente des patients), expressivité.
- **Extensions** : replanification quand un soin dure plus que prévu, préférences des patients, équité de charge entre soignants.
- **Faisabilité** : instances synthétiques. **Différenciation** : les tournées de véhicules classiques (VRP) et les plannings infirmiers ont déjà été traités ; ici, le verrou est la **planification temporelle avec actions durées et dépendances**, pas l'affectation de postes. Plan B : CP-SAT seul avec intervalles optionnels.

**CoursIA** : [Planners-8-Temporal](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/Planners/03-Advanced/Planners-8-Temporal.ipynb) · [Planners-11-Unified-Planning](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/Planners/04-NeuroSymbolic/Planners-11-Unified-Planning.ipynb) · [CSP-4-Scheduling](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Part2-CSP/CSP-4-Scheduling.ipynb)
**Références** : Fox & Long, *PDDL2.1: An Extension to PDDL for Expressing Temporal Planning Domains*, JAIR, 2003 · Fikar & Hirsch, *Home health care routing and scheduling: A review*, Computers & OR, 2017.

### A2 — Robots de livraison de médicaments à l'hôpital : recherche multi-agents
**Difficulté : 4/5** · Mots-clés : `MAPF`, `Conflict-Based Search`, `A*`, `coordination`

Plusieurs robots livrent médicaments et prélèvements dans les couloirs d'un hôpital sans jamais se percuter. Le groupe implémente un algorithme de recherche de chemins multi-agents optimal et le compare à des approches priorisées.
- **MVP** : grille d'un étage d'hôpital (couloirs, ascenseurs, zones interdites) → A* indépendant avec priorités vs Conflict-Based Search (CBS).
- **Réussite** : taux de succès, somme des coûts et temps de calcul en fonction du nombre de robots ; vérificateur d'absence de collision (sommets et arêtes).
- **Extensions** : livraisons urgentes prioritaires, tâches qui arrivent en continu (*lifelong MAPF*), ECBS sous-optimal borné.
- **Faisabilité** : cartes du benchmark MAPF de Moving AI réutilisables. Plan B : planification priorisée seule, avec analyse de ses échecs.

**CoursIA** : [Frontieres-03-MAPF-Guarantee-Audit](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Part5-Frontieres/Frontieres-03-MAPF-Guarantee-Audit-Python.ipynb) · [Search-03-Informed](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Part1-Foundations/Search-03-Informed.ipynb)
**Références** : Sharon et al., *Conflict-based search for optimal multi-agent pathfinding*, Artificial Intelligence, 2015 · [MAPF benchmarks (Moving AI)](https://movingai.com/benchmarks/mapf.html).

### A3 — Itinéraires accessibles en fauteuil roulant : A* multicritère
**Difficulté : 3/5** · Mots-clés : `recherche multicritère`, `front de Pareto`, `OpenStreetMap`, `accessibilité`

Le plus court chemin n'est pas le meilleur pour une personne en fauteuil roulant : pentes, bordures, revêtements et escaliers comptent. Le groupe calcule l'ensemble des itinéraires non dominés entre deux points d'un quartier réel.
- **MVP** : graphe piéton OpenStreetMap (`osmnx`) enrichi des attributs d'accessibilité et de l'altitude → A* multi-objectif (distance, dénivelé, obstacles), front de Pareto affiché sur une carte.
- **Réussite** : comparaison avec la somme pondérée (solutions manquées) ; temps de calcul selon la taille du quartier ; cas d'étude autour d'un hôpital.
- **Extensions** : profils d'utilisateurs, données manquantes traitées de façon pessimiste, heuristiques admissibles multi-critères.
- **Faisabilité** : données ouvertes, attributs OSM parfois incomplets à documenter. Plan B : deux critères seulement.

**CoursIA** : [Search-03-Informed](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Part1-Foundations/Search-03-Informed.ipynb) · [Search-02b-NetworkX](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Part1-Foundations/Search-02b-NetworkX.ipynb) · [Search-03d-WeightedAstar](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Part1-Foundations/Search-03d-WeightedAstar.ipynb)
**Références** : Mandow & Pérez de la Cruz, *Multiobjective A* search with consistent heuristics*, JACM, 2010 · [OSMnx](https://osmnx.readthedocs.io/).

### A4 — Parcours de soins par planification hiérarchique (HTN)
**Difficulté : 3/5** · Mots-clés : `HTN`, `décomposition de tâches`, `parcours de soins`, `explicabilité`

Un parcours de soins (par exemple après un AVC ou pour une prothèse de hanche) se décrit naturellement comme une hiérarchie de tâches : bilan, imagerie, chirurgie, rééducation. Le groupe encode ces méthodes dans un planificateur HTN et produit des plans personnalisés et justifiés.
- **MVP** : domaine HTN (`pyhop`/`GTPyhop` ou `unified-planning`) décrivant un parcours inspiré d'un guide public, avec méthodes alternatives selon l'état du patient → plans générés pour plusieurs profils.
- **Réussite** : couverture des cas prévus, temps de planification, comparaison avec une planification classique sans hiérarchie (taille de l'espace, lisibilité du plan) ; trace de décomposition exposée comme explication.
- **Extensions** : contraintes de ressources, replanification après un événement, vérification que chaque plan respecte des règles de sécurité.
- **Faisabilité** : modèle de taille contrôlée. Plan B : planificateur HTN écrit à la main en Python sur un parcours simplifié.

**CoursIA** : [Planners-9-HTN](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/Planners/03-Advanced/Planners-9-HTN.ipynb) · [Planners-6-Domains](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/Planners/02-Classical/Planners-6-Domains.ipynb)
**Références** : Nau et al., *SHOP2: An HTN Planning System*, JAIR, 2003 · Ghallab, Nau & Traverso, *Automated Planning and Acting*, 2016 (bibliothèque du cours).

---

## B — Programmation par contraintes et optimisation combinatoire

### B1 — Planification des séances de radiothérapie
**Difficulté : 3/5** · Mots-clés : `CP-SAT`, `ordonnancement`, `machines parallèles`, `urgences`

Un traitement de radiothérapie comprend souvent 20 à 35 séances quotidiennes sur un accélérateur compatible, avec un délai maximal avant la première séance selon l'urgence. Le groupe planifie plusieurs semaines de séances pour un service hospitalier.
- **MVP** : générateur d'instances (accélérateurs, compatibilités, protocoles, priorités) → modèle CP-SAT : séances consécutives les jours ouvrés, même machine, créneaux horaires stables, délai d'attente par priorité.
- **Réussite** : comparaison avec une règle « premier arrivé, premier servi » : délais d'attente par catégorie, taux d'occupation ; vérificateur indépendant.
- **Extensions** : horizon glissant avec arrivées en ligne, panne de machine, préférences horaires des patients.
- **Faisabilité** : OR-Tools est suffisant. **Différenciation** : la planification de blocs opératoires et de plannings du personnel a déjà été traitée ; ici, le verrou est la **contrainte de séries de séances consécutives** sur des machines compatibles. Plan B : une seule machine.

**CoursIA** : [CSP-4-Scheduling](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Part2-CSP/CSP-4-Scheduling.ipynb) · [App-4-JobShopScheduling](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Applications/CSP/App-4-JobShopScheduling.ipynb) · [Planners-7-OR-Tools](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/Planners/03-Advanced/Planners-7-OR-Tools.ipynb)
**Références** : Vieira et al., *Operations research for resource planning and -use in radiotherapy: a literature review*, BMC Med. Inform. Decis. Mak., 2016.

### B2 — Logistique du don du sang : collectes et péremption
**Difficulté : 3/5** · Mots-clés : `MILP`, `gestion de stock`, `péremption`, `compatibilités ABO`

Les globules rouges se conservent 42 jours, les plaquettes 7 jours, et les groupes sanguins ne sont pas tous interchangeables. Le groupe optimise le calendrier des collectes mobiles et les substitutions entre groupes pour limiter à la fois pénuries et pertes.
- **MVP** : modèle multi-périodes (stocks par groupe et par âge, demande simulée, compatibilités ABO/Rh) → programmation linéaire en nombres entiers (OR-Tools ou PuLP).
- **Réussite** : comparaison avec une politique « premier entré, premier sorti » sans substitution : pénuries, péremptions, substitutions ; sensibilité à la variabilité de la demande.
- **Extensions** : placement des collectes mobiles sur une carte, demande stochastique (scénarios), plaquettes à durée de vie courte.
- **Faisabilité** : données agrégées publiques (EFS, distributions des groupes) et demande synthétique. Plan B : un seul produit sanguin.

**CoursIA** : [CSP-5-Optimization](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Part2-CSP/CSP-5-Optimization.ipynb) · [Search-09-LinearProgramming](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Part1-Foundations/Search-09-LinearProgramming.ipynb)
**Références** : Osorio, Brailsford & Smith, *A structured review of quantitative models in the blood supply chain*, Int. J. Prod. Res., 2015.

### B3 — Dépistage groupé : concevoir les pools de tests
**Difficulté : 4/5** · Mots-clés : `group testing`, `SAT`, `conception combinatoire`, `décodage`

Mélanger plusieurs prélèvements dans un même test économise des réactifs, mais il faut ensuite identifier les positifs. Le groupe conçoit des plans de mélange et des décodeurs, puis mesure le gain réel selon la prévalence.
- **MVP** : simulateur de prévalence et de tests (éventuellement bruités) → plans de Dorfman et matriciels vs plan non adaptatif conçu par contraintes (matrice *d*-disjointe), décodage par SAT ou ILP.
- **Réussite** : nombre de tests par individu, erreurs de décodage, en fonction de la prévalence ; comparaison aux bornes théoriques.
- **Extensions** : tests imparfaits (sensibilité/spécificité), plans adaptatifs à plusieurs étapes, contraintes de laboratoire (taille maximale des pools).
- **Faisabilité** : simulation pure. Plan B : comparaison analytique Dorfman/matriciel et décodeur simple.

**CoursIA** : [Z3-01-Introduction-Python](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/SMT/Z3-API/Z3-01-Introduction-Python.ipynb) · [Frontieres-05-CoveringArrays-Guarantee-Audit](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Part5-Frontieres/Frontieres-05-CoveringArrays-Guarantee-Audit-Python.ipynb)
**Références** : Dorfman, *The detection of defective members of large populations*, Ann. Math. Stat., 1943 · Aldridge, Johnson & Scarlett, *Group Testing: An Information Theory Perspective*, 2019.

### B4 — Gestion des lits et isolement infectieux
**Difficulté : 3/5** · Mots-clés : `affectation`, `contraintes d'isolement`, `replanification`, `CP-SAT`

Chaque jour, des patients arrivent, partent ou changent de service ; certains doivent être isolés (chambre seule, cohorte de patients porteurs du même germe). Le groupe affecte les patients aux chambres en minimisant les transferts et les refus.
- **MVP** : instances inspirées du benchmark *Patient Admission Scheduling* → modèle CP-SAT avec contraintes de sexe, d'équipement, d'isolement et de cohorte.
- **Réussite** : comparaison avec un glouton : patients non placés, transferts, respect des isolements ; vérificateur indépendant.
- **Extensions** : horizon glissant jour après jour, recherche locale à grand voisinage (LNS), coût des transferts pour le personnel.
- **Faisabilité** : benchmark public de Demeester et al. disponible. Plan B : un seul service, un seul jour.

**CoursIA** : [CSP-3-Advanced](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Part2-CSP/CSP-3-Advanced.ipynb) · [CSP-5-Optimization](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Part2-CSP/CSP-5-Optimization.ipynb)
**Références** : Demeester et al., *A hybrid tabu search algorithm for automatically assigning patients to beds*, Artif. Intell. Med., 2010.

### B5 — Qui tester en priorité ? Optimisation sur un graphe de contacts
**Difficulté : 4/5** · Mots-clés : `optimisation combinatoire`, `graphes`, `ILP`, `glouton sous-modulaire`

Avec un nombre limité de tests ou de vaccins, quels individus cibler sur un réseau de contacts pour réduire le plus la propagation ? Le groupe formule ce choix comme un problème d'optimisation combinatoire et compare des méthodes exactes et approchées.
- **MVP** : graphe de contacts public (SocioPatterns : hôpital, école) → sélection de *k* nœuds par ILP (couverture des voisinages), glouton et heuristiques de centralité.
- **Réussite** : réduction de la propagation mesurée par simulation (SIR) pour chaque méthode ; qualité vs temps de calcul.
- **Extensions** : budget différencié (tests, isolements), graphe temporel, garanties du glouton sous-modulaire.
- **Faisabilité** : jeux SocioPatterns librement téléchargeables. **Différenciation** : une simulation épidémique multi-agents a déjà été réalisée ; ici, la simulation n'est qu'un **outil d'évaluation**, le verrou est l'**optimisation combinatoire** du choix des cibles. Plan B : heuristiques de centralité seules.

**CoursIA** : [Search-09-LinearProgramming](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Part1-Foundations/Search-09-LinearProgramming.ipynb) · [Search-04-LocalSearch](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Part1-Foundations/Search-04-LocalSearch.ipynb)
**Références** : [SocioPatterns](http://www.sociopatterns.org/datasets/) · Kempe, Kleinberg & Tardos, *Maximizing the spread of influence through a social network*, KDD, 2003.

---

## C — Métaheuristiques et calcul évolutionnaire

### C1 — Implanter des défibrillateurs publics : couverture maximale
**Difficulté : 3/5** · Mots-clés : `couverture maximale`, `algorithme génétique`, `recuit simulé`, `open data`

Lors d'un arrêt cardiaque, chaque minute sans défibrillation réduit fortement les chances de survie. Le groupe propose l'emplacement de nouveaux défibrillateurs dans une ville pour couvrir le plus de population à moins de quelques minutes de marche, en tenant compte des appareils existants.
- **MVP** : base nationale Géo'DAE (défibrillateurs existants) + population carroyée INSEE pour une ville → problème de couverture maximale (MCLP) résolu par algorithme génétique et par recuit simulé.
- **Réussite** : comparaison avec la solution exacte ILP sur de petites instances et avec un placement glouton ; gain de couverture par appareil ajouté.
- **Extensions** : horaires d'accessibilité des bâtiments, distance réelle sur le réseau piéton, multi-objectif coût/couverture.
- **Faisabilité** : toutes les données sont ouvertes (data.gouv.fr). Plan B : distances à vol d'oiseau.

**CoursIA** : [Search-05-GeneticAlgorithms](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Part1-Foundations/Search-05-GeneticAlgorithms.ipynb) · [Search-11-Metaheuristics](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Part1-Foundations/Search-11-Metaheuristics.ipynb)
**Références** : [Géo'DAE](https://www.data.gouv.fr/fr/datasets/geo-dae-base-nationale-des-defibrillateurs/) · Church & ReVelle, *The maximal covering location problem*, Papers of the Regional Science Association, 1974.

### C2 — Optimisation multi-objectif des faisceaux en radiothérapie
**Difficulté : 4/5** · Mots-clés : `NSGA-II`, `front de Pareto`, `dosimétrie simplifiée`

Un plan de radiothérapie doit délivrer une forte dose à la tumeur tout en épargnant les organes voisins : ces objectifs sont contradictoires. Le groupe choisit angles et poids de faisceaux sur un fantôme 2D et présente au clinicien un front de compromis plutôt qu'une solution unique.
- **MVP** : fantôme 2D (tumeur, organes à risque) et modèle de dose simplifié → NSGA-II (`pymoo`) sur angles et intensités, front de Pareto couverture/dose aux organes.
- **Réussite** : comparaison avec une somme pondérée et avec des angles équirépartis ; hypervolume ; histogrammes dose-volume des solutions choisies.
- **Extensions** : contraintes dures de dose maximale, plus de deux objectifs, recherche locale hybride.
- **Faisabilité** : modèle de dose volontairement simplifié et documenté comme tel. **Différenciation** : sujet distinct de B1 (planification des séances) et des protocoles de chimiothérapie de CoursIA ; ici, le verrou est l'**optimisation multi-objectif continue et combinatoire**. Plan B : angles fixes, optimisation des poids seulement.

**CoursIA** : [Search-11b-Metaheuristiques-Deep](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Part1-Foundations/Search-11b-Metaheuristiques-Deep.ipynb) · [MGS-05-CompoundMetaheuristics](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Part4-Metaheuristics/MGS-05-CompoundMetaheuristics.ipynb)
**Références** : Deb et al., *A fast and elitist multiobjective genetic algorithm: NSGA-II*, IEEE TEC, 2002 · Ehrgott, Güler, Hamacher & Shao, *Mathematical optimization in intensity modulated radiation therapy*, Annals of OR, 2010.

### C3 — Conception évolutionnaire de molécules candidates
**Difficulté : 4/5** · Mots-clés : `algorithme génétique`, `SELFIES`, `RDKit`, `optimisation multi-objectif`

Explorer l'espace chimique pour trouver des molécules ressemblant à des médicaments est un problème de recherche dans un espace immense. Le groupe fait évoluer des molécules représentées par des chaînes SELFIES vers des propriétés cibles.
- **MVP** : algorithme génétique sur SELFIES (mutations, croisements toujours valides) → optimisation du QED et de la facilité de synthèse (SA score) avec RDKit.
- **Réussite** : comparaison avec une recherche aléatoire et un *hill climbing* ; diversité de la population ; benchmarks GuacaMol simples.
- **Extensions** : contrainte de similarité à une molécule de départ, multi-objectif avec front de Pareto, filtres de toxicité.
- **Faisabilité** : RDKit et `selfies` s'installent par pip, calcul léger. Plan B : un seul objectif (QED).

**CoursIA** : [Search-05-GeneticAlgorithms](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Part1-Foundations/Search-05-GeneticAlgorithms.ipynb) · [MGS-01-Introduction](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Part4-Metaheuristics/MGS-01-Introduction.ipynb)
**Références** : Krenn et al., *Self-referencing embedded strings (SELFIES)*, Mach. Learn.: Sci. Technol., 2020 · Brown et al., *GuacaMol: Benchmarking Models for De Novo Molecular Design*, J. Chem. Inf. Model., 2019.

### C4 — Calibrer un modèle pharmacocinétique sans gradient
**Difficulté : 3/5** · Mots-clés : `CMA-ES`, `essaim particulaire`, `équations différentielles`, `identifiabilité`

La concentration d'un médicament dans le sang suit un modèle à compartiments dont les paramètres varient d'un patient à l'autre. Le groupe ajuste ces paramètres à des mesures bruitées avec des métaheuristiques et étudie la difficulté du paysage d'optimisation.
- **MVP** : modèle à un puis deux compartiments (`scipy.integrate`), données publiques classiques (théophylline) ou simulées → ajustement par CMA-ES, essaim particulaire et évolution différentielle.
- **Réussite** : comparaison avec un optimiseur local à gradient (multi-départs) : précision, robustesse aux minima locaux, nombre d'évaluations ; paramètres non identifiables mis en évidence.
- **Extensions** : population de patients, schéma posologique optimisé une fois le modèle calibré, bruit de mesure réaliste.
- **Faisabilité** : calcul léger. Plan B : un compartiment seulement.

**CoursIA** : [Search-11-Metaheuristics](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Part1-Foundations/Search-11-Metaheuristics.ipynb) · [Search-11c-Empirical-Algorithm-Selection](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Part1-Foundations/Search-11c-Empirical-Algorithm-Selection.ipynb)
**Références** : Hansen, *The CMA Evolution Strategy: A Tutorial*, arXiv:1604.00772 · Gabrielsson & Weiner, *Pharmacokinetic and Pharmacodynamic Data Analysis*, 2016.

---

## D — Logique, SAT/SMT et vérification formelle

### D1 — Déprescription chez le patient polymédiqué : MaxSAT et explications minimales
**Difficulté : 4/5** · Mots-clés : `MaxSAT`, `MUS`, `Z3`, `interactions médicamenteuses`

Un patient âgé prend dix médicaments ; certains interagissent, d'autres sont contre-indiqués par son état. Le groupe cherche le plus petit changement de traitement qui rend la prescription cohérente, et explique chaque conflit par un ensemble minimal de règles en cause.
- **MVP** : base de règles construite à partir d'une source publique (Thésaurus des interactions de l'ANSM, critères STOPP/START) pour un périmètre restreint → encodage Z3, conflits expliqués par noyaux insatisfaisables minimaux (MUS), correction par MaxSAT pondéré.
- **Réussite** : cas tests construits avec la réponse attendue ; minimalité des explications vérifiée ; temps de calcul selon le nombre de médicaments et de règles.
- **Extensions** : substitutions thérapeutiques, posologies et fonction rénale (arithmétique), énumération de toutes les corrections minimales.
- **Faisabilité** : périmètre volontairement réduit (une vingtaine de médicaments). **Différenciation** : les systèmes experts à règles et la vérification de prescriptions du cas OncoPlan de CoursIA existent ; ici, le verrou est la **correction minimale et l'explication par MUS/MaxSAT**. Plan B : explication par noyaux UNSAT non minimaux.

**CoursIA** : [Z3-13-UnsatCores](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/SMT/Z3-API/Z3-13-UnsatCores-Python.ipynb) · [Z3-13b-UnsatCores-MUS](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/SMT/Z3-API/Z3-13b-UnsatCores-MUS-Python.ipynb) · [Z3-06-Advanced-Optimization](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/SMT/Z3-API/Z3-06-Advanced-Optimization-Python.ipynb)
**Références** : [Thésaurus des interactions médicamenteuses (ANSM)](https://ansm.sante.fr/documents/reference/thesaurus-des-interactions-medicamenteuses-1) · Liffiton & Sakallah, *Algorithms for computing minimal unsatisfiable subsets of constraints*, JAR, 2008.

### D2 — Vérification formelle d'une pompe à perfusion
**Difficulté : 4/5** · Mots-clés : `model checking`, `automates temporisés`, `propriétés de sûreté`, `BMC`

Les pompes à perfusion ont été à l'origine d'incidents graves liés à leur logique de contrôle et à leur interface. Le groupe modélise le contrôleur d'une pompe simplifiée et vérifie formellement des propriétés de sûreté (pas de surdosage, alarme toujours déclenchée en cas d'occlusion).
- **MVP** : modèle à états du contrôleur (programmation, perfusion, pause, alarme) → vérification de propriétés par *bounded model checking* avec Z3, contre-exemples affichés.
- **Réussite** : au moins cinq propriétés formalisées, dont certaines violées par une version volontairement fautive ; contre-exemples interprétés ; passage à l'échelle selon la profondeur.
- **Extensions** : automates temporisés avec UPPAAL, erreurs de saisie de l'opérateur, induction pour des preuves non bornées.
- **Faisabilité** : s'inspirer du *Generic Infusion Pump* de la FDA, publié dans la littérature. Plan B : vérification exhaustive de l'espace d'états fini en Python.

**CoursIA** : [Z3-05-Quantifiers-Proofs](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/SMT/Z3-API/Z3-05-Quantifiers-Proofs-Python.ipynb) · [Search-10-SymbolicAutomata](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/Search/Part1-Foundations/Search-10-SymbolicAutomata.ipynb)
**Références** : Arney et al., *Generic Infusion Pump Hazard Analysis and Safety Requirements*, University of Pennsylvania, 2009 · Clarke, Grumberg & Peled, *Model Checking*, MIT Press.

### D3 — Attracteurs des réseaux de régulation génétique booléens
**Difficulté : 4/5** · Mots-clés : `réseaux booléens`, `SAT`, `attracteurs`, `biologie des systèmes`

Dans un réseau booléen, chaque gène est actif ou inactif selon une fonction logique de ses régulateurs. Les états stables du réseau (attracteurs) correspondent à des phénotypes cellulaires. Le groupe énumère ces attracteurs par SAT et étudie l'effet de mutations.
- **MVP** : réseau publié (cycle cellulaire de la levure, ou réseau de différenciation des lymphocytes T) au format standard → énumération des points fixes et cycles par SAT (Z3 ou PySAT), comparaison avec une simulation exhaustive.
- **Réussite** : attracteurs retrouvés conformes à la publication ; passage à l'échelle SAT vs énumération ; effet de *knock-out* de gènes.
- **Extensions** : recherche par SAT de la plus petite intervention qui fait atteindre un attracteur cible, mise à jour asynchrone.
- **Faisabilité** : modèles disponibles sur CellCollective et GINsim. **Différenciation** : la recherche de motifs ADN relevait de l'inférence probabiliste sur séquences ; ici, le verrou est le **raisonnement logique sur la dynamique** d'un réseau. Plan B : points fixes seulement.

**CoursIA** : [Z3-01-Introduction-Python](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/SMT/Z3-API/Z3-01-Introduction-Python.ipynb) · [Z3-03-Tactics-Python](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/SMT/Z3-API/Z3-03-Tactics-Python.ipynb)
**Références** : Dubrova & Teslenko, *A SAT-Based Algorithm for Finding Attractors in Synchronous Boolean Networks*, IEEE/ACM TCBB, 2011 · [CellCollective](https://cellcollective.org/).

---

## E — Représentation des connaissances et web sémantique

### E1 — Diagnostic différentiel des maladies rares par ontologies
**Difficulté : 3/5** · Mots-clés : `ontologies`, `OWL`, `HPO`, `similarité sémantique`

Une maladie rare se reconnaît à une combinaison de signes, rarement tous présents. Le groupe exploite les ontologies publiques des phénotypes et des maladies rares pour classer les diagnostics compatibles avec une liste de signes observés, et explique chaque classement.
- **MVP** : Human Phenotype Ontology + annotations maladies-phénotypes (Orphanet) → similarité sémantique (information content, mesure de Resnik) entre profil patient et profils de maladies, classement des candidats.
- **Réussite** : patients simulés (sous-ensembles bruités de phénotypes connus) : rang de la bonne maladie, comparaison avec un simple recouvrement de mots-clés ; explication par phénotypes partagés.
- **Extensions** : raisonnement OWL sur la hiérarchie, signes absents, question suivante la plus discriminante.
- **Faisabilité** : ontologies téléchargeables librement (`pronto`, `owlready2`). **Différenciation** : les systèmes experts à règles (épaule, diabète) ont déjà été réalisés ; ici, le verrou est le **raisonnement sur une ontologie de grande taille** et la similarité sémantique. Plan B : un sous-domaine de quelques centaines de maladies.

**CoursIA** : [SW-7b-Python-OWL](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/SemanticWeb/SW-7b-Python-OWL.ipynb) · [SW-13-Python-Reasoners](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/SemanticWeb/SW-13-Python-Reasoners.ipynb)
**Références** : [Human Phenotype Ontology](https://hpo.jax.org/) · Köhler et al., *Clinical diagnostics in human genetics with semantic similarity searches in ontologies*, AJHG, 2009.

### E2 — Valider des données cliniques FHIR avec SHACL
**Difficulté : 3/5** · Mots-clés : `RDF`, `SHACL`, `FHIR`, `qualité des données`

Les dossiers patients s'échangent au format FHIR, mais une donnée syntaxiquement correcte peut être cliniquement incohérente (grossesse chez un homme, date de décès antérieure à une prescription). Le groupe convertit des ressources FHIR en RDF et écrit des contraintes SHACL qui détectent ces incohérences.
- **MVP** : patients synthétiques Synthea exportés en FHIR → conversion RDF, bibliothèque de formes SHACL (structurelles puis cliniques), rapport de validation lisible.
- **Réussite** : erreurs injectées volontairement et taux de détection ; temps de validation selon le volume ; comparaison avec une validation par scripts ad hoc.
- **Extensions** : règles SHACL-SPARQL avancées, correction suggérée, lien vers les terminologies (SNOMED CT, LOINC).
- **Faisabilité** : Synthea et `pyshacl` sont libres. Plan B : quelques types de ressources seulement (Patient, Observation, MedicationRequest).

**CoursIA** : [SW-8-Python-SHACL](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/SemanticWeb/SW-8-Python-SHACL.ipynb) · [SW-9-Python-JSONLD](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/SemanticWeb/SW-9-Python-JSONLD.ipynb)
**Références** : [FHIR RDF](https://www.hl7.org/fhir/rdf.html) · [Synthea](https://synthetichealth.github.io/synthea/) · [pySHACL](https://github.com/RDFLib/pySHACL).

### E3 — Éligibilité aux essais cliniques : graphe de connaissances et SPARQL
**Difficulté : 4/5** · Mots-clés : `graphe de connaissances`, `SPARQL`, `critères d'éligibilité`, `logique`

Les critères d'inclusion et d'exclusion des essais cliniques sont écrits en texte libre. Le groupe en formalise une partie dans un graphe de connaissances, puis retrouve par requêtes les essais auxquels un patient synthétique serait éligible.
- **MVP** : essais d'une pathologie récupérés via l'API ClinicalTrials.gov → critères formalisés (âge, sexe, diagnostics, valeurs biologiques) en RDF, requêtes SPARQL générées pour un profil patient.
- **Réussite** : essais annotés à la main comme vérité terrain : précision et rappel de l'appariement ; analyse des critères non formalisables.
- **Extensions** : extraction assistée par LLM vérifiée par le graphe, raisonnement sur les hiérarchies de diagnostics, explication des exclusions.
- **Faisabilité** : API publique et gratuite. Plan B : formalisation manuelle de 20 à 30 essais.

**CoursIA** : [SW-4b-Python-SPARQL](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/SemanticWeb/SW-4b-Python-SPARQL.ipynb) · [SW-11-Python-KnowledgeGraphs](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/SemanticWeb/SW-11-Python-KnowledgeGraphs.ipynb)
**Références** : [ClinicalTrials.gov API](https://clinicaltrials.gov/data-api/api) · Weng et al., *Formal representation of eligibility criteria: a literature review*, J. Biomed. Inform., 2010.

---

## F — IA symbolique hybride et argumentation

### F1 — Formaliser une recommandation de bonne pratique et vérifier sa cohérence
**Difficulté : 4/5** · Mots-clés : `neuro-symbolique`, `formalisation`, `SMT`, `cohérence`

Les recommandations de bonne pratique sont des textes longs, parfois ambigus ou contradictoires. Le groupe traduit une recommandation publique en règles formelles, avec l'aide d'un LLM, puis utilise un solveur pour détecter incohérences, lacunes et redondances.
- **MVP** : une recommandation publique courte (HAS ou NICE) → règles extraites par LLM dans un format contrôlé, encodées en Z3, vérification de cohérence et de complétude sur l'espace des profils patients.
- **Réussite** : comparaison de la formalisation automatique avec une formalisation manuelle de référence (règles correctes, manquantes, fausses) ; anomalies réelles ou injectées détectées.
- **Extensions** : boucle de correction (le solveur renvoie un contre-exemple au LLM), comparaison de deux versions d'une recommandation.
- **Faisabilité** : périmètre d'une seule recommandation. **Différenciation** : la génération validée par CSP et la comparaison LLM/solveur ont été explorées ailleurs ; ici, le verrou est la **vérification de cohérence d'un texte normatif médical**. Plan B : formalisation manuelle et vérification seule.

**CoursIA** : [Z3-13b-UnsatCores-MUS](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/SMT/Z3-API/Z3-13b-UnsatCores-MUS-Python.ipynb) · [Planners-10-LLM-Planning](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/Planners/04-NeuroSymbolic/Planners-10-LLM-Planning.ipynb)
**Références** : Peleg, *Computer-interpretable clinical guidelines: A methodological review*, J. Biomed. Inform., 2013 · [Recommandations HAS](https://www.has-sante.fr/).

### F2 — Argumentation à valeurs pour la décision médicale partagée
**Difficulté : 4/5** · Mots-clés : `argumentation abstraite`, `VAF`, `préférences`, `explicabilité`

Lors d'une décision médicale partagée, les arguments pour et contre un traitement n'ont pas le même poids selon les valeurs du patient (qualité de vie, longévité, autonomie). Le groupe modélise ce débat par un cadre d'argumentation à valeurs et montre comment la décision change selon le profil de valeurs.
- **MVP** : cas clinique documenté (par ex. traitement d'un cancer de la prostate localisé : surveillance, chirurgie ou radiothérapie) → arguments et attaques, cadre de Bench-Capon, extensions calculées pour plusieurs ordres de valeurs.
- **Réussite** : décisions obtenues pour chaque profil et explication par les arguments acceptés ; vérification par un second moteur (Tweety ou implémentation propre).
- **Extensions** : élicitation des valeurs par questionnaire, sémantiques graduelles, dialogue entre médecin et patient.
- **Faisabilité** : modèle de petite taille. **Différenciation** : les sémantiques de Dung, la détection de sophismes et la génération de contre-arguments ont déjà été traitées ; ici, le verrou est l'**argumentation à valeurs** appliquée aux préférences du patient. Plan B : cadre de Dung avec préférences codées à la main.

**CoursIA** : [Argumentation-03b-Value-Based-AF](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/Argument_Analysis/Argumentation-03b-Value-Based-AF-Python.ipynb) · [Argumentation-03-Dung-AF-Semantics](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/Argument_Analysis/Argumentation-03-Dung-AF-Semantics-Python.ipynb) · [Tweety-01-Setup](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/Tweety/Tweety-01-Setup-Python.ipynb)
**Références** : Bench-Capon, *Persuasion in Practical Argument Using Value-based Argumentation Frameworks*, J. Logic Comput., 2003 · Elwyn et al., *Shared decision making: a model for clinical practice*, J. Gen. Intern. Med., 2012.
