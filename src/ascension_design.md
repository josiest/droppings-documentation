# Ascension System Design Specification
The ascension system is a point system to make gameplay more engaging.

Each time the snake consumes a fruit, it will gain an ascension point, which
will build up to an ascension glyph. When glyphs are filled they trigger board
events.

Board events may clear the board of droppings or advance to a next level, or
have other features in the future. Ascension glyphs may have multiple tiers,
working similar to numeric bases.

## Ascension Point System

Each time the snake consumes a fruit, it will gain a divine tally. Divine
tallies will be counted internally, as well as displayed with UI in the form
of an ascenscion glyph.

Ascension glyphs are patterns with distinct pieces. Each divine tally
corresponds to a piece of the ascension glyph. Visually, there are two routes
to take:

1. Ascension glyph is always visible, but individual pieces become "active,"
   changing color

2. Ascension glyph is invisible when no points. Each point reveals the next
   piece of the ascension glyph

When a glyph is completed, it will trigger a board event.

### Technical Specification

#### Ascension System

Ascension system will be a gameboard subsystem so that it's only active when
a game is in session, and resets between sessions

Interface
- Add point, Add point with an amount
- On Point Added(current total)
- On Glyph Completed(tier)
- Get number of points for a tier

Internal State
- mapping of tiers to points per tier

#### Ascension UI

- Ascension glyph Widget has a subobject for each glyph
- root object has a list of objects in order to determine the order the glyph
  pieces are activated in

- validate that subobjects is same length as ascension glyph tier 1 pieces
- when point is added, reveal next ascension glyph piece
- when glyph tier 1 is completed, create new ascension glyph within listbox

## Board Events

Board events are mechanics that trigger when an ascension glyph is completed.
For now, this will mean clearing some or all of the board of droppings. The
droppings system will need to be extended to be able to clear a specific amount
of droppings from the board. This should be data driven.

Ascencion glyphs have multiple tiers that work similar to numeric bases.
I.e. multiple glyphs need to be completed to complete the second tier.

The first tier should clear a specific amount of droppings from the board.
It may also speed up the snake and increase the length of its tail. The latter
will require adding a feature to the snake body that triggers a game over when
it commits oroboros.

The second tier should end the game for now. Once multiple levels are complete,
it will advance to the next level.

### Technical specification

#### Droppings
- add function to game board to clear random range by tag

#### Board event system
Board event system is a gameboard subsystem for managing active board events
- interface: list of board event scripts that are active during gameplay
- internal logic: hook into events on start since it's a monobehavior
