# CQ executors

These functions instruct the backend to execute a given quantum gate or circuit. What that actually means is, naturally, an implementation detail. 

## Key

| Letter Code | Meaning |
| ---- | ---- |
| `s` | _Synchronous_. This function does not return until execution is complete, and the output buffer has been filled. |
| `a` | _Asynchronous_. This function returns immediately. The output buffer should not be accessed unless it has been explicitly synchronised. |
| `m` | _Multi-shot_. This function executes the circuit multiple times on the backend, and returns all results. |
| `b` | _Backend_. The backend the circuit should execute on is specified in this function. |

## Signatures

### Synchronous one-shot execution on default backend

```C
void s_qrun(qkern kernel, qubit * qrp, const size_t NQUBITS, cstate * const crp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `qkern`  | In | A function pointer to the quantum kernel to be executed. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the circuit. Array size should match `NQUBITS`. |

### Asynchronous one-shot execution on default backend

```C
void a_qrun(qkern kernel, qubit * qrp, const size_t NQUBITS, cstate * const crp, struct exec * const ehp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `qkern`  | In | A function pointer to the quantum kernel to be executed. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the circuit. Array size should match `NQUBITS`. |
| `ehp` | `struct exec * const` | Out | A pointer to a CQ execution handle. This handle can be used to ensure the circuit has been executed on the **quantum** device, and the measurement outcome returned to the host. |

### Synchronous multi-shot execution on default backend

```C
void sm_qrun(qkern kernel, qubit * qrp, const size_t NQUBITS, cstate * const crp, const size_t NSHOTS);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `qkern`  | In | A function pointer to the quantum kernel to be executed. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the circuit. Array size should match `NQUBITS * NSHOTS`. |
| `NSHOTS` | `const size_t` | In | The total number of repetitions of the circuit to be executed. |

### Asynchronous multi-shot execution on default backend

```C
void am_qrun(qkern kernel, qubit * qrp, const size_t NQUBITS, cstate * const crp, const size_t NSHOTS, struct exec * const ehp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `qkern`  | In | A function pointer to the quantum kernel to be executed. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the circuit. Array size should match `NQUBITS * NSHOTS`. |
| `NSHOTS` | `const size_t` | In | The total number of repetitions of the circuit to be executed. |
| `ehp` | `struct exec * const` | Out | A pointer to a CQ execution handle. This handle can be used to ensure the circuit has been executed on the **quantum** device, and the measurement outcome returned to the host. |

### Synchronous one-shot execution on specified backend

```C
void sb_qrun(qkern kernel, qubit * qrp, const size_t NQUBITS, cstate * const crp, const backend_id BE);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `qkern`  | In | A function pointer to the quantum kernel to be executed. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the circuit. Array size should match `NQUBITS`. |
| `BE` | `const backend_id` | In | Identifier for a specific **quantum** backend. |

### Asynchronous one-shot execution on specified backend

```C
void ab_qrun(qkern kernel, qubit * qrp, const size_t NQUBITS, cstate * const crp, const backend_id BE, struct exec * const ehp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `qkern`  | In | A function pointer to the quantum kernel to be executed. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the circuit. Array size should match `NQUBITS`. |
| `BE` | `const backend_id` | In | Identifier for a specific **quantum** backend. |
| `ehp` | `struct exec * const` | Out | A pointer to a CQ execution handle. This handle can be used to ensure the circuit has been executed on the **quantum** device, and the measurement outcome returned to the host. |

### Synchronous multi-shot execution on specified backend

```C
void smb_qrun(qkern kernel, qubit * qrp, const size_t NQUBITS, cstate * const crp, const size_t NSHOTS, const backend_id BE);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `qkern`  | In | A function pointer to the quantum kernel to be executed. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the circuit. Array size should match `NQUBITS * NSHOTS`. |
| `NSHOTS` | `const size_t` | In | The total number of repetitions of the circuit to be executed. |
| `BE` | `const backend_id` | In | Identifier for a specific **quantum** backend. |


### Asynchronous multi-shot execution on specified backend

```C
void amb_qrun(qkern kernel, qubit * qrp, const size_t NQUBITS, cstate * const crp, const size_t NSHOTS, const backend_id BE, struct exec * const ehp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `qkern`  | In | A function pointer to the quantum kernel to be executed. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the circuit. Array size should match `NQUBITS * NSHOTS`. |
| `NSHOTS` | `const size_t` | In | The total number of repetitions of the circuit to be executed. |
| `BE` | `const backend_id` | In | Identifier for a specific **quantum** backend. |
| `ehp` | `struct exec * const` | Out | A pointer to a CQ execution handle. This handle can be used to ensure the circuit has been executed on the **quantum** device, and the measurement outcome returned to the host. |


## Synchronisation

## Immediately synchronise execution

This function immediately synchronises an execution, including any classical data which may have already been produced. For example, if on return of `sync_qrun` the number of completed shots is 3, then the first `3 * NQUBITS` entries of the `cstate` array supplied to the executor should contain valid data on the _host_.

```C
void sync_qrun(struct exec * const ehp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `ehp`     | `struct exec * const` | Out | A pointer to a CQ execution handle. On return of the function the handle struct _and_ related `cstate` arrays will be synchronised between device and host. |


## Wait until execution completes

This function returns only when the device has completed execution. The execution handle and related `cstate` arrays are guaranteed to be synchronised to the host on return of this function.

```C
void wait_qrun(struct exec * const ehp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `ehp`     | `struct exec * const` | Out | A pointer to a CQ execution handle. On return of the function the handle struct _and_ related `cstate` arrays will be synchronised between device and host. This function also guarantees that the execution on the device has completed, but it _does not_ guarantee successful completion. |