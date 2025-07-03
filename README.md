# GAMSFiles

[![CI](https://github.com/HolyLab/GAMSFiles.jl/actions/workflows/CI.yml/badge.svg)](https://github.com/HolyLab/GAMSFiles.jl/actions/workflows/CI.yml)
[![codecov.io](http://codecov.io/github/HolyLab/GAMSFiles.jl/coverage.svg?branch=master)](http://codecov.io/github/HolyLab/GAMSFiles.jl?branch=master)

Parse files for [General Algebraic Modeling System](https://www.gams.com/) and return
Julia expressions.
As a matter of practicality, the only current purpose of this package is to parse
the [problem files from Rios & Sahinidis](https://sahinidis.coe.gatech.edu/dfo).
Please reach out if you'd like to help extend this package. 

## Demo

Suppose you have a local file, `problem.gms`, in GAMS format. Then

```julia
julia> using GAMSFiles

# This parses the file, returning an expression that defines a module
# together with information about starting point and bounds
julia> modex, axs = parsegams(Module, "problem.gms");

# Create the module
julia> m = eval(modex)
Main.problem

# Extract the objective function
julia> f = getglobal(m, :objective)
objective (generic function with 1 method)

# Get the initial point and the variable bounds
julia> x0, lo, hi, isfixed = axs
([1.0, 2.0, 1.0, 1.0, 4.0, 3.0], [-Inf, -Inf, -Inf, -Inf, -Inf, -Inf], [Inf, Inf, Inf, Inf, Inf, Inf], [false, false, false, false, false, false])

# Try evaluating the objective
julia> f(x0)
1.624842441282596
```

`x0` is the default initial point. `lo` contains the variable lower bounds, and `hi` the variable upper bounds.
`isfixed` is `true` for variables that should be held constant.
