# CQ datatypes


## Basic datatypes

| Datatype | Description | Implementation Notes |
| -------- | ----------- | -------------------- |
| `qubit`    | Classical handle for a **quantum** resource. A single qubit. |  Not guaranteed to be valid until handle is passed to an allocator function. Handles should be unique. |
| `cstate`  | Datatype capable of holding the outcome of measuring a single qubit - a classical state. | Trivially implemented as a bool. |
| `gate_id`  | Handle for a specific _type_ of quantum gate, i.e. Hadamard. | Could be a function pointer, or a map key. |
| `backend_id` | Handle for a specific backend in quantum-heterogeneous environments. | Could be an unsigned int, or a map key. |


## Compound datatypes

Note that implementers may wish to extend these compound datatypes, but ideally they should not reduce them.

### Gate struct

The gate struct is primarily intended to store the details of a specific instance of a **quantum** gate, to enable the creation of datastructures representing quantum circuits. It combines a reference to the functional description of a gate, and parameters required for that function.

```C
struct gate {
  gate_id id;
  qubit * targets;
  size_t n_targets;
  qubit * controls;
  size_t n_controls;
  complex double * params;
  size_t n_params;
}
```

| Member | Datatype | Notes |
| ------ | -------- | ----- |
| id     | `gate_id` | A reference to the functional description of the gate. |
| targets | `qubit *` | Classical handle for one or more qubits, to which the gate should be applied. |
| n_targets | `size_t` | The number of qubits to which the gate applies. Should be equal to `sizeof(targets)/sizeof(qubit)`. |
| controls | `qubit *` | Classical handle for zero or more qubits which act as controls for the gate that should be applied. |
| n_controls | `size_t` | The number of qubits by which the gate is controlled. Should be equal to `sizeof(controls)/sizeof(qubit)`. |
| params | `complex double *` | Parameters for the gate i.e. rotation angles. |
| n_params | `size_t` | The number of parameters. Should be equal to `sizeof(params)/sizeof(size_t)`. |

### Execution handle struct

The execution handle struct is intended to store the status of an execution of a circuit on a **quantum** device. It can be used to check when the cstate buffer is full, or partially full, and also to return status codes from the **quantum** device. Note that this struct should be a shared datastructure between the host and the classical co-processor of the **quantum** device, and depending on implementation _may_ need to be explicitly synchronised by the user.  

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
