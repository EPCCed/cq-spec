# CQ datatypes


## Basic datatypes

| Datatype | Description | Implementation Notes |
| -------- | ----------- | -------------------- |
| `qubit`    | Classical handle for a **quantum** resource. A single qubit. |  Not guaranteed to be valid until handle is passed to an allocator function. Handles should be unique. |
| `cstate`  | Datatype capable of holding the outcome of measuring a single qubit - a classical state. | Trivially implemented as a bool, _however_ it is very useful to use a type that can also represent an unset state. |
| `backend_id` | Handle for a specific backend in quantum-heterogeneous environments. | Could be an unsigned int, or a map key. |
| `qkern` | Function pointer to a quantum kernel. The specific signature of a quantum kernel is left as an implementation detail, but valid quantum kernels should be fully executable on the _device_. |
| `pqkern` | Function pointer to a _parameterised_ quantum kernel. The specific signature of a paramaterised quantum kernel is left as an implementation detail, but valid quantum kernels should be fully executable on the _device_, and it should accept a `struct qkern_parameters`. |


## Compound datatypes

Note that implementers may wish to extend these compound datatypes, but ideally they should not reduce them.

### Execution handle struct

The execution handle struct is intended to store the status of an execution of a kernel on a **quantum** device. It can be used to check when the cstate buffer is full, or partially full, and also to return status codes from the **quantum** device. Note that this struct should be a shared datastructure between the host and the classical co-processor of the **quantum** device, and depending on implementation _may_ need to be explicitly synchronised by the user.  

```C
struct exec {
  bool complete;
  unsigned int qdev_status;
  size_t completed_shots;
}
```

| Member | Datatype | Notes |
| ------ | -------- | ----- |
| complete | `bool` | Flag indicating whether the execution has fully completed or not. Access to the executors result buffer is not guaranteed to be safe until this is `TRUE`. |
| qdev_status | `unsigned int` | Device status code, usage dependent on hardware vendor. Could be used to indicate a fault preventing further execution. |
| completed_shots | `size_t` | A count of shots that have been completed, most relevant to mult-shot executions. Synchronisation is implementation dependent, but _must_ always be accurate when `complete` is `TRUE`. |

### Kernel parameter struct

The user may define their own `qkern_parameters` struct in order to pass arbitrary parameters into their quantum kernels. The importance of using this struct to do so is that it allows us to strictly define a function pointer for parameterised quantum kernels, which can then be registered by the implementation to the quantum device.

```C
struct qkern_parameters {
  ...
}
```