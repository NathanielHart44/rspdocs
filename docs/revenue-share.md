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

## Process Breach of Contract

The `process_breach_of_contract` endpoint handles the processing of a breach of contract for a specific player and listing in the RSP API. This endpoint accepts a GET request to initiate the breach resolution.

#### Endpoint URL

```
GET /process_breach_of_contract/<str:ign>/<listing_id>/
```

#### Request Format

The request does not require a request body. You need to include the player's in-game name (IGN) and the listing ID as path parameters.

Example Request:

```
GET /process_breach_of_contract/player1/12345/
```

#### Response Format

The response from the `process_breach_of_contract` endpoint follows a standard format:

- `success` (boolean): Indicates whether the request was successful (`true`) or if an error occurred during the process (`false`).
- `response` (object): Contains the response message indicating the success or failure of the breach resolution process.

Example Response Body (Success):

=== "JSON"
    ```json
    {
        "success": true,
        "response": "Successfully ended rental."
    }
    ```

Example Response Body (Error):

=== "JSON"
    ```json
    {
        "success": false,
        "response": "No listing with that listing_id found in RSP database."
    }
    ```

#### Integration Example

Here's an example of how you can call the `process_breach_of_contract` endpoint from your game's backend:

=== "Python"
    ```python
    import requests

    def process_rsp_contract_breach(lender_ign, listing_id):
        response = requests.get(f"{RSP_URL}/process_breach_of_contract/{lender_ign}/{listing_id}/", headers=rsp_headers)
        if response.status_code == 200:
            return response.json()
        else:
            return None
    ```

In this example, `RSP_URL` refers to the base URL of the RSP API, and `rsp_headers` includes the necessary headers for authentication with the RSP API.

To initiate the breach resolution process in the RSP API, you can make a GET request to the `process_breach_of_contract` endpoint, providing the lender's in-game name (IGN) and the listing ID.

=== "Python"
    ```python
    lender_ign = "lender_username"
    listing_id = "12345"
    rsp_response = process_rsp_contract_breach(lender_ign, listing_id)
    if rsp_response["success"]:
        # Handle the breach resolution response data as needed
    else:
        # Error occurred while processing breach of contract
        # Handle the error response
    ```