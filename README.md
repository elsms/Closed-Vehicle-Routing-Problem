
[![Google Colab](https://img.shields.io/badge/Google%20Colab-Notebook-orange)](Project.ipynb)
[![Google OR-Tools](https://img.shields.io/badge/Google_OR--Tools-Optimization-pink)](https://developers.google.com/optimization/routing/cvrp?_gl=1*70op1f*_up*MQ..*_ga*OTczOTE2ODM0LjE3NzA4MzU4MzQ.*_ga_SM8HXJ53K2*czE3NzA4MzU4MzQkbzEkZzAkdDE3NzA4MzU4MzQkajYwJGwwJGgw)
[![QGIS](https://img.shields.io/badge/QGIS-Visualization-green)](https://qgis.org/download/)

# Introduction 
As part of the [Managerial Decision Making and Modeling](https://www.unive.it/data/course/506446) course at [Ca’ Foscari University of Venice](https://www.unive.it/web/en/497/home), a closed multi-vehicle routing problem (CVRP) inspired by the San Polo di Piave facility of O-I, located in Treviso (Italy), was developed. 

The objective was to determine the shortest possible routes to serve all customers included in the analysis, minimizing total travel distance and, consequently, operational costs and delivery time. Google OR-Tools was used for modeling and optimization, and the routing solutions were visualized using QGIS.

# Data 
The dataset was constructed specifically for this study, based on general operational insights obtained about the facility.

### Dataset Description 
The [dataset](Data) includes 20 customers (nodes) and one depot, corresponding to the O-I San Polo di Piave facility located in Treviso (Italy). For each node, the following attributes are reported: 
* **CustomerID**, where the depot is identified as `ID_0`
* **Longitude**
* **Latitude**
* **City**, one of: Treviso, Padua, Vicenza, Venice or Verona 
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
The goal of the analysis is to determine the optimal routes that serve all customers while minimizing total travel distance, computed using OpenStreetMap (OSM) data. To ensure feasibility and robustness, several modeling considerations are required.
* High-demand customers whose requirements exceed a single vehicle’s capacity are split into **virtual nodes** with smaller demands. This allows Google OR-Tools to allocate multiple vehicles to serve those customers without violating capacity constraints.
* **Subtour elimination** is handled automatically by Google OR-Tools by enforcing that all vehicles return to the depot after completing their assigned routes. Multi-trips are not permitted, meaning vehicles cannot return to the depot to reload once dispatched.

Several solution strategies are implemented to evaluate and improve routing performance. 

#### 1. Heuristic Approach
An initial feasible solution is implemented using a **nearest neighbour constructive heuristic**, implemented in OR-Tools via the `PATH_CHEAPEST_ARC` strategy. The first solution is then refined using the `GUIDED_LOCAL_SEARCH` metaheuristic to escape poor local optima and reduce total travel distance by encouraging the solver to explore alternatives. 

#### 2. Metaheuristic Strategies
Starting from the first solution, additional improvement strategies available in OR-Tools are used, including:
* `TABU_SEARCH`
* `SIMULATED_ANNEALING`
* `AUTOMATIC`
* `GREEDY_DESCENT`

#### 3. Cluster-first, Route-second Model
A **cluster-first, route-second approach** is also explored. Customers are first grouped into clusters based on geographical proximity, after which the nearest neighbour heuristic is applied within each cluster to generate routes. The procedure is then repeated using demand-based clustering, enabling a comparison between spatial and demand-driven solutions.

# Key Findings 

* The initial heuristic solution serves all customers with a total travel distance of 9,184 km while satisfying the total demand of 1,517 pallets. Applying the `GUIDED_LOCAL_SEARCH` improvement reduces the total travel distance to 8,971 km. This improvement does not result from reduced service coverage, but rather from smarter route optimization and more efficient use of vehicle capacity. 
* The metaheuristic strategy `TABU_SEARCH` produces the same result as `GUIDED_LOCAL_SEARCH`. The remaining metaheuristic approaches (* `SIMULATED_ANNEALING`, `AUTOMATIC` and `GREEDY_DESCENT`) return the same result as `PATH_CHEAPEST_ARC`.
* Clustering by proximity delivers all pallets with a total travel distance of 9,180 km, nearly identical to the 9,184 km observed using the initial nearest-neighbor heuristic. Clustering by demand consistently returns the same result as the first clustering attempt.

The visualization in QGIS is available in the [folder](Visualization-in-QGIS).

# Conclusions
Among all strategies tested, the `GUIDED_LOCAL_SEARCH` metaheuristic solution achieves the lowest total travel distance of 8,971 km while serving all customers and meeting the full demand. This makes it the **optimal routing solution**. 
<div align="center">
<img width="900" height="479" alt="image" src="https://github.com/user-attachments/assets/f4db7abc-b679-4477-857e-6b6a6cc01a8d" />
</div>



