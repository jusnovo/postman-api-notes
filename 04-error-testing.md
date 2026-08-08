# Error Handling / Negative Testing

## Why test error cases?

Testing an API is not only about proving that valid requests work. It is also important to understand how the API behaves when it receives invalid, unexpected or incomplete input.
Negative testing means deliberately sending requests that should fail and verifying that the API fails in the expected way.
It is useful when working with integrations because we need to know what happens when a customer or another system sends invalid data.

## Test 01 - non-existent Pokemon

I created a request using a Pokémon name that does not exist:

```
{{baseURL}}/pokemon/fakepokemon
```

The API returned code `404 Not Found`. The response body was `Not Found`.

### Status code assertion

Since `404` was the expected response, I created a test to verify it:

```
pm.test("Status code is 404", () => 
{
    pm.expect(pm.response.code).to.eql(404);
});
```
This is a negative test because the API is expected to reject the request. The test itself should still pass because `404` is the expected behavior.

### Error body assertion

The error response was plain text rather than a JSON object, so I used `pm.response.text()` instead of `pm.response.json()`.

```
pm.test("Text is Not Found", () =>
{
    pm.expect(pm.response.text()).to.eql("Not Found");
});
```

This checks that the entire response body is exactly `"Not Found"`.

### Observation

The successful Pokemon requests return structured JSON, while this error response returns plain text.
This is important because code handling API responses should not always assume that an error response has the same JSON structure as a successful response.


## Test 02 - request without a Pokemon name

I then tested 

```{{base_url}}/pokemon/```

I initially expected this request to return an error because there was no Pokemon name after `/pokemon/`.
However, the API returned `200 OK` and a JSON response containing a list of Pokemon.


### Observation

The `/pokemon/` endpoint is not treated as an invalid request by this API. Instead, it acts as a collection endpoint that returns a list of Pokémon.
It was different from my initial expectation. This test demonstrated why negative testing should be based on observed API behavior rather than assumptions.

## Additional test - Pokemon list response

Since `/pokemon/` returns a list rather than an error, I added a test to verify the structure of the response.

First, I parsed the JSON response:

```
const data = pm.response.json();
```

Then I checked that `results` is an array and that it contains at least one item:

```
pm.test("Pokemon list is returned", () => {
    pm.expect(data.results).to.be.an("array");
    pm.expect(data.results.length).to.be.greaterThan(0);
});
```

## Key observations

The tests showed that different types of failures can happen at different levels:

1. A non-existent Pokemon produces an HTTP `404` response.
2. The error response can have a different format from a successful response.
3. `/pokemon/` is not considered an error by this API and instead returns a collection of Pokémon.
4. An expected failure should still result in a passing test if the API behaves as expected.
5. It is important to observe the actual API behavior before writing assertions.
