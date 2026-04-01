# System 3: Threshold-Based Finite State Machines

## Core Principle

A **Threshold-Based Finite State Machine (TBFSM)** is a computational model that guarantees finiteness by using only **discrete threshold transitions**. A state machine remains provably finite if and only if all state transitions are triggered by **thresholds**, not timers.

## Definition

**Threshold**: A discrete boundary value that, when crossed or reached, triggers a state transition.

**Timer**: A continuous or semi-continuous temporal measure that produces unbounded behavior.

A TBFSM formalizes the constraint:
- **All state transitions must be threshold-triggered (discrete)**
- **No timer-based or event-delay mechanisms (continuous/unbounded)**

## Mathematical Property: Guaranteed Finiteness

### Why Thresholds Guarantee Finiteness

1. **Thresholds are countable**: A threshold is a discrete boundary. Even with infinite physical precision, the number of meaningful thresholds is limited by system constraints.

2. **Timers introduce unbounded complexity**: A timer is inherently continuous/unbounded. Adding timers to an FSM means introducing a temporal dimension that violates the "finite" property.

3. **The Fundamental Constraint**: 
   - States based on thresholds = Finite discrete set
   - States based on timers = Infinite temporal states (one for each moment in time)

### The Contradiction

If you claim to have "infinite thresholds," you're not describing thresholds anymore—you're describing a timer. This is because:
- Thresholds must be enumerable and discrete
- An infinite collection of thresholds collapses into continuous measurement territory

## Key Distinction: Thresholds vs. Timers

| Aspect | Threshold | Timer |
|--------|-----------|-------|
| **Nature** | Discrete boundary | Continuous/temporal |
| **Countability** | Finite (by definition) | Unbounded |
| **Trigger mechanism** | Value comparison | Time elapsed |
| **State contribution** | Finite state space | Infinite state space |
| **FSM property** | Preserves finiteness | Violates finiteness |

## Example: Temperature Control

### Conventional Timer-Based FSM
```
States: Off, Heating, Cooling
Transitions: 
  - Heat for 5 minutes if temp < 60°F (TIMER-BASED)
  - Cool for 3 minutes if temp > 75°F (TIMER-BASED)
Problem: Introduces timer state explosion
```

### Threshold-Based FSM
```
States: Off, Heating, Cooling
Transitions:
  - On if temp ≤ 62°F threshold (THRESHOLD-BASED)
  - Off if temp ≥ 68°F threshold (THRESHOLD-BASED)
Guarantee: Exactly 3 states, finite and deterministic
```

## Why This Matters

### 1. **Predictability**
Threshold-based systems have a guaranteed upper bound on complexity. You can analyze and verify all possible states.

### 2. **Resource Efficiency**
No need for continuous temporal tracking. Memory and computation are bounded.

### 3. **Safety in Embedded Systems**
Critical systems (medical, automotive, industrial) benefit from guaranteed finiteness—you can prove the system won't enter an unexpected state.

### 4. **Verifiability**
Threshold-based design enables formal verification and exhaustive state testing.

## Comparison to Conventional FSMs

- **Conventional FSMs**: Allow any transition mechanism (timers, events, signals)
- **Threshold-Based FSMs**: Restrict to discrete threshold comparisons only

Threshold-Based FSMs are a **subset** of conventional FSMs—they trade expressive power for the mathematical guarantee of finiteness.

## System Design Implications

### When to Use TBFSM

- Safety-critical systems requiring formal verification
- Embedded systems with strict resource constraints
- Hysteresis-based control (e.g., thermostats, trip logic)
- Systems where state explosion is a risk

### When Timers Are Necessary

If your system requires:
- Delays or retries
- Event scheduling
- Timeouts or watchdogs

Then you're moving beyond the pure TBFSM model and accepting unbounded state complexity. This is valid—just acknowledge the trade-off.

## Formal Properties

**Theorem**: A state machine using only threshold-based transitions has a finite state space equal to the number of distinct threshold combinations.

**Corollary**: Introducing even one timer mechanism violates this guarantee.

## Practical Pattern Recognition

In real systems, you often see TBFSM in:
- Schmitt trigger circuits (electronics)
- Hysteresis controllers (control theory)
- Trip logic (power systems)
- Level-based automation (process control)

These all use discrete threshold comparisons, not timers, to maintain state finiteness.
