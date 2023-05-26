---
hide:
  - navigation
---

## First Steps

The first step to integration is implementing all of the core endpoints into your production environment. This includes the following endpoints:

- `create_players`
- `get_player_inventory`
- `get_reward_split`

After implementing these endpoints, you should call the `create_players` endpoint for all players in your game. This will create a player account for each player in the database.

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

!!! nfty-note "Note"
    The `create_players` endpoint will only create player accounts for players that do not already exist in the RSP database. If they already exist, the endpoint will not create a new one. This means that you can safely call the `create_players` endpoint for all players in your game, and it will not create any duplicates.