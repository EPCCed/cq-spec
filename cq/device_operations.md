# CQ device operations

Device operations are valid _within_ a quantum kernel.

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

### Device measurement

The results of device measurements can be used by the **quantum** device, but are _not_ synchronised back to the host.

### Qubit measurement

```C
void dmeasure_qubit(qubit * qbp, cstate * csp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qbp`     | `qubit *` | Out   | Handle for the qubit which will be measured. |
| `csp`     | `cstate *` | Out  | Handle for the `cstate` buffer which will hold the measurement result. |

### Full qubit register measurement

```C
void dmeasure_qureg(qubit * qr, const size_t NQUBITS, cstate * cr);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qr`      | `qubit *` | Out   | Handle for the quantum register which will be measured. |
| `NQUBITS` | `const size_t` | In | The number of qubits in `qr`. |
| `cr`      | `cstate *` | Out | Handle for the `cstate` buffer which will hold the measurement results. |

### Partial qubit register measurement

```C
void dmeasure(qubit * qr, const size_t NQUBITS, size_t const * const TARGETS, const size_t NTARGETS, cstate * cr);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qr`      | `qubit *` | Out   | Handle for the quantum register which holds qubits that will be measured. |
| `NQUBITS` | `const size_t` | In | The number of qubits in `qr`. |
| `TARGETS` | `size_t const * const` | In | The indices of the qubits in `qr` which will be measured. Should be `NTARGETS` long. |
| `NTARGETS` | `const size_t` | In | The number of measurement targets. Should be the size of `cr` and `TARGETS`. |
| `cr`      | `cstate *` | Out | Handle for the `cstate` buffer which will hold the measurement results. Should be at least large enough to hold `NTARGETS` results. |

### Host measurement

The results of end-circuit measurements are stored in `cstate` buffers passed to the executor, and will be synchronised back to the host. Note that passing in the result buffer to these functions allows the programmer to determine _where_ in the buffer results are stored.

### Qubit measurement

```C
void measure_qubit(qubit * qbp, cstate * csp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qbp`     | `qubit *` | Out   | Handle for the qubit which will be measured. |
| `csp`     | `cstate *` | Out  | Handle for the `cstate` buffer which will hold the measurement result. |

### Full qubit register measurement

```C
void measure_qureg(qubit * qr, const size_t NQUBITS, cstate * cr);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qr`      | `qubit *` | Out   | Handle for the quantum register which will be measured. |
| `NQUBITS` | `const size_t` | In | The number of qubits in `qr`. |
| `cr`      | `cstate *` | Out | Handle for the `cstate` buffer which will hold the measurement results. |

### Partial qubit register measurement

```C
void measure(qubit * qr, const size_t NQUBITS, size_t const * const TARGETS, const size_t NTARGETS, cstate * cr);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qr`      | `qubit *` | Out   | Handle for the quantum register which holds qubits that will be measured. |
| `NQUBITS` | `const size_t` | In | The number of qubits in `qr`. |
| `TARGETS` | `size_t const * const` | In | The indices of the qubits in `qr` which will be measured. Should be `NTARGETS` long. |
| `NTARGETS` | `const size_t` | In | The number of measurement targets. Should be the size of `cr` and `TARGETS`. |
| `cr`      | `cstate *` | Out | Handle for the `cstate` buffer which will hold the measurement results. Should be at least large enough to hold `NTARGETS` results. |

## Gates 

These functions prepare gate structs to represent specific quantum gates. The gates specified here match those in the [OpenQASM Standard Library](https://openqasm.com/language/standard_library.html#standard-library).

### Phase Gate

```C
void phase(const size_t TARGET, const double ANGLE);
```


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
