---
title: Preauthorize
description: JavaScript preauthorize
exl-id: b7493ca6-1862-4cea-a11e-a634c935c86e
---
# Preauthorize {#js-preauthorize}

>[!NOTE]
>
>The content on this page is provided for information purposes only. Usage of this API requires a current license from Adobe. No unauthorized use is permitted.

## Overview {#preauth-overview}

The Preauthorize API method is to be used by applications to obtain preauthorization decisions for one or more resources. The Preauthorize API request should be used for UI hints and/or content filtering. An actual Authorization API request must be made before allowing user access to the specified resources.

In case an unexpected error (for example, network issue, and MVPD authorization endpoint unavailable) takes place when a Preauthorize API request is processed by the Adobe Pass Authentication services, then one or multiple separate error information will be included for the affected resources as part of the Preauthorize API response result.

### public preauthorize(request: PreauthorizeRequest, callback: AccessEnablerCallback&lt;any&gt;): void {#preauth-method}

**Description:** This method is to be used by applications to obtain authenticated user's preauthorization (informative) decisions from Adobe Pass Authentication service to view specific protected resources, for the primary purpose of decorating the application's UI (e.g. indicating access status with lock and unlock icons).

**Availability:** v4.4.0+ 

**Parameters:**

* `PreauthorizeRequest`: Builder Object used to define the request
* `AccessEnablerCallback`: callback used to return the API response
* `PreauthorizeResponse`: Object used to return the API response content

### class PreauthorizeRequestBuilder {#preath-req-builder-class}

#### setResources(resources: string[]): PreauthorizeRequestBuilder {#set-res-preath-req-buildr}

*   Sets the List of resources for which you want to obtain preauthorization decisions.
*   It is mandatory to set it for the usage of preauthorize API.
*   Each element in the list must be a String representing either the resource ID value or the media RSS fragment which must be agreed with the MVPD.
*   This method sets the information only in the context of current `PreauthorizeRequestBuilder` object instance, which is the receiver of this method call.

*   To build an actual `PreauthorizeRequest` you can have a look at `PreauthorizeRequestBuilder`'s method:

  ```JavaScript
    build(): PreauthorizeRequest
  ```

* `@param {string[]}` resources. The List of resources for which you want to obtain preauthorization decisions.
* `@returns {PreauthorizeRequestBuilder}` The reference to the same `PreauthorizeRequestBuilder` object instance, which is the receiver of the method call. 
* It does this to allow the creation of method chaining.

#### disableFeatures(...features: string[]): PreauthorizeRequestBuilder {#disabl-featres-preauth-req-buildr}

* Sets the features which you want to have them disabled when obtaining preauthorization decisions.
* This function sets the information only in the context of current `PreauthorizeRequestBuilder` object instance, which is the receiver of this function call.
* To build an actual `PreauthorizeRequest` you can have a look at `PreauthorizeRequestBuilder`'s function:

 ```JavaScript
 public func build() -> PreauthorizeRequest
 ```
 
* `@param {string[]}` features. The set of features which you want to have them disabled.
* `@returns` The reference to the same `PreauthorizeRequestBuilder` object instance, which is the receiver of the function call.
* It does this in order to allow the creation of function chaining.

#### build(): PreauthorizeRequest {#preauth-req}

* Creates and retrieves the reference of a new `PreauthorizeRequest` object instance.
* This method instantiates a new `PreauthorizeRequest` object every time it is called.
* This method uses the values set in advance in the context of current `PreauthorizeRequestBuilder` object instance, which is the receiver of this method call. 
* Bear in mind that this method does not produce any side effects,
* therefore it does not alter the state of the SDK or the state of the `PreauthorizeRequestBuilder` object instance, which is the receiver of this method call.
* It means that successive calls of this method for the same receiver will create different new `PreauthorizeRequest` object instances, but having the same information, in case the values set to the `PreauthorizeRequestBuilder` where not modified between the calls.
* In case you do not need to update any of the provided information (resources and caching) you may reuse the PreauthorizeRequest instance for multiple uses of the preauthorize API.
* `@returns {PreauthorizeRequest}`

### interface AccessEnablerCallback&lt;T&gt; {#interface-access-enablr-callback}

#### onResponse(result: T); {#on-response-result}

* Response callback called by the SDK when the preauthorize API request was fulfilled.
* The result is either a successful or an error result containing a status.
* `@param {T} result`

#### onFailure(result: T); {#on-failure-result}

* Failure callback called by the SDK when the preauthorize API request could not be serviced.
* The result is a failure result containing a status.
* `@param {T} result`

### class PreauthorizeResponse {#preauth-response-class}

#### public status: Status; {#public-status}

* Returns: Additional status (state) information in case of failure.
* Might hold a `null` value.

#### public decisions: Decision[]; {#public-decisions}

* Returns: The list of preauthorization decisions. One decision for each resource.
* The list might be empty in case of failure.

### class Status {#class-status}

#### public status: number; {#public-status-numbr}

* The HTTP response status code as documented in RFC 7231.
* Might be 0 in case the `Status` comes from the SDK instead of Adobe Pass Authentication services.

#### public code: number; {#public-code-numbr}

* The standard Adobe Pass Authentication services error code.
* Might hold an empty string or a `null` value.

#### public message: string; {#public-msg-string}

* The detailed message which in some cases is provided by the MVPD authorization endpoints or by Programmer degradation rules.
* Might hold an empty string or a `null` value.

#### public details: string; {#public-details-strng}

* Holds a detailed message which in some cases is provided by the MVPD authorization endpoints or by Programmer degradation rules.
* Might hold an empty string or a `null` value.


#### public helpUrl: string; {#public-help-url-string}

* The URL that links to more information about why this state/error occurred and possible solutions.
* Might hold an empty string or a `null` value.

#### public trace: string; {#public-trace-string}

* The unique identifier for this response, which can be used when contacting support to identify specific issues in more complex scenarios.
* Might hold an empty string or a `null` value.

#### public action: string; {#public-action-string}

*   The recommended action to remediate the situation.
    * **none**: Unfortunately there is no predefined action to remediate this issue. This might indicate an improper invocation of the public API
    * **configuration**: A configuration change is needed through TVE dashboard or by contacting support.
    * **application-registration**: The application must register itself again.
    * **authentication**: The user must authenticate or re-authenticate.
    * **authorization**: The user must obtain authorization for the specific resource.
    * **degradation**: Some form of degradation should be applied.
    * **retry**: Retrying the request might solve the issue
    * **retry-after**: Retrying the request after the indicated period of time might solve the issue.
*   Might hold an empty string or a `null` value.

### class Decision {#class-decision}

#### public id: string; {#public-id-string}

* The resource id for which the decision was obtained.

#### public authorized: boolean; {#public-auth-boolean}

* The value of the flag indicating if the decision is successful or not.

#### public error: Status; {#public-error-status}

* Additional status (state) information in case some error has occurred. Might hold a `null` value.

## Client implementation example {#client-imp-example}

```JavaScript

let accessEnablerApi = new window.AccessEnabler.AccessEnabler("software statement");
let accessEnablerModels = window.AccessEnabler.models;



// Build request
let requestBuilder = new accessEnablerModels.PreauthorizeRequest.getBuilder();
let request = requestBuilder
    .setResources(["RES01", "RES02", "RES03"])
    .disableFeatures("LOCAL_CACHE")
    .build();



// Create callback
let callback = {
    onResponse(response) {
        // Handle onResponse
    },
    onFailure(response) {
        // Handle onFailure
    }
};

// Invoke call
accessEnablerApi.preauthorize(request, callback);
```


## Scenario examples {#scenario-examples}

### Scenario 1: All requested resources were authorized {#all-req-res-auth}

<table>
<thead>
  <tr>
    <th>Enhanced error code flag</th>
    <th>Response</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Disabled</td>
    <td>

```JavaScript

        {
    "decisions": [
        {
        "id": "RES01",
        "authorized": true
        },
        {
        "id": "RES02",
        "authorized": true
        },
        {
        "id": "RES03",
        "authorized": true
        }
    ]
    }    
```

</td>
  </tr>
</tbody>


### Scenario 2: Some requested resources were authorized. {#sm-req-res-auth}

<table>
<thead>
  <tr>
    <th>Enhanced error code flag</th>
    <th>Response</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Disabled</td>
    <td>

    ```JavaScript

        {
    "decisions": [
        {
        "id": "RES01",
        "authorized": true
        },
        {
        "id": "RES02",
        "authorized": false
        },
        {
        "id": "RES03",
        "authorized": true
        }
    ]
    }
       
    ```

</td>
  </tr>

  <tr>
    <td>Enabled</td>
    <td>

    ```JavaScript
    {
      "decisions": [
        {
        "id": "RES01",
        "authorized": true
        },
        {
        "id": "RES02",
        "authorized": false,
        "error": {
            "status": 403,
            "code": "preauthorization_denied_by_mvpd",
            "message": "The MVPD has returned a \"Deny\" decision when requesting pre-authorization for the specified resource.",
            "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
            "action": "none"
        }
        },
        {
        "id": "RES03",
        "authorized": true
        },
    ]
    }
    
    ```

</td>
  </tr>
</tbody>


### Scenario 3: None of the requested resources were authorized. {#none-req-res-auth}

<table>
<thead>
  <tr>
    <th>Enhanced error code flag</th>
    <th>Response</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Disabled</td>
    <td>

    ```JavaScript

        {
    "decisions": [
        {
        "id": "RES01",
        "authorized": false
        },
        {
        "id": "RES02",
        "authorized": false
        },
        {
        "id": "RES03",
        "authorized": false
        }
    ]
    }
       
    ```

</td>
  </tr>

  <tr>
    <td>Enabled</td>
    <td>

    ```JavaScript

    {
    "decisions": [
        {
        "id": "RES01",
        "authorized": false,
        "error": {
            "status": 403,
            "code": "preauthorization_denied_by_mvpd",
            "message": "The MVPD has returned a \"Deny\" decision when requesting pre-authorization for the specified resource.",
            "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
            "action": "none"
            }
        },
        {
            "id": "RES02",
            "authorized": false,
            "error": {
                "status": 403,
                "code": "preauthorization_denied_by_mvpd",
                "message": "The MVPD has returned a \"Deny\" decision when requesting pre-authorization for the specified resource.",
                "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
                "action": "none"
            }
        },
        {
        "id": "RES03",
        "authorized": false,
        "error": {
            "status": 403,
            "code": "maximum_execution_time_exceeded",
            "message": "The request did not complete in the maximum allowed time. Retrying the request might solve the issue.",
            "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
            "action": "retry"
                }
            }
        ]
    }
    
    ```

</td>
  </tr>
</tbody>


### Scenario 4: Bad client request - no resources specified. {#bad-cl-req-no-res-sp}

<table>
<thead>
  <tr>
    <th>Enhanced error code flag</th>
    <th>Response</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Disabled/Enabled</td>
    <td>

    ```JavaScript
    {
    "status": {
    "status": 400,
    "code": "internal_error",
    "message": "The request failed due to an internal error.",
    "details": "Required String[] parameter 'resource' is not present",
    "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
    "action": "none"
    },
    "decisions": []
    }
    ```

</td>
  </tr>
</tbody>
</table>

### Scenario 5: Bad client request - empty resources specified. {#bad-cl-req-empt-res-sp}

<table>
<thead>
  <tr>
    <th>Enhanced error code flag</th>
    <th>Response</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Disabled/Enabled</td>
    <td>

    ```JavaScript
    {
    "status": {
    "status": 412,
    "code": "missing_resource",
    "message": "The resource parameter is missing",
    "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
    "action": "none"
    },
    "decisions": []
    }
    ```

</td>
  </tr>
</tbody>
</table>

### Scenario 6: Network error. {#ntwrk-error}

<table>
<thead>
  <tr>
    <th>Enhanced error code flag</th>
    <th>Response</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Enabled</td>
    <td>

    ```JavaScript
    {
    "decisions": [
        {
        "id": "RES01",
        "authorized": false,
        "error": {
            "status": 403,
            "code": "network_received_error",
            "message": "There was a read error while retrieving the response from the associated partner service. Retrying the request might solve the issue.",
            "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
            "action": "retry"
            }
        },
        {
            "id": "RES02",
            "authorized": false,
            "error": {
                "status": 403,
                "code": "network_received_error",
                "message": "There was a read error while retrieving the response from the associated partner service. Retrying the request might solve the issue.",
                "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
                "action": "retry"
                }   
        }
    ]
    }
    ```

</td>
  </tr>
</tbody>
</table>

### Scenario 7: Preauthorize flow was invoked without a valid AuthN session.

<table>
<thead>
  <tr>
    <th>Enhanced error code flag</th>
    <th>Response</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Disabled/Enabled</td>
    <td>

    ```JavaScript
    {
    "status": {
    "status": 0,
    "code": "authentication_session_missing",
    "message": "The authentication session associated with this request could not be retrieved. The user must re-authenticate with a supported MVPD in order to continue.",
    "action": "authentication"
    },
    "decisions": []
    }

    ```

</td>
  </tr>
</tbody>
</table>



### Scenario 8: Preauthorize flow was invoked before the setRequestor call was completed

<table>
<thead>
  <tr>
    <th>Enhanced error code flag</th>
    <th>Response</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Disabled/Enabled</td>
    <td>

    ```JavaScript
    {
    "status": {
    "status": 0,
    "code": "requestor_not_configured",
    "message": "The requestor is not yet configured which is a prerequisite for using any API apart from the setRequestor API.",
    "action": "retry"
    },
    "decisions": []
    }
    ```

</td>
  </tr>
</tbody>
</table>
