# Cargo Dependency Resolution Issue Proof of Concept

This repo contains an empty Cargo project with two dependencies, `sqlx 0.7.2` and `vodozemac 0.4.0`. 
If you try to build this project Cargo will claim a dependency conflict:

```
error: failed to select a version for `zeroize`.
    ... required by package `rsa v0.9.0`
    ... which satisfies dependency `rsa = "^0.9"` of package `sqlx-mysql v0.7.2`
    ... which satisfies dependency `sqlx-mysql = "=0.7.2"` of package `sqlx v0.7.2`
    ... which satisfies dependency `sqlx = "^0.7.2"` of package `orig v0.1.0 (<redacted for privacy>)`
versions that meet the requirements `^1.5` are: 1.6.0, 1.5.7, 1.5.6, 1.5.5, 1.5.4, 1.5.3

all possible versions conflict with previously selected packages.

  previously selected package `zeroize v1.3.0`
    ... which satisfies dependency `zeroize = "=1.3"` of package `x25519-dalek v1.2.0`
    ... which satisfies dependency `x25519-dalek = "^1.2.0"` of package `vodozemac v0.4.0`
    ... which satisfies dependency `vodozemac = "^0.4.0"` of package `orig v0.1.0 (<redacted for privacy>)`

failed to select a version for `zeroize` which could resolve this conflict
```

I believe this conflict is incorrect. `sqlx` with the feature set selected **does not** depend on
`sqlx-mysql`, but cargo claims it does here. You can confirm this by removing the dependency on
`vodozemac` and running `cargo tree`. Neither `sqlx-mysql` or `zeroize` will appear in the
dependency tree.
