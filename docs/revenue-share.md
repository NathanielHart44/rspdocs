!!! nfty-note "Note"
    These endpoints are only necessary for games that plan on implementing their own custom UI for the RSP integration. If you plan on using the RSP UI, you can skip this section.

## Get Player Listings

The `get_player_listings` endpoint retrieves the listings associated with a specific player in the RSP API. This endpoint accepts a GET request to retrieve the player's listings.

#### Endpoint URL

```
GET /get_player_listings/<str:ign>/
```

#### Request Format

The request does not require a request body. You need to include the player's in-game name (IGN) as a path parameter.

Example Request:

```
GET /get_player_listings/player1/
```

#### Response Format

The response from the `get_player_listings` endpoint follows a standard format:

- `success` (boolean): Indicates whether the request was successful (`true`) or if an error occurred during the process (`false`).
- `response` (object): Contains the listings associated with the player, including the borrower listings and the lender listings.

Example Response Body (Success):

=== "JSON"
    ```json
    {
        "success": true,
        "response": {
            "borrower_listings": [
                {
                    "id": 1,
                    "item_name": "Item 1",
                    "borrower": "player1",
                    "lender": "lender1"
                },
                {
                    "id": 2,
                    "item_name": "Item 2",
                    "borrower": "player1",
                    "lender": "lender2"
                }
            ],
            "lent_listings": [
                {
                    "id": 3,
                    "item_name": "Item 3",
                    "borrower": "player2",
                    "lender": "player1"
                },
                {
                    "id": 4,
                    "nft_uuids": [
                        "4a",
                        "4b"
                    ],
                    "description": "Item 4",
                    "lender_ign": "player1",
                    "tier": ,
                    "player_rating": 0,
                    "contract": {},
                    "status": "Available",
                    "market_type": "Revenue Share",
                    "borrower_ign": null,
                    "automatically_relist": true,
                    "private": false
                }
            ]
        }
    }
    ```

Example Response Body (Error):

=== "JSON"
    ```json
    {
        "success": false,
        "response": {
            "error": "Player not found"
        }
    }
    ```

#### Integration Example

Here's an example of how you can call the `get_player_listings` endpoint from your game's backend:

=== "Python"
    ```python
    import requests

    def get_player_listings(ign):
        response = requests.get(f"{RSP_URL}/get_player_listings/{ign}/", headers=rsp_headers)
        if response.status_code == 200:
            return response.json()
        else:
            return None
    ```

In this example, `RSP_URL` refers to the base URL of the RSP API, and `rsp_headers` includes the necessary headers for authentication with the RSP API.

To retrieve the player's listings from the RSP API, you can make a GET request to the `get_player_listings` endpoint, providing the player's in-game name (IGN).

=== "Python"
    ```python
    ign = "player1"
    rsp_listings = get_player_listings(ign)
    
    if rsp_listings["success"]:
        borrower_listings = rsp_listings["response"]["borrower_listings"]
        lent_listings = rsp_listings["response"]["lent_listings"]
        # Handle the listings data as needed
    else:
        # Error occurred while retrieving player listings
        # Handle the error response
    ```