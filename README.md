# Introduction 

As part of the Managerial Decision Making and Modeling course at Ca’ Foscari University of Venice, a closed multi-vehicle routing problem (CVRP) inspired by the San Polo di Piave facility of O-I, located in Treviso (Italy), was developed. The objective was to determine the shortest possible routes to serve all customers included in the analysis, minimizing total travel distance and, consequently, operational costs and delivery time. Google OR-Tools was used for modeling and optimization, and the routing solutions were visualized using QGIS.

# Data 
The dataset was constructed specifically for this study, based on general operational insights obtained about the facility.

### Dataset Description 
The dataset includes 20 customers (nodes) and one depot, corresponding to the O-I San Polo di Piave facility located in Treviso (Italy). For each node, the following attributes are reported: 
* **CustomerID**, where the depot is identified as `ID_0`
* **Longitude**
* **Latitude**
* **City**, one of: Treviso, Padua, Vicenza, Vewnice or Verona 
* **Demand**, which is the daily demand in pallets 
* **Net Profit**, which is the daily net profit computed as: $\text{daily demand} \cdot (\text{selling price per pallet} - (\text{production cost per pallet} + \text{transportation cost per pallet}))$


### Assumptions for Data Construction 
#### Wine Production 
* Large wineries: approximately 150,000 bottles per day
* Small wineries: approximately 4 million bottles per day
#### Pallet and Pricing 
* 1 pallet = 1,000 bottles 
* Price of bottles ≈ €1.50
* Selling price per pallet ≈ €150
#### Costs and Profitability
* Target profit margin: 25-30% of total costs
* Production cost per pallet: €80, approximately 53% of the selling price, consistent with the glass production industry
#### Transportation Costs (per pallet per day)
* Treviso: €5
* Venice, Padua, Vicenza: €10
* Verona: €15 

# Methodology 

Several solution strategies were implemented.

#### 1. Heuristic Approach
An initial feasible solution was implemented using a **nearest neighbour constructive heuristic**, implemented in OR-Tools via the `PATH_CHEAPEST_ARC` strategy. The first solution was then refined using the `GUIDED_LOCAL_SEARCH` metaheuristic to escape poor local optima and reduce total travel distance by encouraging the solver to explore alternatives.

#### 2. Metaheuristic Strategies
Starting from the first solution, additional improvement strategies available in OR-Tools were used, including:
* `TABU_SEARCH`
* `SIMULATED_ANNEALING`
* `AUTOMATIC`
* `GREEDY_DESCENT`
#### 3. Cluster-first, Route-second Model
A **cluster-first, route-second approach** was also explored. Customers were first grouped into clusters based on geographical proximity, after which the nearest neighbour heuristic was applied within each cluster to generate routes. The procedure was then repeated using demand-based clustering, enabling a comparison between spatial and demand-driven solutions.

# Key Findings 

# Conclusion 
