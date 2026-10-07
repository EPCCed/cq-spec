# CQ host operations

Operations which are valid on the host, but not the device. Implementers are encouraged to use the return values for error handling.

## Resource management

Operations which manage resources which are shared between host and device.

### Allocate qubit

```C
int alloc_qubit(qubit ** qhp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qhp`      | `qubit **` | Out | On return of this function the memory location pointed to by `qhp` should contain a valid and unique `qubit` handle.

### Deallocate qubit

```C
int free_qubit(qubit ** qhp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qhp`      | `qubit **` | Out | On return of this function the memory location pointed to by `qhp` should _not_ contain a valid `qubit` handle. |

### Allocate qubit register

```C
int alloc_qureg(qubit ** qrp, size_t N);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qrp`     | `qubit **`| Out    | On return of this function the memory location pointed to by `qrp` should contain _N_ valid `qubit` handles in an array. |
| `N`       | `size_t` | In     | This function should allocate valid and unique handles for `N` qubits. |

### Deallocate qubit register

```C
int free_qureg(qubit ** qrp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qrp`     | `qubit **` | Out | On return of this function the memory location pointed to by `qrp` should _not_ contain any valid qubit handles, and any held resources should have been released.

### Allocate execution handle

```C
int alloc_exec(struct exec ** ehp);
```
| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `ehp`     | `struct exec **`| Out | On return of this funciton the memory location pointed to by `ehp` should contain a valid `exec` handle. |

### Deallocate execution handle

```C
int free_exec(struct exec ** ehp);
```
| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `ehp`     | `struct exec **`| Out | On return of this funciton the memory location pointed to by `ehp` should _not_ contain a valid `exec` handle. |

### Register quantum kernel 

This function provides a hook for implementers to ensure that the quantum kernel can be accessed by the **quantum** device.

```C
int register_qkern(qkern kernel);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `qkern`  | In     | Should point to a valid quantum kernel. Quantum kernels should _only_ contain quantum operations. |

### Register parameterised quantum kernel

This function provides a hook for implementers to ensure that the parameterised quantum kernel can be accessed by the **quantum** device.

```C
int register_pqkern(pqkern pkernel);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `pkernel`  | `pqkern`  | In     | Should point to a valid parameterised quantum kernel. Quantum kernels should _only_ contain quantum operations. The paramaterised quantum kernel should accept a `void *` as an argument. |

## Execution handle getters

Given that the execution handle is defined as an opaque type, we provide a minimal interface for accessing data crucial to CQ.

### Get the identifier of execution handle
```C
size_t exec_id(struct exec * const eh);
```
| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `eh`  | `struct exec * const`  | In     | Pointer to execution handle from which to fetch data. |

### Check if execution handle is initialised
```C
bool exec_is_init(struct exec * const eh);
```
| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `eh`  | `struct exec * const`  | In     | Pointer to execution handle from which to fetch data. |

### Check if execution was completed.
```C
bool exec_completed(struct exec * const eh);
```
| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `eh`  | `struct exec * const`  | In     | Pointer to execution handle from which to fetch data. |

### Check if the execution was halted. 
```C
bool exec_halted(struct exec * const eh);
```
| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `eh`  | `struct exec * const`  | In     | Pointer to execution handle from which to fetch data. |

### Check the current status of the execution.
```C
int exec_status(struct exec * const eh);
```
| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `eh`  | `struct exec * const`  | In     | Pointer to execution handle from which to fetch data. |

### Check number of completed shots.
```C
size_t exec_completed_shots(struct exec * const eh);
```
| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `eh`  | `struct exec * const`  | In     | Pointer to execution handle from which to fetch data. |

### Check number of expected shots.
```C
size_t exec_expected_shots(struct exec * const eh);
```
| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `eh`  | `struct exec * const`  | In     | Pointer to execution handle from which to fetch data. |

## Executors

These functions instruct the backend to execute a given quantum kernel. What that actually means is, naturally, an implementation detail. 

### Key

| Letter Code | Meaning |
| ---- | ---- |
| `s` | _Synchronous_. This function does not return until execution is complete, and the output buffer has been filled. |
| `a` | _Asynchronous_. This function returns immediately. The output buffer should not be accessed unless it has been explicitly synchronised. |
| `m` | _Multi-shot_. This function executes the quantum kernel multiple times on the backend, and returns all results. |
| `b` | _Backend_. The backend the quantum kernel should execute on is specified in this function. |
| `p` | _Parameterised_. The quantum kernel is parameterised, so this executor accepts a `pqkern`. |

### Synchronous one-shot execution on default backend

```C
int s_qrun(qkern kernel, qubit * qrp, const size_t NQUBITS, cstate * const crp, const size_t NMEASURE);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `qkern`  | In | A function pointer to the quantum kernel to be executed. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the kernel. Size should be `NMEASURE`. |
| `NMEASURE` | `const size_t` | In | The number of host measurements which will be taken in the kernel. |

### Asynchronous one-shot execution on default backend

```C
int a_qrun(qkern kernel, qubit * qrp, const size_t NQUBITS, cstate * const crp, const size_t NMEASURE, struct exec * const ehp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `qkern`  | In | A function pointer to the quantum kernel to be executed. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the kernel. Array size should match `NMEASURE`. |
| `NMEASURE` | `const size_t` | In | The number of host measurements which will be taken in the kernel. |
| `ehp` | `struct exec * const` | Out | A pointer to a CQ execution handle. This handle can be used to ensure the kernel has been executed on the **quantum** device, and the measurement outcome returned to the host. |

### Synchronous multi-shot execution on default backend

```C
int sm_qrun(qkern kernel, qubit * qrp, const size_t NQUBITS, cstate * const crp, const size_t NMEASURE, const size_t NSHOTS);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `qkern`  | In | A function pointer to the quantum kernel to be executed. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the kernel. Array size should match `NMEASURE * NSHOTS`. |
| `NMEASURE` | `const size_t` | In | The number of host measurements which will be taken in the kernel. |
| `NSHOTS` | `const size_t` | In | The total number of repetitions of the kernel to be executed. |

### Asynchronous multi-shot execution on default backend

```C
int am_qrun(qkern kernel, qubit * qrp, const size_t NQUBITS, cstate * const crp, const size_t NMEASURE, const size_t NSHOTS, struct exec * const ehp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `qkern`  | In | A function pointer to the quantum kernel to be executed. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the kernel. Array size should match `NMEASURE * NSHOTS`. |
| `NMEASURE` | `const size_t` | In | The number of host measurements which will be taken in the kernel. |
| `NSHOTS` | `const size_t` | In | The total number of repetitions of the kernel to be executed. |
| `ehp` | `struct exec * const` | Out | A pointer to a CQ execution handle. This handle can be used to ensure the kernel has been executed on the **quantum** device, and the measurement outcome returned to the host. |

### Synchronous one-shot execution on specified backend

```C
int sb_qrun(qkern kernel, qubit * qrp, const size_t NQUBITS, cstate * const crp, const size_t NMEASURE, const backend_id BE);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `qkern`  | In | A function pointer to the quantum kernel to be executed. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the kernel. Array size should match `NMEASURE`. |
| `NMEASURE` | `const size_t` | In | The number of host measurements which will be taken in the kernel. |
| `BE` | `const backend_id` | In | Identifier for a specific **quantum** backend. |

### Asynchronous one-shot execution on specified backend

```C
int ab_qrun(qkern kernel, qubit * qrp, const size_t NQUBITS, cstate * const crp, const size_t NMEASURE, const backend_id BE, struct exec * const ehp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `qkern`  | In | A function pointer to the quantum kernel to be executed. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the kernel. Array size should match `NMEASURE`. |
| `NMEASURE` | `const size_t` | In | The number of host measurements which will be taken in the kernel. |
| `BE` | `const backend_id` | In | Identifier for a specific **quantum** backend. |
| `ehp` | `struct exec * const` | Out | A pointer to a CQ execution handle. This handle can be used to ensure the kernel has been executed on the **quantum** device, and the measurement outcome returned to the host. |

### Synchronous multi-shot execution on specified backend

```C
int smb_qrun(qkern kernel, qubit * qrp, const size_t NQUBITS, cstate * const crp, const size_t NMEASURE, const size_t NSHOTS, const backend_id BE);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `qkern`  | In | A function pointer to the quantum kernel to be executed. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the kernel. Array size should match `NMEASURE * NSHOTS`. |
| `NMEASURE` | `const size_t` | In | The number of host measurements which will be taken in the kernel. |
| `NSHOTS` | `const size_t` | In | The total number of repetitions of the kernel to be executed. |
| `BE` | `const backend_id` | In | Identifier for a specific **quantum** backend. |


### Asynchronous multi-shot execution on specified backend

```C
int amb_qrun(qkern kernel, qubit * qrp, const size_t NQUBITS, cstate * const crp, const size_t NMEASURE, const size_t NSHOTS, const backend_id BE, struct exec * const ehp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `qkern`  | In | A function pointer to the quantum kernel to be executed. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the kernel. Array size should match `NMEASURE * NSHOTS`. |
| `NMEASURE` | `const size_t` | In | The number of host measurements which will be taken in the kernel. |
| `NSHOTS` | `const size_t` | In | The total number of repetitions of the kernel to be executed. |
| `BE` | `const backend_id` | In | Identifier for a specific **quantum** backend. |
| `ehp` | `struct exec * const` | Out | A pointer to a CQ execution handle. This handle can be used to ensure the kernel has been executed on the **quantum** device, and the measurement outcome returned to the host. |

### Synchronous one-shot parameterised execution on default backend

```C
int sp_qrun(pqkern kernel, void * kernpar, const size_t KERNPAR_SIZE, qubit * qrp, const size_t NQUBITS, cstate * const crp, const size_t NMEASURE);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `pqkern` | In | A function pointer to the parameterised quantum kernel to be executed. |
| `kernpar` | `void *` | In | A typeless pointer to the arbitrary parameters to be passed to the parameterised kernel. |
| `KERNPAR_SIZE` | `const size_t` | In | The size of `kernpar` in bytes. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the kernel. Array size should match `NMEASURE`. |
| `NMEASURE` | `const size_t` | In | The number of host measurements which will be taken in the kernel. |

### Asynchronous one-shot parameterised execution on default backend

```C
int ap_qrun(pqkern kernel, void * kernpar, const size_t KERNPAR_SIZE, qubit * qrp, const size_t NQUBITS, cstate * const crp, const size_t NMEASURE, struct exec * const ehp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `pqkern`  | In | A function pointer to the parameterised quantum kernel to be executed. |
| `kernpar` | `void *` | In | A typeless pointer to the arbitrary parameters to be passed to the parameterised kernel. |
| `KERNPAR_SIZE` | `const size_t` | In | The size of `kernpar` in bytes. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the kernel. Array size should match `NMEASURE`. |
| `NMEASURE` | `const size_t` | In | The number of host measurements which will be taken in the kernel. |
| `ehp` | `struct exec * const` | Out | A pointer to a CQ execution handle. This handle can be used to ensure the kernel has been executed on the **quantum** device, and the measurement outcome returned to the host. |

### Synchronous multi-shot parameterised execution on default backend

```C
int smp_qrun(pqkern kernel, void * kernpar, const size_t KERNPAR_SIZE, qubit * qrp, const size_t NQUBITS, cstate * const crp, const size_t NMEASURE, const size_t NSHOTS);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `pqkern`  | In | A function pointer to the parameterised quantum kernel to be executed. |
| `kernpar` | `void *` | In | A typeless pointer to the arbitrary parameters to be passed to the parameterised kernel. |
| `KERNPAR_SIZE` | `const size_t` | In | The size of `kernpar` in bytes. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the kernel. Array size should match `NMEASURE * NSHOTS`. |
| `NMEASURE` | `const size_t` | In | The number of host measurements which will be taken in the kernel. |
| `NSHOTS` | `const size_t` | In | The total number of repetitions of the kernel to be executed. |

### Asynchronous multi-shot parameterised execution on default backend

```C
int amp_qrun(pqkern kernel, void * kernpar, const size_t KERNPAR_SIZE, qubit * qrp, const size_t NQUBITS, cstate * const crp, const size_t NMEASURE, const size_t NSHOTS, struct exec * const ehp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `pqkern`  | In | A function pointer to the parameterised quantum kernel to be executed. |
| `kernpar` | `void *` | In | A typeless pointer to the arbitrary parameters to be passed to the parameterised kernel. |
| `KERNPAR_SIZE` | `const size_t` | In | The size of `kernpar` in bytes. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the kernel. Array size should match `NMEASURE * NSHOTS`. |
| `NMEASURE` | `const size_t` | In | The number of host measurements which will be taken in the kernel. |
| `NSHOTS` | `const size_t` | In | The total number of repetitions of the kernel to be executed. |
| `ehp` | `struct exec * const` | Out | A pointer to a CQ execution handle. This handle can be used to ensure the kernel has been executed on the **quantum** device, and the measurement outcome returned to the host. |

### Synchronous one-shot parameterised execution on specified backend

```C
int sbp_qrun(pqkern kernel, void * kernpar, const size_t KERNPAR_SIZE, qubit * qrp, const size_t NQUBITS, cstate * const crp, const size_t NMEASURE, const backend_id BE);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `pqkern`  | In | A function pointer to the parameterised quantum kernel to be executed. |
| `kernpar` | `void *` | In | A typeless pointer to the arbitrary parameters to be passed to the parameterised kernel. |
| `KERNPAR_SIZE` | `const size_t` | In | The size of `kernpar` in bytes. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the kernel. Array size should match `NMEASURE`. |
| `NMEASURE` | `const size_t` | In | The number of host measurements which will be taken in the kernel. |
| `BE` | `const backend_id` | In | Identifier for a specific **quantum** backend. |

### Asynchronous one-shot parameterised execution on specified backend

```C
int abp_qrun(pqkern kernel, void * kernpar, const size_t KERNPAR_SIZE, qubit * qrp, const size_t NQUBITS, cstate * const crp, const size_t NMEASURE, const backend_id BE, struct exec * const ehp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `pqkern`  | In | A function pointer to the parameterised quantum kernel to be executed. |
| `kernpar` | `void *` | In | A typeless pointer to the arbitrary parameters to be passed to the parameterised kernel. |
| `KERNPAR_SIZE` | `const size_t` | In | The size of `kernpar` in bytes. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the kernel. Array size should match `NMEASURE`. |
| `NMEASURE` | `const size_t` | In | The number of host measurements which will be taken in the kernel. |
| `BE` | `const backend_id` | In | Identifier for a specific **quantum** backend. |
| `ehp` | `struct exec * const` | Out | A pointer to a CQ execution handle. This handle can be used to ensure the kernel has been executed on the **quantum** device, and the measurement outcome returned to the host. |

### Synchronous multi-shot parameterised execution on specified backend

```C
int smbp_qrun(pqkern kernel, void * kernpar, const size_t KERNPAR_SIZE, qubit * qrp, const size_t NQUBITS, cstate * const crp, const size_t NMEASURE, const size_t NSHOTS, const backend_id BE);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `pqkern`  | In | A function pointer to parameterised the quantum kernel to be executed. |
| `kernpar` | `void *` | In | A typeless pointer to the arbitrary parameters to be passed to the parameterised kernel. |
| `KERNPAR_SIZE` | `const size_t` | In | The size of `kernpar` in bytes. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the kernel. Array size should match `NMEASURE * NSHOTS`. |
| `NMEASURE` | `const size_t` | In | The number of host measurements which will be taken in the kernel. |
| `NSHOTS` | `const size_t` | In | The total number of repetitions of the kernel to be executed. |
| `BE` | `const backend_id` | In | Identifier for a specific **quantum** backend. |


### Asynchronous multi-shot parameterised execution on specified backend

```C
int ambp_qrun(pqkern kernel, void * kernpar, const size_t KERNPAR_SIZE, qubit * qrp, const size_t NQUBITS, cstate * const crp, const size_t NMEASURE, const size_t NSHOTS, const backend_id BE, struct exec * const ehp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `pqkern`  | In | A function pointer to the parameterised quantum kernel to be executed. |
| `kernpar` | `void *` | In | A typeless pointer to the arbitrary parameters to be passed to the parameterised kernel. |
| `KERNPAR_SIZE` | `const size_t` | In | The size of `kernpar` in bytes. |
| `qrp`     | `qubit *`| In | Pointer to the register of qubits the kernel will act upon. |
| `NQUBITS` | `const size_t` | In | The size of `qrp`. Highest qubit index is `NQUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the kernel. Array size should match `NMEASURE * NSHOTS`. |
| `NMEASURE` | `const size_t` | In | The number of host measurements which will be taken in the kernel. |
| `NSHOTS` | `const size_t` | In | The total number of repetitions of the kernel to be executed. |
| `BE` | `const backend_id` | In | Identifier for a specific **quantum** backend. |
| `ehp` | `struct exec * const` | Out | A pointer to a CQ execution handle. This handle can be used to ensure the kernel has been executed on the **quantum** device, and the measurement outcome returned to the host. |

## Synchronisation

### Immediately synchronise execution

This function immediately synchronises an execution, including any classical data which may have already been produced. For example, if on return of `sync_qrun` the number of completed shots is 3, then the first `3 * NMEASURE` entries of the `cstate` array supplied to the executor should contain valid data on the _host_.

```C
int sync_qrun(struct exec * const ehp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `ehp`     | `struct exec * const` | Out | A pointer to a CQ execution handle. On return of the function the handle struct _and_ related `cstate` arrays will be synchronised between device and host. |


### Wait until execution completes

This function returns only when the device has completed execution. The execution handle and related `cstate` arrays are guaranteed to be synchronised to the host on return of this function.

```C
int wait_qrun(struct exec * const ehp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `ehp`     | `struct exec * const` | Out | A pointer to a CQ execution handle. On return of the function the handle struct _and_ related `cstate` arrays will be synchronised between device and host. This function also guarantees that the execution on the device has completed, but it _does not_ guarantee successful completion. |

### Synchronise and end execution

```C
int halt_qrun(struct exec * const ehp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `ehp`     | `struct exec * const` | Out | A pointer to a CQ execution handle. On return of the function the handle struct _and_ related `cstate` arrays will be synchronised between device and host. This function also guarantees that the execution on the device had completed. It will attempt to actively end execution rather than waiting for completion. |
