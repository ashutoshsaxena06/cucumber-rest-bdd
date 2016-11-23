# Gherkin Steps

This test suite introduces behavioural test steps on top of functional REST API steps from [cucumber-api](https://github.com/hidroh/cucumber-api)

The following is a list of steps, and their equivalent functional step

## Setup

```
Behavioural                                         Functional
--------------------------------------------------- --------------------------------------------------------------
Given I am a client                                 Given I send an accept JSON
```

## Retrieval

```
Behavioural                                         Functional
--------------------------------------------------- --------------------------------------------------------------
When I request an item "2"                          When I send a GET request to "http://url/items/2"

When I request a list of items                      When I send a GET request to "http://url/items"

When I request a list of items with:                When I send a GET request to "http://url/items" with:
   | User Id | 12 |                                     | userId |
                                                        | 12     |
```

## Creation

```
Behavioural                                         Functional
--------------------------------------------------- --------------------------------------------------------------
When I request to create an item                    When I send a POST request to "http://url/items"

When I request to create an item with:              When I set JSON request body to:
    | attribute | type    | value |                     """
    | User Id   | integer | 12    |                     {"userId":12,"title":"foo"}
    | Title     | string  | foo   |                     """
                                                    And I send a POST request to "http://url/items"

When I request to create an item with id "4"        When I send a PUT request to "http://url/items/4"

When I request to replace the item "4" with:        When I set JSON request body to:
    | attribute | type    | value |                     """
    | User Id   | integer | 7     |                     {"userId":7,"title":"foo"}
    | Title     | string  | foo   |                     """
                                                    And I send a PUT request to "http://url/items/4"
```

## Modification

```
Behavioural                                         Functional
--------------------------------------------------- --------------------------------------------------------------
When I request to modify the item "4" with:         When I set JSON request body to:
    | attribute | type    | value |                     """
    | Body      | string  | bar   |                     {"body":"bar"}
                                                        """
                                                    And I send a PATCH request to "http://url/items/4"
```

## Status Inspection

```
Behavioural                                         Functional
--------------------------------------------------- --------------------------------------------------------------
Then the request is successful                      Then the response status should be "200"

Then the request was redirected                     <N/A> (response status between "300" and "400")

Then the request failed                             <N/A> (response status between "400" and "600")

Then the request was successful and an item was     Then the response status should be "201"
  created

Then the request was successfully accepted          Then the response status should be "202"

Then the request was successful and no response     Then the response status should be "204"
  body is returned

Then the request failed because it was invalid      Then the response status should be "400"

Then the request failed because i am unauthorised   Then the response status should be "401"

Then the request failed because it was forbidden    Then the response status should be "403"

Then the request failed because the item was not    Then the response status should be "404"
  found  

Then the request failed because it was not allowed  Then the response status should be "405"

Then the request failed because there was a         Then the response status should be "409"
  conflict    

Then the request failed because the item has gone   Then the response status should be "410"

Then the request failed because it was not          Then the response status should be "501"
  implemented
```

## Response Inspection

```
Behavioural                                         Functional
--------------------------------------------------- --------------------------------------------------------------
Then the response has the following attributes:     Then the JSON response should have "userId" of type numeric
    | attribute | type    | value |                   with value "12"
    | User Id   | integer | 12    |                 Then the JSON response should have "title" of type numeric
    | Title     | string  | foo   |                   with value "foo"
    | Body      | string  | bar   |                 Then the JSON response should have "body" of type numeric with
                                                      value "bar"

Then the response is a list of 12 items             Then the JSON response should have "$." of type array with 12
                                                      entries

Then the response is a list of at least 12 items    <N/A>

Then two items have have the following attributes:  <N/A>
    | attribute | type    | value |
    | User Id   | integer | 12    |
    | Title     | string  | foo   |
    | Body      | string  | bar   |

<N/A>                                               Then the JSON response should follow "schema.json"
```

### attribute saving and re-use
```
Behavioural                                         Functional
--------------------------------------------------- --------------------------------------------------------------
When I save "User Id" as "user"                     When I grab "$.userId" as "user"
And I request the user "{user}"                     And I send a GET request to "http://url/users/{user}"
```