# How we built the GitHub globe

* Author: Tobias Ahlin
* Link: https://github.blog/2020-12-21-how-we-built-the-github-globe/

Q: why three.js?

draw the world map
use html5 canvas to parse the data and analyze if if is land or ocean.
Q: curious what kind of world map png it is using.

Rotate the earch according to the current location.
note: use new Date() to determine the timezone. interesting.

perf optimization
1. turn off antialias (every game pler with cheap video card knows this, lol)
2. to smooth the edge, place a halo behind the sphere. interesting.
3. use a chrome to mock the canvas before it is loaded and ready for animation (similar to youtube and us)
4. quality tiers
Q: how to determine which quality tier to use?

Q: I am more curious how it handles multiple curve animations at the same time? Is is using techlogics like zone or something similiar for better perf?