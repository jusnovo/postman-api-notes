# Chaining Requests

## What does it mean to chain a request?
To chain a request means to pull a value from one response and use it to drive the next request, instead of manually copying values between requests.
This allows requests to be connected into an end-to-end API flow.

## How to do it?

### Step 01 - Pokémon → Species

I used Pikachu as the test case.
The goal was to extract the `species.url` value from the Pokémon response and store it as a collection variable.
The next request could then use this variable instead of having a hardcoded Pokémon name in the URL. 

The script consists of three main steps:

1. Parse the JSON response using the `pm.response.json()` function and save it as the `data` variable.
2. Extract the required value from the `data` variable.
3. Save the extracted value as a collection variable using `pm.collectionVariables.set("x", y)`.

Script:
```
const data = pm.response.json();
const value = data.species.url;
pm.collectionVariables.set("species_url", value);
```
### Step 02 - using the newly created variable in the Species & Evolution request

Instead of using a hardcoded URL: `{{baseURL}}/pokemon-species/pikachu`, I changed the request URL to `{{species_url}}`.
The value of `{{species_url}}` is taken from the previous Pokémon request, which links the two requests together.

### Step 03 - Evolution Request

I applied the same approach to the Species response.
The `evolution_chain.url` value was extracted and stored as a new collection variable called `evolution_url`, using the below script:

```
const data = pm.response.json();
const value = data.evolution_chain.url;
pm.collectionVariables.set("evolution_url", value);
```
I then created a new Evolution Chain request using the dynamically generated variable `{{evolution_url}}`.
This created a three-step API flow: Pokemon → Species → Evolution Chain.
The value from each response is automatically passed to the next request without manually copying URLs.

## Result

The three requests can now be executed in sequence: Pokemon → Species → Evolution Chain.

Changing the Pokémon in the first request automatically updates the subsequent requests through the collection variables.
