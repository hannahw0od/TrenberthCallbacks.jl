# TrenberthCallbacks.jl

`TrenberthCallbacks.jl` provides a callback for collecting energy-budget diagnostics from [SpeedyWeather.jl](https://github.com/SpeedyWeather/SpeedyWeather.jl) simulations.

The callback records the model diagnostics required to construct a Trenberth-style energy budget, including radiative and surface energy fluxes.

## Installation

`TrenberthCallbacks.jl` can be installed directly from GitHub.

Open the Julia package manager by pressing `]` in the Julia REPL, then run:

```julia
add https://github.com/hannahw0od/TrenberthCallbacks.jl.git
```

Then load the required packages:

```julia
using SpeedyWeather
using Dates
using TrenberthCallbacks
```

## Example

### 1. Create the model

First, create a SpeedyWeather spectral grid and model:

```julia
spectral_grid = SpectralGrid()
model = PrimitiveWetModel(spectral_grid)
```

### 2. Add the required diagnostics

The Trenberth callback requires radiation and surface-flux diagnostics to be enabled on the model:

```julia
add!(model, SpeedyWeather.RadiationOutput()...)
add!(model, SpeedyWeather.SurfaceFluxesOutput()...)
```

### 3. Create the Trenberth callback

Create a `TrenberthCallback` and specify how frequently the energy-budget diagnostics should be recorded.

For example, to record diagnostics every 12 hours:

```julia
cb = TrenberthCallbacks.TrenberthCallback(
    schedule = Schedule(every = Hour(12))
)
```

Then add the callback to the model:

```julia
add!(model.callbacks, :trenberth => cb)
```

### 4. Run the simulation

Initialise the model and run the simulation:

```julia
sim = initialize!(model)
run!(sim, period = Day(10))
```

The `TrenberthCallback` will collect the required energy-budget diagnostics according to the specified schedule.

## Complete Example

```julia
using SpeedyWeather
using Dates
using TrenberthCallbacks

# Set up the model
spectral_grid = SpectralGrid()
model = PrimitiveWetModel(spectral_grid)

# Add diagnostics required by the Trenberth callback
add!(model, SpeedyWeather.RadiationOutput()...)
add!(model, SpeedyWeather.SurfaceFluxesOutput()...)

# Create a callback that records diagnostics every 12 hours
cb = TrenberthCallbacks.TrenberthCallback(
    schedule = Schedule(every = Hour(12))
)

# Add the callback to the model
add!(model.callbacks, :trenberth => cb)

# Initialise and run the simulation
sim = initialize!(model)
run!(sim, period = Day(10))
```

## Using the Output

After the simulation has finished, the collected diagnostics are stored by the `TrenberthCallback` and can be used to analyse the model's energy budget.

For an interactive visualisation of these diagnostics, see **TrenberthDiagram.jl**:

```julia
add https://github.com/hannahw0od/TrenberthDiagram.jl.git
```

`TrenberthDiagram.jl` provides tools for turning the diagnostics collected by `TrenberthCallbacks.jl` into an interactive Trenberth-style energy-budget diagram.

## Related Packages

- **SpeedyWeather.jl** — provides the atmospheric model and diagnostics used by the callback.
- **TrenberthDiagram.jl** — provides an interactive visualisation of the energy-budget diagnostics collected by this package.

MADE BY HANNAH-JANE WOOD, LUCY HARLOW, AND OFER COHEN



[![Build Status](https://github.com/hannahw0od/TrenberthCallbacks.jl/actions/workflows/CI.yml/badge.svg?branch=main)](https://github.com/hannahw0od/TrenberthCallbacks.jl/actions/workflows/CI.yml?query=branch%3Amain)


