# Patterns for Managing Source Code Branches
Link: https://martinfowler.com/articles/branching-patterns.html

## Base Patterns
### Source Branching
* Create a copy of the code line and start working on it
* The falling isn't going to hurt you, but the landing will. With
 source code: branching is easy, merging is harder.
### Mainline
* A single, shared branch that serves as the current state of the product
* Should be kept in a healthy state

## Integration Patterns
### Mainline Integration
* Developers integrate by pulling -> merging -> push back again
### Feature Branching
* Push all work for a feature to a single remote branch, integrate into
 main line when the feature is complete
### Integration Frequency
* High frequency integration is more controllable. Need automated tests.
### Continuous Integration
* Never leave more than a day's work in your local branch
* Partially implemented features can be integrated, with feature hiding
 or feature flags
* Open source code base adopts feature branching, as it's hard to predict
 a contributor's potential outcomes.
### Pre-Integration Review
* Every commit to mainline is peer-reviewed before integration.
* Pre-Integration Reviews and Feature Branching are the more common combination.
 Why bother reviewing a partially complete feature?
* The feedback is usually not in time. Pair Programming offers a faster
 feedback cycle.
* Useful when there exists a few trusted maintainers and many untrusted
 contributors. Open source code base, as well as in commercial groups.
* Integration Friction: how long it takes to do a single integration
 really matters.
* Modularity help reduce learning cost and ease integration conflicts.

Overall, the author prefers continuous integration. If cases are continuous
 integration is hard, fix those obstacles.
 
## The path from mainline to production release
* To be continued....