# Flutter Chess Board - Project Overview

## Project Description

**flutter_chess_board** is a Flutter package that provides a fully functional chessboard widget. The widget maintains game state, enforces chess rules, and provides callbacks for game events like moves, checkmate, and draws.

- **Version**: 0.9.4
- **Language**: Dart/Flutter
- **Package Manager**: Flutter pub.dev
- **Repository**: https://github.com/deven98/flutter_chess_board

## Project Structure

```
flutter_chess_board/
├── lib/                           # Main package library
│   ├── flutter_chess_board.dart   # Public API exports
│   └── src/                       # Internal implementation
│       ├── chess_board.dart       # Main ChessBoard widget (StatefulWidget)
│       ├── chess_board_controller.dart  # Controller for programmatic control
│       ├── board_model.dart       # Game state and logic model
│       ├── board_rank.dart        # Represents a row of squares
│       └── board_square.dart      # Individual square widget
├── example/
│   └── lib/
│       └── main.dart              # Example app showing usage
├── test/
│   └── chess_board_test.dart      # Unit tests
├── images/                        # Board background assets (brown, green, orange, dark_brown)
├── pubspec.yaml                   # Project configuration and dependencies
├── README.md                       # User documentation
└── CHANGELOG.md                    # Version history
```

## Core Dependencies

- **chess** (>=0.6.5 <1.0.0): Handles chess game rules and logic
- **chess_vectors_flutter** (>=1.0.6 <2.0.0): Provides chess piece vector graphics
- **scoped_model** (>=1.0.0 <2.0.0): State management pattern

## Key Components

### ChessBoard Widget (`lib/src/chess_board.dart`)

The main widget that users interact with. A StatefulWidget that:
- Displays an 8x8 chess board with pieces
- Handles user touch/click input for moves
- Maintains game state through board position and move history
- Supports 4 board theme types: brown, darkBrown, orange, green

**Key Properties:**
- `size`: Board dimensions (width and height)
- `boardType`: Visual theme of the board
- `onMove`: Callback when a move is made (receives move notation string)
- `onCheckMate`: Callback when checkmate occurs (receives winning color)
- `onDraw`: Callback when game ends in a draw
- `whiteSideTowardsUser`: Orientation control (white or black side faces user)
- `enableUserMoves`: Boolean to enable/disable user interaction
- `controller`: ChessBoardController for programmatic control

### ChessBoardController (`lib/src/chess_board_controller.dart`)

Provides programmatic control over the board:
- Make moves programmatically
- Reset the board
- Change board state without user interaction

### BoardModel (`lib/src/board_model.dart`)

The state management model using scoped_model pattern:
- Tracks current board position
- Maintains move history
- Implements chess rules validation
- Detects check, checkmate, and draw conditions
- Interacts with the `chess` package for game logic

### Board Layout Components

- **board_rank.dart**: Represents a single row (rank) of the chess board
- **board_square.dart**: Individual square widget handling piece display and user interaction

## Board Types

The widget supports 4 different board visual themes:
1. **Brown** (default)
2. **Dark Brown**
3. **Orange**
4. **Green**

Each has resolution-specific assets (1x, 2x, 3x) for different screen densities.

## Game State Management

The project uses the **scoped_model** pattern for state management:
- `BoardModel` extends ScopedModel to manage chess game state
- Changes to the model notify listeners (the ChessBoard widget)
- The widget rebuilds when moves are made or game state changes

## Architecture Notes

- **Separation of Concerns**: Game logic is separated from UI through the model pattern
- **Immutable Board Representation**: Uses the `chess` package for reliable position representation
- **Callback-based Events**: Game events (moves, checkmate, draw) are communicated via callbacks
- **Orientation Support**: Can flip the board so either player can be presented to the user

## Working with the Code

### Adding New Board Themes
1. Create new board image asset in `images/` directory
2. Add asset paths to `pubspec.yaml` flutter.assets section
3. Add new enum value to `BoardType` enum in `chess_board.dart`
4. Update board selection logic to use new asset

### Modifying Game Rules
1. Changes to rule enforcement come from the `chess` package
2. To customize rules beyond what `chess` provides, modify `board_model.dart`
3. Ensure `onCheckMate` and `onDraw` callbacks are triggered correctly

### Adding New Callbacks
1. Add callback parameter to ChessBoard widget
2. Add callback to BoardModel or board state
3. Trigger callback at appropriate point in game flow
4. Update widget builder to pass callback to model

## Development Requirements

- **Dart SDK**: >=2.0.0 <3.0.0
- **Flutter SDK**: Any recent stable version
- See `pubspec.yaml` for exact dependencies

## Example Usage

```dart
import 'package:flutter/material.dart';
import 'package:flutter_chess_board/flutter_chess_board.dart';

void main() {
  runApp(
    MaterialApp(
      home: Scaffold(
        body: Center(
          child: ChessBoard(
            size: 200.0,
            onMove: (move) {
              print("Move made: $move");
            },
            onCheckMate: (color) {
              print("Checkmate! Winner: $color");
            },
            onDraw: () {
              print("Game is a draw!");
            },
          ),
        ),
      ),
    ),
  );
}
```

## Current Status

- Version 0.9.4 (pre-1.0)
- Under final testing before 1.0 release
- Stable for most use cases
- Chess logic is well-tested and reliable

## Recent Changes

Recent commits show:
- Return type of check detection changed from `bool` to `PieceColor`
- Example code updates
- Added `onCheck` callback
- SDK constraint updates to fix compatibility issues
