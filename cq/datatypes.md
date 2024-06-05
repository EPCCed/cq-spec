# CQ datatypes


## Basic datatypes

| Datatype | Description | Implementation Notes |
| -------- | ----------- | -------------------- |
| `qubit`    | Classical handle for a **quantum** resource. A single qubit. |  Not guaranteed to be valid until handle is passed to an allocator function. Handles should be unique. |
| `cstate`  | Datatype capable of holding the outcome of measuring a single qubit - a classical state. | Trivially implemented as a bool. |
| `gate_id`  | Handle for a speccific _type_ of quantum gate, i.e. Hadamard. | Could be a function pointer, or an map key. | 


## Compound datatypes

### Gate struct

The gate struct is primarily intended to store the details of a specific instance of a **quantum** gate, to enable the creation of datastructures representing quantum circuits. It combines a reference to the functional description of a gate, and parameters required for that function.

```C
struct gate {
  gate_id id;
  qubit * qp;
  size_t n_qubits;
  complex double * params;
  size_t n_params;
}
```

| Member | Datatype | Notes |
| ------ | -------- | ----- |
| id     | `gate_id` | A reference to the functional description of the gate. |
| qp     | `qubit *` | Classical handle for one or more qubits, to which the gate should be applied. Order should match the order of formal parameters in the interface specified by `gate_id`. |
| n_qubits | `size_t` | The number of qubits to which the gate applies. Should be equal to `sizeof(qp)/sizeof(qubit)`. |
| params | `complex double *` | Parameters for the gate i.e. rotation angles. Order should match the order of formal parameters in the interface specified by `gate_id`. |
| n_params | `size_t` | The number of parameters. Should be equal to `sizeof(params)/sizeof(size_t)`. | 