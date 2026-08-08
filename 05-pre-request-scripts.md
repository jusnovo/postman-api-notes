# Pre-request Scripts / Dynamic Variables

## Why use pre-request scripts?

A pre-request script runs before an API request is sent.
This allows me to generate or prepare values dynamically instead of using static data. For example, I can generate a random ID, timestamp, or unique request value before every request.

## Step 01 - generate a random ID

I generated a random Pokémon ID between 1 and 151 using a pre-request script.

```
const randomId = Math.floor(Math.random() * 151) + 1;
pm.collectionVariables.set("random_id", randomId);
```

## Step 02 - use the variable in the request

Instead of using a specific Pokémon, I changed the request URL to:

```
{{base_url}}/pokemon/{{random_id}}
```
The `random_id` variable is replaced with a newly generated value before every request.
This means the same request can test different Pokémon without manually changing the URL.

## Step 03 - validate the generated ID

After the request is sent, I used a test to check whether the ID returned by the API matches the ID generated before the request.

```
const data = pm.response.json();
const randomId = Number(pm.collectionVariables.get("random_id"));

pm.test("IDs match", () => 
{
    pm.expect(data.id).to.eql(randomId);
});
```
### How it works

First, I parse the JSON response:

```
const data = pm.response.json();
```

This allows me to access the returned Pokémon ID using:

```
data.id
```

The `random_id` collection variable is retrieved using:

```
pm.collectionVariables.get("random_id")
```
Collection variables are returned as strings, so I convert the value back into a number using:

```
Number(pm.collectionVariables.get("random_id"))
```

I then compare the generated ID with the ID returned by the API:

```
pm.expect(data.id).to.eql(randomId);
```
If both IDs are the same, the test passes.

## What I learned

This exercise showed how pre-request scripts and tests can work together. The pre-request script generates data before the API call, while the test validates the result after the API call.
This approach can be used for more than random Pokémon IDs. In real API testing, pre-request scripts can be used to generate timestamps, unique request IDs, random test data, authentication values, and other dynamic inputs.
