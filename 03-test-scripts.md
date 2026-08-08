# Test Scripts

## Why should we test API requests?

A successful HTTP response does not necessarily mean that an API request worked correctly.
For example, a `200 OK` response tells us that the server successfully processed the request, but it does not guarantee that the response contains the data we expected.
Postman allows us to create automated assertions that verify different aspects of an API response.

## Test 01 - status code

The first test checks whether the API returned HTTP status code `200`.

```
pm.test("Status code is 200", () =>
{
    pm.expect(pm.response.code).to.eql(200);
});
```
## Test 02 - response time

The second test checks whether the API responds within a reasonable amount of time.

```
pm.test("Response time is less than 1000 ms", () =>
{
    pm.expect(pm.response.responseTime).to.be.lessThan(1000);
});
```

## Test 03 - response data

The next test checks whether the API returned the data we actually expected, rather than only checking whether the request technically succeeded.

```
const data = pm.response.json();
pm.test("Response contains Pikachu", () =>
{
    pm.expect(data.name).to.eql('pikachu')
});
```
`pm.response.json()` parses the JSON response and stores it in the data variable, allowing me to access fields such as `data.name` and `data.types`.
## Test 04 - response contains at least one type

The `types` field is an array, so I can use its `length` property to check whether the response contains at least one type.

```
pm.test("Response has types", () => 
{
    pm.expect(data.types.length).to.be.greaterThan(0);
});
```

Here:
- `data.types` accesses the `types` array.
- `.length` returns the number of elements in the array.
- `.to.be.greaterThan(0)` verifies that the array is not empty.

## Test 05 -  dynamic response validation

Hardcoding `"pikachu"` makes the test specific to one Pokemon. Instead, I made the test dynamic by extracting the Pokemon name from the request URL.

```
const requestUrl = pm.request.url.toString();
const parts = requestUrl.split("/");
const pokemonName = parts[parts.length - 1];
```
Here:
- `pm.request.url` accesses the URL of the current request,
- `.toString()` converts the Postman URL object into a string.
- `.split("/")` separates the URL into an array using `/` as the separator,
- `parts[parts.length - 1]` retrieves the last element of the array, which is the Pokemon name,

I can then compare the requested Pokémon with the Pokémon returned by the API:

```
pm.test("Response contains requested Pokemon", () =>
{
    pm.expect(data.name).to.eql(pokemonName);
});
```
This makes the test reusable for different Pokémon without changing the assertion.

## Breaking a test intentionally

To understand what a failed assertion looks like, I temporarily changed the expected Pokemon name from `"pikachu"` to `"bulbasaur"`.
The request still returned `200 OK`, but the data assertion failed because the API returned `"pikachu"`.
This demonstrated that a successful HTTP response does not necessarily mean that the returned data is correct.
