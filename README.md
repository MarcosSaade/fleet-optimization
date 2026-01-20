# Fleet Optimization for Transportation

## 📋 Overview

This project implements a **stochastic optimization model** to determine the optimal fleet configuration for my university's transportation service. Using **Monte Carlo simulation** and **queueing theory**, the system analyzes different vehicle combinations to minimize operational costs while maintaining acceptable passenger waiting times, based on real data.

### Key Features

- **Monte Carlo Simulation**: 50 simulations per configuration to capture stochastic variability
- **Non-homogeneous Poisson Process**: Models realistic passenger arrival patterns throughout the day
- **Empirical Data Integration**: Uses real bus arrival data from different time periods
- **Multi-objective Optimization**: Balances cost minimization and service quality
- **Comprehensive Analytics**: Detailed visualizations and performance metrics

## 🎯 Problem Statement

**Objective**: Find the optimal combination of vehicles to minimize costs while keeping average passenger waiting time under 10 minutes.

### Available Vehicles

| Vehicle | Capacity | Schedule | Cost/Hour | Available | Priority |
|---------|----------|----------|-----------|-----------|----------|
| Bus A | 37 seats | 6:00-17:00 (11h) | $42.6/h | 1 | 1 (Highest) |
| Bus B | 31 seats | 8:00-14:00 (6h) | $42.6/h | 2 | 3 |
| Van A | 19 seats | 6:00-20:00 (14h) | $24.8/h | 1 | 4 (Lowest) |
| Van B | 13 seats | 7:00-22:00 (15h) | $24.8/h | 3 | 2 |

### Constraints

- **Service Coverage**: Must provide continuous service from 6:00 AM to 10:00 PM
- **Maximum Waiting Time**: Average passenger waiting time < 10 minutes
- **Vehicle Availability**: Limited to maximum available quantities

## 🚀 Getting Started

### Prerequisites

```bash
python >= 3.8
numpy
pandas
matplotlib
seaborn
```

### Installation

1. Clone the repository:
```bash
git clone https://github.com/MarcosSaade/fleet-optimization.git
cd fleet-optimization
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Run the Jupyter notebook:
```bash
jupyter notebook optimizacion_flota.ipynb
```

## 📊 Methodology

### 1. Passenger Arrival Modeling

Passengers arrive following a **non-homogeneous Poisson process** with time-varying rates:

- **6:00-8:00**: λ = 3.0 min (early morning)
- **8:00-10:00**: λ = 2.0 min (morning rush)
- **10:00-13:00**: λ = 3.0 min (mid-morning)
- **13:00-17:00**: λ = 1.6 min (peak hour)
- **17:00-22:00**: λ = 2.6 min (evening)

### 2. Bus Arrival Data

Vehicle trips are generated using **empirical sampling** from real interarrival time data collected in four time periods:
- 6:00-9:00 AM
- 9:00 AM-2:00 PM
- 2:00-6:00 PM
- 6:00-10:00 PM

### 3. Monte Carlo Simulation

Each fleet configuration is tested with:
- **50 independent simulations** with different random seeds
- **Statistical analysis** of mean, standard deviation, and ranges
- **Robust evaluation** accounting for stochastic variability

### 4. Performance Metrics

For each configuration, we calculate:
- **Average waiting time** (min)
- **Maximum waiting time** (min)
- **Average queue length** (passengers)
- **Daily operational cost** ($)
- **Total trips per day**
- **Vehicle utilization** (%)

## 📈 Results

The analysis generates comprehensive visualizations including:

1. **Cost vs. Waiting Time Analysis** - Pareto frontier showing trade-offs
2. **Configuration Comparison** - Top configurations side-by-side
3. **Operations Timeline** - Hourly vehicle activity and queue evolution
4. **Vehicle Utilization** - Occupancy rates by vehicle and time
5. **Monte Carlo Variability** - Statistical robustness of results

### Optimal Configurations

The system identifies three recommended options:

- **Economic Option**: Minimizes daily cost while meeting requirements
- **Premium Option**: Minimizes waiting time for best service quality
- **Balanced Option**: Optimal cost-benefit trade-off

## 📁 Project Structure

```
fleet-optimization/
├── optimizacion_flota.ipynb    # Main Jupyter notebook
├── data/
│   ├── glaxo_6-9_full.csv      # Bus arrival data (6-9 AM)
│   ├── glaxo_9-2_full.csv      # Bus arrival data (9 AM-2 PM)
│   ├── glaxo_2-6_full.csv      # Bus arrival data (2-6 PM)
│   ├── glaxo_6-10_full.csv     # Bus arrival data (6-10 PM)
│   └── resultados_optimizacion_flota.csv  # Results export
├── plots/
│   ├── 01_costo_vs_espera.png
│   ├── 02_comparacion_configuraciones.png
│   ├── 03_timeline_operaciones.png
│   ├── 04_ocupacion_vehiculos.png
│   ├── 05_evolucion_ocupacion_horaria.png
│   ├── 06_top_configuraciones.png
│   └── 07_variabilidad_montecarlo.png
└── README.md
```

## 🔬 Technical Details

### Simulation Algorithm

```python
1. Generate all valid fleet combinations (with coverage constraint)
2. For each configuration:
   a. Run N Monte Carlo simulations
   b. For each simulation:
      - Generate passenger arrivals (Poisson process)
      - Generate vehicle trips (empirical sampling)
      - Simulate queueing system
      - Record metrics
   c. Aggregate statistics across simulations
3. Filter valid configurations (avg_wait < 10 min)
4. Rank by cost and service quality
```

### Key Classes

- **`VehicleType`**: Represents a vehicle type with capacity, schedule, and cost
- **`BusArrivalData`**: Loads and samples from empirical bus arrival data
- **`FleetSimulator`**: Simulates the queueing system for a given fleet configuration
- **`DetailedFleetSimulator`**: Extended simulator with detailed trip tracking


## 🛠️ Customization

To adapt this model for your use case:

1. **Update vehicle data**: Modify capacity, schedules, and costs in `VEHICLES` list
2. **Adjust arrival patterns**: Update λ parameters in `generate_passenger_arrivals()`
3. **Change constraints**: Modify coverage requirements or max waiting time threshold
4. **Add new metrics**: Extend `calculate_metrics()` method
5. **Adjust simulation parameters**: Change `N_SIMULATIONS` for more/less precision
