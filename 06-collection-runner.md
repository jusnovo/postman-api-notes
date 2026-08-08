# Collection Runner

## Why use the Collection Runner?

The Collection Runner allows multiple requests to be executed as a sequence instead of running each request manually.
It is useful for checking whether a collection of API requests and their tests work together as an end-to-end flow.

## Step 01 - running the collection

I ran the collection containing my Pokemon requests, chained requests, error cases, and test scripts.
The Collection Runner provided a summary of the results for each request and its assertions.

## Step 02 - interpreting a failed test

During the run, the `IDs match` test failed:

```
expected 25 to deeply equal 16
```

This happened because the request was still using `/pokemon/pikachu`, which returned Pokemon ID 25, while the pre-request script had generated a random ID of 16.
The test itself was working correctly. The problem was that the random-ID test was being used with a request designed specifically for Pikachu.
This showed that tests need to match the scenario they are intended to validate.

## Step 03 - deliberately breaking a test

I then intentionally changed one of the expected values in the Pikachu test so that it would fail.
The request itself still returned a successful response, but the assertion failed in the Collection Runner.

This demonstrated the difference between:

1. A request succeeding.
2. An individual test passing.
3. The entire collection passing.

## What I learned

The Collection Runner allows individual API requests and their tests to be executed together and gives a higher-level view of the overall result.
It is useful for demonstrating an integration flow because I can see which requests and assertions pass or fail without manually checking every request.
I also learned that a failing test does not necessarily mean that the API request failed. The request can succeed while an assertion fails because the returned data does not match the expected behavior.
