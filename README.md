# Snake RL Generalization Study

Does a reinforcement learning agent trained on a 9×9 Snake grid learn a *transferable policy*, or does it memorize environment-specific patterns that collapse under distribution shift?

This project compares **DQN**, **DoubleDQN**, and **PPO** across spatial scaling (6×6 → 12×12), obstacle introduction (0–8 obstacles), and three state representations (local, extended, hybrid). All agents are trained exclusively on a 9×9 grid with no obstacles, then evaluated on held-out configurations.

## Key Findings

- All three agents generalize better to **larger** grids than smaller ones — a larger grid contains the training environment as a strict subset, preserving policy coverage
- Obstacle generalization shows consistent degradation with no metric reversal, narrowing the gap over random from ~10× (0 obstacles) to ~5–6× (8 obstacles)
- DQN/DoubleDQN outperform PPO in this setting due to sample efficiency, discrete action space fit, and step-wise reward structure
- Performance is largely **insensitive to feature space complexity** (local vs. hybrid), suggesting Snake's state space is simple enough for compact representations

## Repo Structure

```
├── index.html          # Self-contained interactive paper with live simulation
├── README.md
```

The HTML file is fully self-contained — all figures are embedded and the live Snake simulation (Random / Trained DQN / Hamiltonian agents on an 8×8 grid) runs in-browser with no dependencies.

## Methods

| | DQN / DoubleDQN | PPO |
|---|---|---|
| Training steps | 150,000 | 800,000 |
| Parallel envs | 8 | 16 |
| Seeds | 3 | 3 |
| Key hyperparams | lr=5e-4, γ=0.99, buffer=100k | lr=1e-4, epochs=10, reward norm |

Generalization performance is reported as **food/step normalized against a Hamiltonian cycle baseline** (for size generalization) and against a random baseline (for obstacle generalization), to correct for grid-geometry artifacts in raw food/step comparisons.

## References

- Almalki & Wocjan (2019) — DQN/SARSA in Snake
- Cobbe et al. (2019) — CoinRun & Procgen generalization benchmarks
- Narvekar et al. (2020) — Curriculum learning survey
- Weinberger, Liang & Chen (2024) — Hamiltonian cycle baseline in Snake
