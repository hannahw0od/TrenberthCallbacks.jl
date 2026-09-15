# TrenberthCallbacks.jl

To add this package to your environment:
- Go into your Julia package manager
- Run: add https://github.com/hannahw0od/TrenberthCallbacks.jl.git

Using TrenberthCallbacks.jl:

using SpeedyWeather, Dates, TrenberthCallbacks

spectral_grid = SpectralGrid()
model = PrimitiveWetModel(spectral_grid)

# Explicitly add diagnostics your callback expects
add!(model, SpeedyWeather.RadiationOutput()...)
add!(model, SpeedyWeather.SurfaceFluxesOutput()...)

cb = TrenberthCallbacks.TrenberthCallback(schedule=Schedule(every=Hour(12)))
add!(model.callbacks, :trenberth => cb)

sim = initialize!(model)
run!(sim, period=Day(10))

Made by Hannah-Jane Wood, Lucy Harlow, and Ofer Cohen



[![Build Status](https://github.com/hannahw0od/TrenberthCallbacks.jl/actions/workflows/CI.yml/badge.svg?branch=main)](https://github.com/hannahw0od/TrenberthCallbacks.jl/actions/workflows/CI.yml?query=branch%3Amain)


