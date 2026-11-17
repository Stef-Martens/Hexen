# Hexen

A turn-based card game on a hexagonal grid, built in **Unity (C#)** as the final exam project for *Platform Development 3*.

You play cards to move and attack: each card has its own move pattern on the hex board. Enemies and the cards in your hand are generated randomly at the start of every game.

![Unity](https://img.shields.io/badge/Unity-2021.3-000000?logo=unity&logoColor=white)
![C#](https://img.shields.io/badge/C%23-239120?logo=csharp&logoColor=white)

## Cards

| Card     | Effect |
|----------|--------|
| Swipe    | Hits every tile in a straight line, from the player to the edge of the board |
| Slash    | Hits the chosen neighbouring tile and the two tiles next to it (an arc of three) |
| Pushback | Pushes enemies in an arc of three tiles one step away from the player |
| Teleport | Moves the player to the chosen tile |
| Meteor   | Hits a target tile and the tiles around it |

## Code structure

The project was about clean game architecture as much as gameplay, so the logic is kept separate from Unity's views:

- **Model / view separation**: `Board`, `Position` and `Engine` hold the game logic as plain C#. `BoardView`, `PositionView`, `PieceView` and `CardView` only handle presentation and input.
- **Strategy pattern for cards**: every card is a `MoveSet` subclass (`SwipeMoveSet`, `SlashMoveSet`, …) that decides which tiles it highlights and what happens when it is played. New cards plug in without touching the engine.
- **State pattern for the game flow**: `GameStateManager` switches between `StartState` (random board setup) and `PlayingState`.
- **Hex-grid maths**: `Position` handles the hex directions and neighbours, and `PositionHelper` converts between grid coordinates and Unity world space.

```
Assets/Scripts/
├── Board.cs, Engine.cs, Position.cs        Game model and rules
├── *MoveSet.cs, MoveSetCollection.cs       One strategy per card type
├── GameStateManager.cs, *State.cs          Game-flow state machine
├── *View.cs                                Unity presentation layer
└── CardManager.cs                          Random hand generation
```

## Running it

1. Install **Unity 2021.3.10f1** (or a later 2021.3 LTS) via Unity Hub.
2. Open this folder as a project.
3. Open `Assets/Scenes/HexGame.unity` and press **Play**.
