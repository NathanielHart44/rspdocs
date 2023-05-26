## Create Players

The `create_players` endpoint allows you to create players for a specific game in the RSP API. This endpoint accepts a POST request to create multiple players in a single call.

#### Endpoint URL

```
POST /create_players/
```

#### Request Format

The request should include the following parameters:

- `igns` (list): A list of in-game names (IGNs) for the players to be created.

Example Request Body:

=== "JSON"
    ```json
    {
        "igns": ["player1", "player2", "player3"]
    }
    ```

#### Response Format

The response from the `create_players` endpoint follows a standard format:

- `success` (boolean): Indicates whether the players were successfully created (`true`) or if there was an error during the creation process (`false`).
- `response` (object): Contains additional information about the players created and any potential errors.

Example Response Body (Success):
=== "JSON"
    ```json
    {
        "success": true,
        "response": {
            "players_created": 3,
            "pre-existing_players": []
        }
    }
    ```

Example Response Body (Error):
=== "JSON"
    ```json
    {
        "success": false,
        "response": {
            "error": "Invalid request payload"
        }
    }
    ```

#### Integration Example

Here's an example of how you can call the `create_players` endpoint from your game's backend:

=== "Python"
    ```python
    import requests

    def rsp_create_players(payload):
        response = requests.post(f"{RSP_URL}/create_players/", json=payload, headers=rsp_headers)
        if response.status_code == 200:
            return response.json()
        else:
            return None
    ```

In this example, `RSP_URL` refers to the base URL of the RSP API, and `rsp_headers` includes the necessary headers for authentication with the RSP API.

To create players and integrate with the RSP API, you can make a POST request to the `create_players` endpoint, providing the necessary payload data.

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

## Get Player Inventory

The `get_player_inventory` endpoint retrieves the inventory of a player for a specific game in the RSP API. This endpoint accepts a POST request to retrieve the inventory information.

#### Endpoint URL

```
POST /get_player_inventory/<str:ign>/
```

#### Request Format

The request should include the following parameters:

- `nfts_in_wallet` (list): A list of token IDs representing the NFTs in the player's wallet.
- `availability_types` (optional): A list of availability types to filter the inventory response. This parameter can be specified as a list of the following values, or omitted to retrieve all NFTs:
    - `owned_and_available` - NFTs that are owned by the player and available to be used in-game by this player.
    - `owned_not_available` - NFTs that are owned by the player but are currently not able to be used in-game by this player.
    - `available_not_owned` - NFTs that are owned by another player but are currently being used in-game by this player.

Example Request Body:

=== "JSON"
    ```json
    {
        "nfts_in_wallet": ["nft1", "nft2"],
        "availability_types": ["owned_and_available", "available_not_owned"]
    }
    ```

#### Response Format

The response from the `get_player_inventory` endpoint follows a standard format:

- `success` (boolean): Indicates whether the request was successful (`true`) or if an error occurred during the process (`false`).
- `response` (object): Contains the inventory information for the player, including owned and available NFTs, owned but not available NFTs, and available but not owned NFTs.

Example Response Body (Success):

=== "JSON"
    ```json
    {
        "success": true,
        "response": {
            "lender_ign": "lender_username"
            "owned_not_available": [],
            "owned_and_available": [
                "1",
                "2"
            ],
            "available_not_owned": [
                "3"
            ]
        }
    }
    ```

Example Response Body (Error):

=== "JSON"
    ```json
    {
        "success": false,
        "response": "Player not found"
    }
    ```

#### Integration Example

Here's an example of how you can call the `get_player_inventory` endpoint from your game's backend:

=== "Python"
    ```python
    import requests

    def get_rsp_inventory(ign, payload):
        response = requests.post(f"{RSP_URL}/get_player_inventory/{ign}/", json=payload, headers=rsp_headers)
        if response.status_code == 200:
            return response.json()
        else:
            return None
    ```

In this example, `RSP_URL` refers to the base URL of the RSP API, and `rsp_headers` includes the necessary headers for authentication with the RSP API.

To retrieve the player's inventory from the RSP API, you can make a POST request to the `get_player_inventory` endpoint, providing the player's in-game name (IGN) and the necessary payload data.

=== "Python"
    ```python
    ign = "player1"
    payload = {
        "nfts_in_wallet": ["nft1", "nft2"],
        "availability_types": ["owned_and_available"]
    }

    rsp_inventory = get_rsp_inventory(ign, payload)

    if rsp_inventory["success"]:
        # Handle the inventory response data as needed
    else:
        # Error occurred while retrieving inventory
        # Handle the error response
    ```

## Get Rewards Split

The `get_rewards_split` endpoint retrieves the rewards split information for a specific player in the RSP API. This endpoint accepts a GET request to retrieve the rewards split data.

#### Endpoint URL

```
GET /get_rewards_split/<str:ign>/
```

#### Request Format

The request does not require a request body. You can include the player's in-game name (IGN) as a path parameter.

Example Request:

```
GET /get_rewards_split/player1/
```

#### Response Format

The response from the `get_rewards_split` endpoint follows a standard format:

- `success` (boolean): Indicates whether the request was successful (`true`) or if an error occurred during the process (`false`).
- `response` (object): Contains the rewards split information for the player, including the lender's in-game name (IGN) and the percentage split.

Example Response Body (Success):

=== "JSON"
    ```json
    {
        "success": true,
        "response": {
            "lender_ign": "lender_username",
            "lender_percent": 70
        }
    }
    ```

Example Response Body (Error):

=== "JSON"
    ```json
    {
        "success": false,
        "response": "Player not found"
    }
    ```

#### Integration Example

Here's an example of how you can call the `get_rewards_split` endpoint from your game's backend:

=== "Python"
    ```python
    import requests

    def get_rsp_rewards_split(ign):
        response = requests.get(f"{RSP_URL}/get_rewards_split/{ign}/", headers=rsp_headers)
        if response.status_code == 200:
            return response.json()
        else:
            return None
    ```

In this example, `RSP_URL` refers to the base URL of the RSP API, and `rsp_headers` includes the necessary headers for authentication with the RSP API.

To retrieve the rewards split information from the RSP API, you can make a GET request to the `get_rewards_split` endpoint, providing the player's in-game name (IGN).

=== "Python"
    ```python
    ign = "player1"
    rsp_rewards_split = get_rsp_rewards_split(ign)

    if rsp_rewards_split["success"]:
        # Handle the rewards split response data as needed
    else:
        # Error occurred while retrieving rewards split
        # Handle the error response
    ```