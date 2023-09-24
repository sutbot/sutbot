<!--
**sutbot/sutbot** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

-  ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
-  ...
- 😄 Pronouns: ...
-  ...
-->


# SUT Bot: Specification

📫 How to reach me: sam@manifoldfinance.com <br>
🔭 I’m currently working on: Automated Orchestrated Concurrent Testing and Remediation <br>
⚡ Fun fact: commands start with `mother`  <br>

[TOC]

## Abstract

The current paradigm for workflow based actuation and fullfilment typically consists of a DAG-oriented paradigm. We find this problematic as expressing different transition rules for different steps is prohibativly complex. Additionally, rules apply to all steps. Special rules for specific types of steps is difficult to implement, and worse, more difficult to maintain.

Additionally, reaching an end state determination debuging is difficult. End state is the state where the workflow can no longer be run. This can happen if the various steps end up in a state where there is no way to move them forward in with respect to the workflow's DAG for that job.

Each step is a state machine with its own states, actions and transition rules. A workflow is achieved by guarding actions on state machines with pre-conditions, which state the states other machines need to be in. An action can be performed when the other machines are in the states as specified in the pre-conditions. With this, one machine can be run only if its predecessors have successfully run, thereby achieving a workflow. However, since such pre-conditions can be specified for any state transition, arbitrary steps can be run based on transitions (e.g., if a step fails, a clean-up step can be run).


> TLDR: We specify a jobset building agent that can safely execute functions, especially in separate processes, and manage their results and exceptions.

### Walkthrough

Initialization:

Necessary modules are imported, and a logger is set up for logging exceptions and other messages.
Safe Function Execution:

The `exception_safe_call function` allows for the safe execution of any function, catching exceptions and returning them as part of a tuple.

The `make_exception_safe` function is a decorator that wraps any function to make it exception safe.

#### Callback Execution:

The `call_with_result_callback` function is a utility that calls a function and then executes a callback with the function's result.

#### Subprocess Execution:

The `run_function_as_execption_safe_subprocess` function runs a function in a separate subprocess, ensuring it's exception safe. The result or exception is placed in a queue for retrieval.

The ExceptionSafeSubProcessFunction class provides a more comprehensive way to manage functions executed in subprocesses. It allows for starting the subprocess, waiting for it, checking its status, retrieving results, and terminating it.



## Jobset Vector: `jvvset`

> TODO
> 
```mermaid
  stampCompare -> fork
  fork -> eventa
  event -> join
  join -> peek
  peek -> happenedBefore
```

### jvvset

| Name              | Description                             | Method      |
|-------------------|-----------------------------------------|-------------|
| newNodeController | Creates new node model                  | -           |
| forkController    | Clones node and returns two nodes       | -           |
| joinController    | Joins two nodes into one                | -           |
| eventController   | Increments value and adds event         | -           |
| itcNodeApi        | API layer                               | -           |
| itcNodeApi        | API layer for `newNodeController`       | newNode()   |
| itcNodeApi        | API layer for `forkController`          | fork()      |
| itcNodeApi        | API layer for `joinController`          | join()      |
| itcNodeApi        | API layer for `eventController`         | nodeEvent() |



### High-Level Overview:

Modules and Classes
Standalone Functions


### WorkflowAPIHandler

**State Management**: Methods that deal with the state of steps.
**Messaging**: Methods that send messages or commands to steps.
**Information Retrieval**: Methods that retrieve information about steps.



#### exception_safe_call Function

| Parameter  | Description                                                                                             |
|------------|---------------------------------------------------------------------------------------------------------|
| function   | Callable to be called with the args and kwargs given.                                                   |
| args       | The *args for the callable.                                                                             |
| kwargs     | The **kwargs for the callable.                                                                          |
| **Return** | A tuple of the form (<return_value>, <exception>) where: <return_value> is the value returned by the callable. <exception> is the exception raised by the callable. If no exception is raised, this will be None. |

#### make_exception_safe Function

| Parameter  | Description                                                                                             |
|------------|---------------------------------------------------------------------------------------------------------|
| function   | Callable to wrap.                                                                                      |
| **Return** | A callable which has the name and docstring of the wrapped function but it returns a tuple of the form (<return_value>, <exception>) where: <return_value> is the value returned by the callable. <exception> is the exception raised by the callable. If no exception is raised, this will be None. |

    
### call_with_result_callback Function

| Parameter  | Description                                                                                             |
|------------|---------------------------------------------------------------------------------------------------------|
| function   | The function to be called with args and kwargs.                                                         |
| callback   | The callable that will be called with the return value of the function.                                 |
| args       | Args for function.                                                                                     |
| kwargs     | Kwargs for function.                                                                                   |
| **Return** | None.                                                                                                  |


### run_function_as_execption_safe_subprocess

    
| Paraeter  | Description                                                                                             |
|------------|---------------------------------------------------------------------------------------------------------|
| function   | The function to be called.                                                                             |
| args       | Args for function.                                                                                     |
| kwargs     | Kwargs for function.                                                                                   |
| **Return** | A tuple consisting of the following: process: The multiprocessing.Process object that is running the function. result queue: A read-only multiprocessing.Queue that will contain only one message; a tuple of the form: (<return_value>, <exception>) where: <return_value> is the value returned by the callable. <exception> is the exception raised by the callable. If no exception is raised, this will be None. |

    


#### start Method

| Parameter  | Description                                                                                             |
|------------|---------------------------------------------------------------------------------------------------------|
| args       | The args compatible with the function this is initialized with.                                         |
| kwargs     | Keyword arguments compatible with the function this is initialized with.                                |
| **Return** | None                                                                                                   |





## Operator 

Navigation paths to resources are generally broken into multiple segments, `{scheme}://{host}/{version}/{category}/[{pathSegment}][?{query}]` where:
    
- `version` can be v1.0 or beta.
    
- `category` is a logical grouping of APIs into top-level categories.
    
- `pathSegment` is one or many navigation segments that can address an entity, collection of entities, property, or operation available for an entity.
    
- `query` string must follow the OData standard for query representations 

####  Operator precedence

Services MUST use the following operator precedence for supported operators when evaluating _$filter_ expressions.
Operators are listed by category in order of precedence from highest to lowest.
Operators in the same category have equal precedence:

| Group           | Operator | Description           |
|:----------------|:---------|:----------------------|
| Grouping        | ( )      | Precedence grouping   |
| Unary           | not      | Logical Negation      |
| Relational      | gt       | Greater Than          |
|                 | ge       | Greater than or Equal |
|                 | lt       | Less Than             |
|                 | le       | Less than or Equal    |
| Equality        | eq       | Equal                 |
|                 | ne       | Not Equal             |
| Conditional AND | and      | Logical And           |
| Conditional OR  | or       | Logical Or            |


####  Operations resource
Services MAY provide a "/operations" resource at the tenant level.

Services that provide the "/operations" resource MUST provide GET semantics.
`GET` **MUST** enumerate the set of operations, following standard pagination, sorting, and filtering semantics.
The default sort order for this operation MUST be:

Primary Sort           | Secondary Sort
---------------------- | -----------------------
Not Started Operations | Operation Creation Time
Running Operations     | Operation Creation Time
Completed Operations   | Operation Creation Time

> **Note**    
>  that "Completed Operations" is a goal state (see below), and may actually be any of several different states such as "successful", "cancelled", "failed" and so forth.

#### Operation resource

An operation is a user addressable resource that tracks a stepwise long running operation.
Operations **MUST** support `GET` semantics.
The `GET` operation against an operation **MUST** return:

1. The operation resource, it's state, and any extended state relevant to the particular API.
2. 200 OK as the response code.

Services *MAY* support operation cancellation by exposing `DELETE` on the operation.
If supported `DELETE` operations **MUST** be idempotent.

> Note: From an API design perspective, cancellation does not explicitly mean rollback.
On a per-API defined case it may mean rollback, or compensation, or completion, or partial completion, etc.
Following a cancelled operation, It **SHOULD NOT** be a client's responsibility to return the service to a consistent state which allows continued service.

Services that do not support operation cancellation **MUST** return a 405 Method Not Allowed in the event of a `DELETE`.

#### Operations States

Operations **MUST** support the following states:

1. NotStarted
2. Running
3. Succeeded. Terminal State.
4. Failed. Terminal State.
5. Unresponsive (Zombie)
6. Timeout
7.  Optional: Cancelling, Cancelled, Aborting, Aborted, Tombstone, Deleting, Deleted.

Services *MAY* add additional states, such as "Cancelled" or "Partially Completed". Services that support cancellation **MUST** sufficiently describe their cancellation such that the state of the system can be accurately determined, and any compensating actions may be run.

Services that support additional states should consider this list of canonical names and avoid creating new names if possible: Cancelling, Cancelled, Aborting, Aborted, Tombstone, Deleting, Deleted.

#### Operation Response

An operation **MUST** contain, and provide in the `GET` response, the following information:

1. The timestamp when the operation was created.
2. A timestamp for when the current state was entered.
3. The operation state (notstarted / running / completed).

Services *MAY* add additional, API specific, fields into the operation.
The operation status JSON returned looks like:

```json
{
  "createdDateTime": "2015-06-19T12-01-03.45Z",
  "lastActionDateTime": "2015-06-19T12-01-03.45Z",
  "status": "notstarted | running | succeeded | failed"
}
```

##### Percent complete
Sometimes it is impossible for services to know with any accuracy when an operation will complete.

Which makes using the Retry-After header problematic.
In that case, services MAY include, in the operationStatus JSON, a percent complete field.

```json
{
  "createdDateTime": "2015-06-19T12-01-03.45Z",
  "percentComplete": "50",
  "status": "running"
}
```

In this example the server has indicated to the client that the long running operation is 50% complete.

##### Target resource location
For operations that result in, or manipulate, a resource the service MUST include the target resource location in the status upon operation completion.

```json
{
  "createdDateTime": "2015-06-19T12-01-03.45Z",
  "lastActionDateTime": "2015-06-19T12-06-03.0024Z",
  "status": "succeeded",
  "resourceLocation": "https://api.contoso.com/v1.0/databases/db1"
}
```

####  Operation tombstones
Services *MAY* choose to support tombstoned operations.
Services *MAY* choose to delete tombstones after a service defined period of time.

#### The typical flow, polling
- Client invokes a stepwise operation by invoking an action using POST
- The server MUST indicate the request has been started by responding with a 202 Accepted status code. The response SHOULD include the location header containing a URL that the client should poll for the results after waiting the number of seconds specified in the Retry-After header.
- Client polls the location until receiving a 200 response with a terminal operation state.

##### Example of the typical flow, polling
Client invokes the restart action:

```http
POST https://FQDN/v1.0/databases HTTP/1.1
Accept: application/json

{
  "fromFile": "myFile.db",
  "color": "red"
}
```

The server response indicates the request has been created.

```http
HTTP/1.1 202 Accepted
Operation-Location: https://api.FQDN.com/v1.0/operations/123
```

Client waits for a period of time then invokes another request to try to get the operation status.

```http
GET https://api.FQDN.com/v1.0/operations/123
Accept: application/json
```

Server responds that results are still not ready and optionally provides a recommendation to wait 30 seconds.

```http
HTTP/1.1 200 OK
Retry-After: 30

{
  "createdDateTime": "2023-06-19T12-01-03.4Z",
  "status": "running"
}
```

Client waits the recommended 30 seconds and then invokes another request to get the results of the operation.

```http
GET https://api.FQDN.com/v1.0/operations/123
Accept: application/json
```

Server responds with a "status:succeeded" operation that includes the resource location.

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "createdDateTime": "2023-06-19T12-01-03.45Z",
  "lastActionDateTime": "2023-06-19T12-06-03.0024Z",
  "status": "succeeded",
  "resourceLocation": "https://FQDN/v1.0/databases/db1"
}
```


## API

| Parameter                 | Type/Description                                                                                                      |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------|
| `success_pairs`           | A list of tuples. Each tuple is an ordered pair associating a step with its successor when it successfully runs. E.g., `[(a, b), (b, c)]` results in: `a --[if successful]--> b --[if successful]--> c` |
| `failure_pairs`           | Similar to `success_pairs`, but links failures. E.g., `[(a, b)]` results in: `a --[if failed]--> b`                    |
| `context`                 | The context dictionary.                                                                                               |
| `context_serializer`      | A `core.api.serializers.Serializer` like callable that can be used to serialize the context object.                    |
| `socket_file`             | The socket file name to be used for communicating with the workflow API server.                                       |
| `workflow_delay`          | Introduce a delay in the loop of the state machine evaluations. This delay affects how frequently callbacks are called.|
| `api_delay`               | The delay used by the API server. This decides how frequently the server listens for API messages.                     |
| `transition_rules`        | The rules for state transitions as a mapping.                                                                         |
| `initial_state`           | The initial state of the machines. This is a single state that will be applied to all the machines.                   |
| `action_evaluations`      | A mapping expressing the rules and functions for automatic state transitions.                                         |
| `final_callback_function` | Callable to execute when the final states are reached.                                                                |
| `machine_serializer`      | An object like `core.api.serializers.Serializer` to serialize the machines.                                           |
| `api_handlers`            | An object to handle API calls.                                                                                        |


## API

- Each `method`:
   - Function Name
   - Description 
   - Parameters 
   - Returns 


### `filter_steps_by_states`

**Description**:
Filter step objects that are in the given states.

| Parameter     | Description                                                                                                         |
|---------------|---------------------------------------------------------------------------------------------------------------------|
| `steps`       | An iterable of step objects.                                                                                        |
| `step_states` | A dictionary of the form: `{ <machine 1 name (str)>: <state of machine 1 (str)>, ... }`                             |
| `states`      | The collection of states to filter by.                                                                              |

**Returns**: A generator of step objects that are in the given states.


### `filter_steps_by_tags`

**Description**:
Filter step objects by the given tags.

| Parameter | Description                                                                                                     |
|-----------|-----------------------------------------------------------------------------------------------------------------|
| `steps`   | An iterable of step objects.                                                                                    |
| `tags`    | Tags are arbitrary key-value pairs (dictionary) that can be associated with a step.                             |

**Returns**: A generator of step objects that have the given tags associated with them.


### `filter_steps_by_action`

**Description**:
Filter step objects on which, the given action can be performed.

| Parameter     | Description                                                                                                         |
|---------------|---------------------------------------------------------------------------------------------------------------------|
| `steps`       | An iterable of step objects.                                                                                        |
| `transitions` | A dictionary of the form: `{ <machine 1 name (str)>: [<possible action for machine 1 (str)>, ...], ... }`           |
| `action`      | The action to filter by (str).                                                                                      |

**Returns**: (No specific return description provided)


### `extract_step_ids`

**Description**:
Extract the 'id' attribute of each step.

| Parameter | Description                       |
|-----------|-----------------------------------|
| `steps`   | An iterable of step objects.      |

**Returns**: A generator of step IDs.


### `get_class_globals`

**Description**:
Returns a set of the globals defined in the given class. Globals are identified to be uppercase and do not start with an underscore.

| Parameter | Description                                         |
|-----------|-----------------------------------------------------|
| `klass`   | The class whose globals need to be fetched.          |

**Returns**: A set of class globals


### `make_api_client`

**Description**:
Factory to create a MethodAPIClientWrapper that communicates using a SocketClient.

| Parameter     | Description                                                                                     |
|---------------|-------------------------------------------------------------------------------------------------|
| `socket_file` | A Unix socket file that will be used to communicate.                                            |
| `timeout`     | The timeout (in seconds) while waiting for a response.                                          |

**Returns**: A MethodAPIClientWrapper that serves requests at the given Unix socket file.




# `WorkflowAPIHandler` class



### `WorkflowAPIHandler.__init__`

**Description**:
Initialise the API handler.

| Parameter          | Description                                                                                                     |
|--------------------|-----------------------------------------------------------------------------------------------------------------|
| `steps`            | An iterable of step objects (similar to default_workflow.step.Step).                                            |
| `callback_manager` | A managed callback callable similar to core.api.callbacks.ManagedCallback.                                      |
| `process`          | The process running the state machine evaluator.                                                                |



### `WorkflowAPIHandler.get_serialized_context`

**Description**:
Get a serialized copy of the current state of the context.

**Returns**: An APIHandlerResponse object whose 'return_value' is the current state of the context as a dictionary or other serialized structure.



### `WorkflowAPIHandler.start`

**Description**:
Start a workflow. This will change the state of all steps in 'Ready' state to 'Waiting' state.

| Parameter | Description                                                                                                     |
|-----------|-----------------------------------------------------------------------------------------------------------------|
| `dry_run` | Boolean. API call doesn't have any effect when True.                                                           |

**Returns**: An APIHandlerResponse object whose 'return_value' is the list of step IDs that were started.


### `WorkflowAPIHandler.shutdown`

**Description**:
Shutdown the workflow server. After this call, no other API calls can be served.

| Parameter | Description                                                                                                     |
|-----------|-----------------------------------------------------------------------------------------------------------------|
| `dry_run` | Boolean. API call doesn't have any effect when True.                                                           |

**Returns**: An APIHandlerResponse object whose 'return_value' is the list of step IDs that were interrupted and 'relay_value' is set to SocketServer.SHUTDOWN to signal the server to stop serving requests.


### `WorkflowAPIHandler.send_message_to_steps`

**Description**:
Send a message to matching steps.

| Parameter  | Description                                                                                                     |
|------------|-----------------------------------------------------------------------------------------------------------------|
| `message`  | Any JSON encodable Python object.                                                                               |
| `dry_run`  | Boolean. API call doesn't have any effect when True.                                                           |
| `tags`     | Any key=value pair provided in the arguments is treated as a tag, except for dry_run=True.                      |

**Returns**: An APIHandlerResponse object whose 'return_value' is the list of step IDs to which the message was sent.


### `WorkflowAPIHandler.list`

**Description**:
List all the steps' tags in a workflow (in topological order).

| Parameter | Description                                                                                                     |
|-----------|-----------------------------------------------------------------------------------------------------------------|
| `tags`    | Any key=value pair provided in the arguments is treated as a tag, except for dry_run=True.                      |

**Returns**: An APIHandlerResponse object whose 'return_value' is the list of dictionaries (tags) of the matching steps.


### `WorkflowAPIHandler.status`

**Description**:
Get workflow status.

| Parameter | Description                                                                                                     |
|-----------|-----------------------------------------------------------------------------------------------------------------|
| `fields`  | List of field names to return in the results.                                                                   |
| `states`  | List of strings that represent the states of a Step.                                                            |
| `tags`    | Any key=value pair provided in the arguments is treated as a tag, except for dry_run=True.                      |

**Returns**: An APIHandlerResponse object whose 'return_value' is the list of dictionaries, each containing the status of the matching steps.


### `WorkflowAPIHandler.steps_waiting_for_user_input`

**Description**:
Get status of steps that are waiting for user input.

| Parameter | Description                                                                                                     |
|-----------|-----------------------------------------------------------------------------------------------------------------|
| `tags`    | Any key=value pair provided in the arguments is treated as a tag, except for dry_run=True.                      |

**Returns**: An APIHandlerResponse object whose 'return_value' is the list of step IDs that are waiting for user input.

Alright, let's continue with the extraction of the remaining methods within the `WorkflowAPIHandler` class and then move on to the standalone functions.

---

### `WorkflowAPIHandler.pause`

**Description**:
Send message to pause steps. Mark the specified steps so that they are paused. A step can only be paused if it is ready or waiting.

| Parameter | Description                                                                                                     |
|-----------|-----------------------------------------------------------------------------------------------------------------|
| `dry_run` | Boolean. API call doesn't have any effect when True.                                                           |
| `tags`    | Any key=value pair provided in the arguments is treated as a tag, except for dry_run=True.                      |

**Returns**: An APIHandlerResponse object whose 'return_value' is the list of step IDs that were paused.

---

### `WorkflowAPIHandler.interrupt`

**Description**:
Send message to interrupt running steps. This will interrupt the running steps by sending them SIGINT. A step can be interrupted only if it is running.

| Parameter | Description                                                                                                     |
|-----------|-----------------------------------------------------------------------------------------------------------------|
| `dry_run` | Boolean. API call doesn't have any effect when True.                                                           |
| `tags`    | Any key=value pair provided in the arguments is treated as a tag, except for dry_run=True.                      |

**Returns**: An APIHandlerResponse object whose 'return_value' is the list of step IDs that were interrupted.

---

### `WorkflowAPIHandler.resume`

**Description**:
Send message to resume steps. Mark the specified steps so that they are resumed. A step can only be resumed if it is either paused, marked to pause, paused due to failure or interrupted.

| Parameter | Description                                                                                                     |
|-----------|-----------------------------------------------------------------------------------------------------------------|
| `dry_run` | Boolean. API call doesn't have any effect when True.                                                           |
| `tags`    | Any key=value pair provided in the arguments is treated as a tag, except for dry_run=True.                      |

**Returns**: An APIHandlerResponse object whose 'return_value' is the list of step IDs that were resumed.

---

### `WorkflowAPIHandler.rerun`

**Description**:
Send message to resume steps. Mark the specified steps so that they are resumed. A step can only be resumed if it is either paused, marked to pause, paused due to failure or interrupted.

| Parameter | Description                                                                                                     |
|-----------|-----------------------------------------------------------------------------------------------------------------|
| `dry_run` | Boolean. API call doesn't have any effect when True.                                                           |
| `tags`    | Any key=value pair provided in the arguments is treated as a tag, except for dry_run=True.                      |

**Returns**: An APIHandlerResponse object whose 'return_value' is the list of step IDs that were re-run.

---

### `WorkflowAPIHandler.skip`

**Description**:
Send message to mark steps to be skipped. Mark the specified steps so that they are skipped when execution reaches them. A step can only be skipped if it is either paused, marked to pause, paused due to failure, waiting or ready.

| Parameter | Description                                                                                                     |
|-----------|-----------------------------------------------------------------------------------------------------------------|
| `dry_run` | Boolean. API call doesn't have any effect when True.                                                           |
| `tags`    | Any key=value pair provided in the arguments is treated as a tag, except for dry_run=True.                      |

**Returns**: An APIHandlerResponse object whose 'return_value' is the list of step IDs that were marked to be skipped.

---

### `WorkflowAPIHandler.unskip`

**Description**:
Send message to un-skip steps that have been marked to be skipped. A step can be un-skipped only if it is marked to be skipped but has not been skipped yet. Once a step has already been skipped, this API call will not affect them (they cannot be unskipped).

| Parameter | Description                                                                                                     |
|-----------|-----------------------------------------------------------------------------------------------------------------|
| `dry_run` | Boolean. API call doesn't have any effect when True.                                                           |
| `tags`    | Any key=value pair provided in the arguments is treated as a tag, except for dry_run=True.                      |

**Returns**: An APIHandlerResponse object whose 'return_value' is the list of step IDs that were unmarked from being skipped.

---

### `make_api_client`

**Description**:
Factory to create a MethodAPIClientWrapper that communicates using a SocketClient.

| Parameter    | Description                                                                                                     |
|--------------|-----------------------------------------------------------------------------------------------------------------|
| `socket_file`| A Unix socket file that will be used to communicate.                                                            |
| `timeout`    | The timeout (in seconds) while waiting for a response.                                                          |

**Returns**: A MethodAPIClientWrapper that serves requests at the given Unix socket file.

