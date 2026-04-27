# Digital Twin for DHC Optimisation

**Technical Manual & Service Specification v1.0**

**Author:** VEO
**Last updated:** 04 Feb 2026

---

## 1. Business Context & Definitions

This service focuses on integrating **AI-driven analytics with digital twin technologies** to support the optimisation of district heating network (DHC/DHN) operation.

The digital twin provides a **virtual representation of the physical heating network**, allowing operators to simulate different production and distribution scenarios without affecting real operations. Using historical and operational data, the system reproduces network behaviour and enables testing of alternative strategies in a risk-free environment.

The service supports:

* Optimisation of heat production and distribution
* Evaluation of fuel mix strategies (e.g., biomass vs gas)
* Analysis of demand variations and operational conditions
* Long-term planning for efficiency and sustainability

By combining simulation and data-driven modelling, the service improves decision-making, reduces operational risks, and enables more efficient and resilient district heating system management.

---

### Key Terms

* **Building Portfolio**
  Buildings managed or owned by municipalities or organisations.

* **Energy Consumption**
  Measured energy usage over time (kWh).

* **Energy Performance Indicator (EPI)**
  Metric (kWh/m²/year) used to assess building efficiency.

* **Clustering**
  Grouping buildings with similar characteristics and consumption behaviour.

* **Resolution / Granularity**
  Data analysed at **hourly or monthly resolution**.

* **Underperforming Buildings**
  Buildings with energy performance below cluster benchmarks.

* **Retrofit Prioritisation**
  Ranking buildings for energy efficiency improvements.

* **Outlier Detection**
  Identification of abnormal consumption patterns.

---

## 1.1 Veolia Context

Veolia develops this service using the **Torrelago district heating network** as a real operational testbed.

Although no physical digital twin is fully deployed yet, the service uses:

* Historical datasets
* Operational measurements

to simulate system behaviour and evaluate alternative operational strategies.

This approach allows:

* Analysis of energy flows
* Understanding system response under different conditions
* Testing optimisation strategies without impacting real operations

The service also prepares the foundation for:

* Full digital twin deployment
* Integration of advanced simulation technologies
* Scaling to large district heating systems

---

## 2. Problem Statement

The goal is to develop a **production-ready building analytics and digital twin service** that:

* Clusters buildings within a portfolio
* Estimates baseline energy consumption
* Detects deviations and inefficiencies
* Simulates operational scenarios

The service must:

* Provide results via a **REST API**
* Ensure **availability and responsiveness**
* Deliver **reproducible and traceable outputs**

---

### Outputs

* Cluster assignments
* Baseline energy profiles
* Deviation metrics
* Portfolio-level aggregates

---

### Target Variables

* Energy consumption per building
* Energy intensity (kWh/m²)
* Time-based consumption patterns

---

### Constraints

* Only **historical and available data** can be used (no future leakage)
* Models must ensure:

  * Causality
  * Reproducibility
  * Version tracking

---

## 3. Data Description

The service uses three main datasets:

1. **Building metadata**
2. **Historical energy bills**
3. **Real-time energy consumption data**

---

## 3.1 Building Metadata

| Variable      | Variable name | Type    | Unit | Description       | Example     |
| ------------- | ------------- | ------- | ---- | ----------------- | ----------- |
| Building name | name          | String  | -    | Name of building  | Town Hall   |
| Address       | address       | String  | -    | Building address  | Athinas 63  |
| Latitude      | latitude      | String  | -    | GPS latitude      | 37.973      |
| Longitude     | longitude     | String  | -    | GPS longitude     | 23.706      |
| Category      | category      | String  | -    | Building type     | Services    |
| Total area    | total_area    | Float   | m²   | Building area     | 105.5       |
| Built year    | built_year    | Integer | -    | Construction year | 1990        |
| Supply number | supply_number | String  | -    | Energy supply ID  | 11111111111 |

---

## 3.2 Historical Energy Bills

| Variable              | Variable name               | Type    | Unit       | Description      | Example    |
| --------------------- | --------------------------- | ------- | ---------- | ---------------- | ---------- |
| Bill number           | bill_number                 | Integer | -          | Bill ID          | 1111111111 |
| Issue date            | bill_issue_date             | Date    | YYYY-MM-DD | Issue date       | 28/02/2023 |
| Meter reading date    | meter_reading_date          | Date    | YYYY-MM-DD | Reading date     | 28/02/2023 |
| Previous reading date | previous_meter_reading_date | Date    | YYYY-MM-DD | Previous reading | 28/02/2023 |
| Energy                | bill_kWh                    | Integer | kWh        | Energy consumed  | 500        |
| Cost                  | bill_total_euros            | Float   | €          | Total cost       | 45.5       |
| Supply number         | supply_number               | String  | -          | Supply ID        | 1111111111 |
| Bill type             | bill_type                   | String  | -          | Type             | Settlement |

---

## 3.3 Real-Time Energy Consumption

| Variable      | Variable name         | Type      | Unit             | Description        | Example    |
| ------------- | --------------------- | --------- | ---------------- | ------------------ | ---------- |
| Building name | building_name         | String    | -                | Building name      | Town Hall  |
| Supply number | supply_number         | String    | -                | Supply ID          | 1111111111 |
| Timestamp     | timestamp_consumption | Timestamp | YYYY-MM-DD HH:MM | Consumption time   | 2023-02-28 |
| Consumption   | consumnption          | Float     | kWh              | Energy consumption | 1.2        |

---

## 4. Analytics, Scope & Update Frequency

### Temporal Scope

* Rolling historical windows (typically **up to 12 months**)
* Adaptable depending on data availability
* Continuous updates with new data

---

### Update Frequency

* **Monthly updates**
* On-demand recalculation after:

  * Operational changes
  * Retrofit actions

---

### Methodology and Technical Approach

The service follows a **hybrid digital twin approach**:

#### 1. Historical Data Analysis

* Identify system behaviour patterns
* Analyse:

  * Consumption
  * Temperature relationships
  * System dynamics

#### 2. Data-Driven Modelling

* Build models mapping:

  * Inputs → demand, weather
  * Outputs → consumption, performance

#### 3. Integration with Forecasting

* Uses outputs from **Heating Demand Forecasting**
* Enables future scenario simulation

#### 4. Scenario-Based Simulation

Simulates:

* Supply temperature changes
* Load distribution strategies
* Control strategies

#### 5. Continuous Updating

* Models updated as new data arrives
* Ensures alignment with real system behaviour

---

### Output Format

For each building:

* Cluster assignment + description
* Energy baseline
* Deviation score
* Inefficiency indicator
* Retrofit priority score

---

### Additional Outputs

* Scenario simulation results
* Performance indicators:

  * Efficiency
  * Losses
  * System performance
* Time-series simulation profiles
* Optimisation recommendations
* Scenario-based insights

---

## 5. Evaluation Protocols & Metrics

Evaluation ensures:

* Reliability
* Accuracy
* Operational usefulness

---

## 5.1 Data Usage & Analytical Protocol

* Rolling window up to **12 months**
* Only available data used
* Adaptation for limited data
* Full reproducibility required

---

## 5.2 Data Gaps and Exceptions

* Missing data excluded
* Poor-quality buildings may be excluded
* All exclusions documented

---

## 5.3 Service Evaluation Metrics & KPIs

* **NEPD (Normalised Energy Performance Deviation)**
* **SCIB (Share of Consumption in Inefficient Buildings)**
* **CCI (Cluster Cohesion Index)**

Additional validation includes:

* Accuracy vs real system behaviour
* Consistency across scenarios
* Reliability of optimisation insights

---

## 6. Deliverables & Submissions

---

## 6.1 Deliverable Reports

### 1. Pre-Service Report

* Analytical approach
* Clustering methodology
* System architecture
* Integration plan

### 2. Interim Report

* Data coverage
* Preliminary results
* KPI performance
* Method refinements

### 3. Final Report

* Final performance
* Portfolio insights
* Optimisation recommendations
* Lessons learned

---

## 6.2 Technical Specifications & Submissions

### Service Interface Documentation

* API documentation
* Data formats
* Authentication

---

### Deployment Artefacts

* Docker or equivalent (if used)
* Alternative deployment models documented

---

### Configuration & Handover

* Model versioning
* Configuration parameters
* Operational procedures

---

### Security & Data Protection

* Data handling policies
* Access control
* Compliance requirements

---

## Implementation & Availability

* Implemented in **Jupyter Lab environment**
* Currently operates in **offline mode**
* Uses:

  * Historical data
  * Real-time inputs (for testing)

---

### Future Integration

* Designed for integration into **EnerTEF platform**
* Modular architecture:

  * Data ingestion
  * Modelling
  * Simulation
  * Output generation

---

### Capabilities

* Simulation of operational strategies
* Risk-free testing
* Decision-support for operators
* Integration with:

  * Forecasting services
  * Optimisation tools
  * Scheduling systems

---

**The service ultimately enables a transition from reactive operation to predictive and simulation-driven optimisation of district heating networks.**
