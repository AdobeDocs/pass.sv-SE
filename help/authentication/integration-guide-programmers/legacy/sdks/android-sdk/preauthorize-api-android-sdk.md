---
title: Förauktorisera Android
description: Förauktorisera Android
exl-id: b5337595-135f-4981-a578-2da432f125d6
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# (Äldre) Förauktorisera {#preuthorize-android}

>[!NOTE]
>
>Innehållet på den här sidan tillhandahålls endast i informationssyfte. Användning av denna API kräver en aktuell licens från Adobe. Ingen obehörig användning är tillåten.

</br>


API-metoden för förhandsauktorisering måste användas av program för att ett beslut om förhandsauktorisering ska kunna tas för en eller flera resurser. Förauktoriserings-API-begäran ska användas för användargränssnittstips och/eller innehållsfiltrering. En faktisk begäran om auktoriserings-API måste göras innan användaren kan bevilja åtkomst till de angivna resurserna.



Om ett oväntat fel inträffar (t.ex. nätverksproblem, slutpunkten för MVPD-auktorisering är inte tillgänglig osv.) när en förauktoriserad API-begäran bearbetas av Adobe Pass Authentication-tjänster, kommer en eller flera separerade felinformation att inkluderas för den eller de berörda resurserna som en del av förauktoriserings-API-svarsresultatet.


## `public void preauthorize(PreauthorizeRequest request, AccessEnablerCallback<PreauthorizeResponse> callback);`


**Beskrivning:**

**Tillgänglighet:** v3.6.0+

**Parametrar:**

- *PreauthorizedRequest*: Builder-objektet som används för att definiera begäran
- AccessEnablerCallback : återanrop som används för att returnera API-svar
- PreAuthzeResponse : Objektet som används för att returnera API
svarsinnehåll


### public class PreAuthzeRequest {#androidpreauthorizerequest}

**klassen PreAuthzeRequest.Builder**

```java
    ///
    /// Sets the `List` of resources for which you want to obtain preauthorization decisions.
    ///
    /// Each element in the list must be a `String` representing either the resource ID value or the media RSS fragment which must be agreed with the MVPD.
    ///
    /// This function sets the information only in the context of current `Builder` object instance which is the receiver of this function call.
    ///
    /// To build an actual `PreauthorizeRequest` you can have a look at `Builder`'s function:
    /// ```
    /// public func build() -> PreauthorizeRequest
    /// ```
    ///
    /// - Parameter resources: The `List` of resources for which you want to obtain preauthorization decisions.
    ///
    /// - Returns: The reference to the same `Builder` object instance which is the receiver of the function call. It does this in order to allow the creation of function chaining.
    ///
```

**public Builder setResources(List\&lt;String\> resources)**

```
    ///
    /// Sets the features which you want to have them disabled when obtaining preauthorization decisions.
    ///
    /// The list of available features are provided by `PreauthorizeRequest.Feature` enumeration.
    ///
    /// This function sets the information only in the context of current `Builder` object instance which is the receiver of this function call.
    ///
    /// To build an actual `PreauthorizeRequest` you can have a look at `Builder`'s function:
    /// ```
    /// public func build() -> PreauthorizeRequest
    /// ```
    ///
    /// - Parameter features: The set of features which you want to have them disabled.
    ///
    /// - Returns: The reference to the same `Builder` object instance which is the receiver of the function call. It does this in order to allow the creation of function chaining.
    ///
```


**public Builder disableFeatures(Set\&lt;PreAuthzeRequest.Feature\>
funktioner)**

```
    ///
    /// Creates and retrieves the reference of a new `PreauthorizeRequest` object instance.
    ///
    /// This function instantiates a new `PreauthorizeRequest` object every time it is called.
    ///
    /// This function uses the values set in advance in the context of current `Builder` object instance which is the receiver of this function call.
    ///
    /// Bear in mind that this function does not produce any side effects, therefore it does not alter the state of the SDK or the state of the `Builder` object instance which is the receiver of this function call.
    ///
    /// It means that successive calls of this function for the same receiver will create different new `PreauthorizeRequest` object instances, but having the same information, in case the values set to the `Builder` where not modified between the calls.
    ///
    /// In case you do not need to update any of the provided information (resources, features, etc.) you may reuse the `PreauthorizeRequest` instance for multiple uses of the `preauthorize` API.
    ///
    /// - Returns: The reference to a new `PreauthorizeRequest` object instance.
    ///
```

**public PreauthorizedRequest build()**

**enum PreAuthzeRequest.Feature**

```java
    ///
    /// This feature controls whether to use the information from the AccessEnabler SDK cache or to bypass it and
    /// rely on Adobe Pass server information via a network call.
    ///
    LOCAL_CACHE

    ///
    /// This feature controls whether to use the information from the Adobe Pass server cache or to bypass it and
    /// rely on MVPD server information via a network call.
    ///

    REMOTE_CACHE
```


### `abstract class AccessEnablerCallback<PreauthorizeResponse>` {#accessenablercallback}

```java
    /// Response callback called by the SDK when the preauthorize API request was fulfilled. The result is either a successful or an error result containing a status.

**public void onResponse(PreauthorizeResponse result)**

 

    /// Failure callback called by the SDK when the preauthorize API request could not be serviced. The result is a failure result containing a status. 

**public void onFailure(PreauthorizeResponse result)**
```



### class PreAuthzeResponse {#preauthorizeresponse}

```java
    ///
    /// - Returns: Additional status (state) information in case of failure.
    ///   Might hold a `null` value.
    ///

**public [Status](#status) getStatus()**

 

    ///
    /// - Returns: The list of preauthorization decisions. One decision for each resource.
    ///            The list might be empty in case of failure.
    ///

**public List\<[Decision](#status)\> getDecisions()**
```


**klassstatus** {#status}

```java
///
/// - Returns: The HTTP response status code as documented in RFC 7231.
/// Might be 0 in case the \`Status\` comes from the SDK instead of Adobe Pass Authentication services.

///

**public int getStatus()**

    ///
    /// - Returns: The standard Adobe Pass Authentication services error code.
    ///            Might hold an empty string or a `null` value.
    ///

**public String getCode()**

    ///
    /// - Returns: The human readable message which can be displayed to the end user.
    ///            Might hold an empty string or a `null` value.
    ///

**public String getMessage()**

    ///
    /// - Returns: The detailed message which in some cases is provided by the MVPD authorization endpoints or by Programmer degradation rules.
    ///            Might hold an empty string or a `null` value.
    ///

**public String getDetails()**

    ///
    /// - Returns: The URL that links to more information about why this state/error occurred and possible solutions.
    ///            Might hold an empty string or a `null` value.
    ///

**public String getHelpUrl()**

    ///
    /// - Returns: The unique identifier for this response, which can be used when contacting support to identify specific issues in more complex scenarios.
    ///            Might hold an empty string or a `null` value.
    ///

**public String getTrace()**

    ///
    /// - Returns: The recommended action to remediate the situation.
    ///             - none: Unfortunately there is no predefined action to remediate this issue. This might indicate an improper invocation of the public API
    ///             - configuration: A configuration change is needed through TVE dashboard or by contacting support.
    ///             - application-registration: The application must register itself again.
    ///             - authentication: The user must authenticate or re-authenticate.
    ///             - authorization: The user must obtain authorization for the specific resource.
    ///             - degradation: Some form of degradation should be applied.
    ///             - retry: Retrying the request might solve the issue
    ///             - retry-after: Retrying the request after the indicated period of time might solve the issue.
    ///            Might hold an empty string or a `null` value.
    ///

**public String getAction()**
```

</br>

>**class Decision** {#decision}

```
    ///
    /// This is a getter function.
    ///
    /// - Returns: The resource id for which the decision was obtained.
    ///

    public Status getId()

 

    ///
    /// This is a getter function.
    ///
    /// - Returns: The value of the flag indicating if the decision is successful or not.
    ///

**public boolean isAuthorized()**

 

    ///
    /// This is a getter function.
    ///
    /// - Returns: Additional status (state) information in case some error has occurred.
    ///            Might hold a `null` value.
    ///

**public Status getError()**
```

</br>



Exempel:


```java
    // build request object 
    Preauthorize request = new PreauthorizeRequest.Builder()
                .setResources(resourcesList)
                .disableFeatures(PreauthorizeRequest.FEATURE.LOCAL_CACHE) // optional, use only to disable local cache for preauthorized resources
                .build();
    }
    
    // execute preauthorize call, passing a callback function to process the response
    accessEnabler.preauthorize(request, new AccessEnablerCallback<PreauthorizeResponse>() {
        @Override
        public void onResponse(PreauthorizeResponse result) {
            // Handle onResponse
        }
    
        @Override
        public void onFailure(PreauthorizeResponse result) {
            // Handle onFailure
        }
    });
```
