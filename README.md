# Reinforcement Learning for Dynamic Difficulty Adjustment in Turn-Based Battle Games

## Overview

This project implements and evaluates a reinforcement learning (RL) agent using SARSA to perform Dynamic Difficulty Adjustment (DDA) in a turn-based battle game. The work reproduces and extends concepts from Pagalyte et al., "Go with the Flow: Reinforcement Learning in Turn-Based Battle Video Games" (IVA 2020).

The goal is to explore whether reinforcement learning, combined with reward-sign inversion, can produce an agent that maintains balanced and engaging gameplay by adapting difficulty in real time.

The project includes:

- A fully implemented turn-based battle environment
- A human-like player model
- Multiple competing agent types:
  - SARSA + DDA (ε = 0.3)
  - SARSA + DDA (ε = 0.5)
  - SARSA without DDA
  - Scripted Easy agent
  - Scripted Hard agent
- Experiments over 3000 battles per configuration
- Detailed analysis of win rates, pacing, and action-selection behavior

## Environment Summary

### Game Rules

- Both players start at 75 HP
- Actions available:
  - **Hit** (light attack)
  - **Charge** (heavy attack)
  - **Combo** (multi-hit randomized attack)
  - **Heal** (+10 HP)

### State Representation

Health is grouped into 5 bands:

- 75
- 51–74
- 31–50
- 16–30
- 1–15

These produce a 17-state model indicating health comparison and terminal states.

### Turn Order

1. Player acts
2. Player action resolves
3. Agent acts
4. Agent action resolves
5. State updates

## Agent Types

### 1. Human-Like Player

A rule-based stochastic model mimicking human behavior:

- Prefers Charge and Combo
- Heals when low HP
- Occasionally uses Hit

### 2. SARSA with DDA

- On-policy SARSA
- Reward sign "flips" after player or agent streaks (±5 wins)
- Encourages the agent to become easier or harder depending on player performance
- Exploration rate:
  - 0.3 (30/70)
  - 0.5 (50/50)

### 3. SARSA without DDA

- Identical SARSA configuration
- No reward swapping
- Used as an ablation baseline

### 4. Easy Scripted Agent

- Defensive and predictable
- Frequent healing
- Minimal damage output

### 5. Hard Scripted Agent

- Aggressive
- High Combo and Charge usage
- Rare healing

## Experimental Setup

- 3000 battles per agent configuration
- Metrics collected:
  - Player and agent win rates
  - Average battle length
  - Action distributions
  - Policy behavior
  - Streak patterns (important for DDA)
- All agents were tested against the same human-like player for consistency.

## Key Results (3000-Battle Evaluation)

| Agent Type | Player Win % | Agent Win % | Avg Battle Length |
|------------|--------------|-------------|-------------------|
| SARSA + DDA (30/70) | 60.1% | 39.9% | 10.58 |
| SARSA + DDA (50/50) | 67.0% | 33.0% | 10.67 |
| SARSA No DDA | 55.9% | 44.1% | 9.89 |
| Fixed Easy | 98.7% | 1.3% | 12.14 |
| Fixed Hard | 61.9% | 38.1% | 9.27 |

### Interpretation

- All SARSA variants maintain balanced, interactive gameplay
- DDA smooths out win/loss streaks and stabilizes pacing
- Easy agent performs trivially; Hard agent performs aggressively but predictably
- RL agents show coherent strategy: high healing, mixed offensive actions

## Project Structure

```
project_root/
│
├── results/
│   ├── metrics/
│   ├── plots/
│   └── logs/
│
├── README.md  ← (this file)
└── RLinTBGames.ipynb  ← full runnable notebook
```

## How to Run

### Notebook Version 

Open:
```
main.ipynb
```

and run all cells.

### Dependencies

- Python 3.9+
- NumPy
- Matplotlib
- Collections
- Random

Install with:
```bash
pip install numpy matplotlib
```

## Visualizations

Plots include:

- Win/loss trends over episodes
- Action distributions per agent
- Sample HP progression charts
- Comparison tables

Each figure includes descriptive captions in the report.

## Summary of Findings

- SARSA learns stable and coherent strategies
- DDA influences balance without destabilizing behavior
- Higher exploration (50/50) leads to smoother long-term performance
- Scripted agents validate expected difficulty boundaries
- Results closely align with the original paper's findings

The experiment is considered complete and meets all objectives originally outlined in the project proposal.
