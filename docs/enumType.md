# Enum type
A set of named constants or a set of distinct cases.
Numeric and string-based enums

## Numeric enums
Auto-incremented:

enum Direction {
  Up = 1,
  Down,
  Left, 
  Right,
}
Up = 1, Down = 2, Left = 3, Right = 4

enum Direction {
  Up,
  Down,
  Left, 
  Right,
}
Up = 0, Down = 1, Left = 2, Right = 3

Direction.Down


## String enums

## Heterogeneous enums