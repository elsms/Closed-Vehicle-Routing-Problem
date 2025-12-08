# Closed Vehicle Routing Problem

As part of the Managerial Decision Making and Modeling course at Ca' Foscari University of Venice, we developed a closed multi-vehicle routing problem (CVRP) inspired by the San Polo di Piave facility of O-I, located in Treviso. The dataset used in our analysis was constructed by us, based on general operational insights we received about the facility.

Our objective was to determine the shortest possible routes to serve all customers included in the analysis, minimizing total travel distance and, consequently, operational costs and delivery time. We used Google OR-Tools for modeling and optimization, and visualized the routing solutions using QGIS.

We found several solutions.

Heuristic approach: We first used the Nearest Neighbour method, adopting a constructive approach with PATH_CHEAPEST_ARC to generate an initial solution. We then improved it using the GUIDED_LOCAL_SEARCH metaheuristic.
Metaheuristic strategies: We further enhanced the initial solution using various built-in metaheuristics available in OR-Tools, including TABU_SEARCH, SIMULATED_ANNEALING, AUTOMATIC, and GREEDY_DESCENT.
Cluster-first, Route-second model: Finally, we implemented a clustering strategy by grouping customers based on geographical proximity. We then applied the Nearest Neighbour method again using OR-Tools. We repeated this approach by clustering based on demand as well.
