# Collections and Organization

## What is a collection?

A collection in Postman is a way of grouping related API requests together.
Instead of keeping requests as separate, unrelated requests, I can organize them into folders based on the API resource or purpose of the request.
This becomes especially useful as the number of requests grows and when I want to run multiple requests together.

## Step 01 - creating basic API requests

I started by creating simple `GET` requests against the PokéAPI.

For example, to retrieve information about Pikachu:

```
{{baseURL}}/pokemon/pikachu
```

This request returns information about the Pokémon itself, such as its:

- name
- ID
- types
- abilities
- species URL
- other Pokemon data

Next, I created a request for Pikachu's species information:

```text
{{baseURL}}/pokemon-species/bulbasaur
```

This endpoint returns information about the Pokemon's species rather than the Pokemon itself.
This helped me see that APIs can expose related resources through different endpoints.

## Step 02 - understanding the structure of an API request

The basic structure of the requests I created was:

```
HTTP method + base URL + endpoint
```

The HTTP method tells the API what kind of operation I want to perform.

In these examples I used `GET`, because I wanted to retrieve information rather than create or modify data.


## Step 03 - using a base URL variable

Instead of repeating the full API URL in every request, I created a collection variable called `baseURL`.

The requests could then use:

```
{{baseURL}}/pokemon/pikachu
```

instead of:

```
https://pokeapi.co/api/v2/pokemon/pikachu
```

This makes the collection easier to maintain because the base URL is stored in one place rather than being hardcoded into every request.
If the API base URL changed, I would only need to update the variable instead of changing every request individually.


## Step 04 - creating related requests

As I became more familiar with the API, I created additional Pokémon requests:

```
{{baseURL}}/pokemon/pikachu
{{baseURL}}/pokemon/bulbasaur
{{baseURL}}/pokemon/meowth
```

I also created corresponding species requests:

```
{{baseURL}}/pokemon-species/pikachu
{{baseURL}}/pokemon-species/bulbasaur
{{baseURL}}/pokemon-species/meowth
```

This gave me a small set of related requests that could be organized into folders.


## Step 05 - organizing requests into folders

I organized the requests into folders based on the type of resource they interact with.

The collection eventually looked like:

```
PokéAPI Practice
│
├── Pokémon
│   ├── Pikachu
│   ├── Bulbasaur
│   ├── Meowth
│
├── Species & Evolution
    ├── Pikachu
    ├── Bulbasaur
    └── Meowth
```

The folder structure makes it easier to understand what each request is for.


## Step 06 - why organization matters

As the number of API requests grows, keeping everything in one list becomes difficult to manage. Folders provide a clear structure and make related requests easier to find.
They also become useful when working with the Collection Runner because I can choose which requests or folders to execute together.
The collection therefore becomes more than just a list of API requests. It becomes an organized API testing workspace.


## What I learned

I learned how to:
- create and save API requests in a Postman collection,
- use variables for reusable values such as the API base URL,
- understand the relationship between an HTTP method, base URL, and endpoint,
- organize requests into folders,
- separate different API resources into logical groups,
- build a collection that can later be used for automated testing.

This organization became the foundation for the later exercises involving request chaining, test scripts, error handling, dynamic variables, and the Collection Runner.
