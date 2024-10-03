# CQ quantum operations

Quantum operations are valid _within_ a quantum kernel.

## Resource management

### Set qubit

We assume only classical states can be prepared trivially.

```C
void set_qubit(qubit qh, cstate cs);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qh`      | `qubit`  | Out     | Handle to the qubit which is to be prepared in a classical state. |
| `cs`      | `cstate` | In     | The classical state in which the qubit is to be prepared. |

### Set qubit register using unsigned integer

```C
void set_qureg(const unsigned long long STATE_IDX, const size_t N, qubit * qrp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `STATE_IDX` | `const unsigned long long` | In | Converted to an `N`-bit binary number, which is then used to set the qubit register. |
| `N`       | `const size_t` | In | The number of qubits in `qrp`. Should be equal `sizeof(qrp)/sizeof(qubit)`. |
| `qrp`     | `qubit * ` | Out    | The qubit register which should be prepared in a classical state. |

### Set qubit register using cstate

```C
void set_qureg_cstate(cstate const * const CRP, const size_t N, qubit * qrp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `CRP`     | `cstate const * const` | In   | An array of classical states in which the quantum register should be prepared. |
| `N`       | `const size_t` | In     | The number of qubits in `qrp` and classical states in `CRP`. Should be equal to `sizeof(qrp)/sizeof(qubit)` _and_ `sizeof(crp)/sizeof(cstate)`. |
| `qrp`     | `qubit * ` | Out    | The qubit register which should be prepared in a classical state. |


## Control

### Abort

Forces the quantum device to cease execution and return to the host.

```C
void abort(const unsigned int STATUS);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `STATUS`  | `const unsigned int` | In | Should be used to set the the `qdev_status` field of the execution handle. |


## Measurements

### Mid-circuit measurement

The results of mid-circuit measurements can be used by the **quantum** device, but are _not_ synchronised back to the host.

### End-circuit measurement

The results of end-circuit measurements are stored in `cstate` buffers passed to the executor, and will be synchronised back to the host.

## Gates 

These functions prepare gate structs to represent specific quantum gates. The gates specified here match those in the [OpenQASM Standard Library](https://openqasm.com/language/standard_library.html#standard-library).

### Identity

```C
void id(const qubit TARGET, struct gate * gp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `TARGET` | `const qubit` | In | The target to which the identity operation should be applied. |
| `gp` | `struct gate *` | Out | Pointer to the gate struct which will be modified to represent the identity operation. |

### U Gate

### Phase Gate

### Pauli X

### Pauli Y

### Pauli Z

### Hadamard

### S Gate

### Adjoint S Gate

### T Gate

### Adjoint T Gate

### SX Gate

### X Rotation

### Y Rotation

### Z Rotation

### Controlled X

### Controlled Y

### Controlled Z

### Controlled Phase

### Controlled X Rotation

### Controlled Y Rotation

### Controlled Z Rotation

### Controlled Hadamard

### Controlled U Gate

### Swap

### Double Controlled X

### Controlled Swap
