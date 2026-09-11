# bikeshare-rebalance-optimizer

Which bike-share stations to restock when the trucks can only reach a few.

The problem is a selection one: a handful of trucks over a network of several hundred
docking stations reach perhaps thirty of them in a shift, so choosing which stations to
touch is the decision. This repository recovers demand from station data that only
records rentals, prices a bike by its position in the network rather than by the demand
at its dock, and evaluates repositioning policies against that price.

Start with [docs/architecture.md](docs/architecture.md) for the argument, then
[docs/formulation.md](docs/formulation.md) for the model. The operating contract for
working in this repo is [CLAUDE.md](CLAUDE.md).
