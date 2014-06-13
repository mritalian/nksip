# Responses API

This document describes the API NkSIP makes available to extract information from Responses.

Most functions in the API allows two ways to refer to the responses:
* From a full *response object* (`nksip:response()`). When you use the option `callback` when using the any [request sending function](../reference/sending_functions.md), it is called as `{reply, Code, Response, Call}`. You can use these API functions inside the function call.
* From a *response handle* (`nksip:handle()`). You can get a response handle from a response object using [get_handle/1](#nksip_responseget_handle1). You can then use the handle to call most functions in this API. 
    
    In this case, the API function must contact with the corresponding call process to get the actual response, so you cannot use this method _inside_ the same call process (like in the callback function). This method is useful to refer to the response from a _spawned_ process, avoiding the need to copy the full object. Please notice that the response object may not exists any longer at the moment that the handle is used. Most functions return `error` in this case.


<br/>


Function|Description
---|---
[get_handle/1](#nksip_responseget_handle1)|Grabs a response's handle
[app_id/1](#nksip_responseapp_id1)|Gets then SipApp's _internal name_
[app_name/1](#nksip_responseapp_name1)|Gets the SipApp's _user name_
[code/1](#nksip_responsemethod1)|Gets the SIP code of the response
[body/1](#nksip_responsebody1)|Gets the body of the response
[call_id/1](#nksip_responsecall_id1)|Gets the Call-ID header of the response
[meta/2](#nksip_responsemeta2)|Gets specific metadata from the response
[header/2](#nksip_responseheader2)|Gets the values for a header or headers in a response
[wait_491/0](#nksip_responsewait_4910)|Sleeps a random time between 2.1 and 4 secs


## Functions List

### nksip_response:get_handle/1
```erlang
-spec get_handle(nksip:response()|nksip:id()) ->
    nksip:id().
```
Grabs a response's handle.


### nksip_response:app_id/1
```erlang
-spec app_id(nksip:response()|nksip:id()) -> 
    nksip:app_id().
```
Gets then SipApp's _internal name_.


### nksip_response:app_name/1
```erlang
-spec app_name(nksip:response()|nksip:id()) -> 
    term().
```
Gets the SipApp's _user name_


### nksip_response:code/1
```erlang
-spec code(nksip:response()|nksip:id()) ->
    nksip:sip_code() | error.
```
Gets the SIP code of the response.


### nksip_response:body/1
```erlang
-spec body(nksip:response()|nksip:id()) ->
    nksip:body() | error.
```
Gets the body of the response.


### nksip_response:call_id/1
```erlang
-spec call_id(nksip:response()|nksip:id()) ->
    nksip:call_id().
```
Gets the Call-ID header of the response.


### nksip_response:meta/2
```erlang
-spec meta(Meta::nksip_sipmsg:field()|[nksip_sipmsg:field()], nksip:response()|nksip:id()) ->
    term() | [{nksip_sipmsg:field(), term()}] | error.
```
Gets specific metadata from the response.

See [Metadata Fields](../reference/metadata.md) for a description of available fields.
If `Meta` is simple term, its value is returned. If it is a list, it will return a list of tuples, where the first element is the field name and the second is the value.


### nksip_response:header/2
```erlang
-spec header(Name::string()|binary()|[string()|binary()], nksip:response()|nksip:id()) -> 
    [binary()] | [{binary(), binary()}] | error.
```
Gets the values for a header or headers in a response.

If `Name` is a single value, a list is returned with the values of all the headers having that name. If it is a list, a list of tuples is returned, where the first element is the header name and the second is the list of values.

NkSIP uses only lowercase for header names.


### nksip_response:wait_491/0
```erlang
-spec wait_491() -> 
    ok.
```
Sleeps a random time between 2.1 and 4 secs. It should be called after receiving a 491 response and before trying the response again.
