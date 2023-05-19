---
hide:
  - navigation
---

## First Steps

Call the `create_players` endpoint for all players in your game. This will create a player account for each player in the database.

=== "Python"
    ```python
    payload = {
        "igns": ["player1", "player2", "player3"]
    }
    rsp_players = rsp_create_players(payload)
    if rsp_players["success"]:
        # Players created successfully
        # Handle the response data as needed
    else:
        # Error occurred while creating players
        # Handle the error response
    ```
