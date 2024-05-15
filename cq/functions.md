# CQ functions

## Resource management

### Allocate qubit

```C
void alloc_qubit(qubit * qhp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qhp`      | `qubit *` | Out | On return of this function the memory location pointed to by `qp` should contain a valid and unique `qubit` handle.

### Deallocate qubit

```C
void free_qubit(qubit * qhp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qhp`      | `qubit *` | Out | On return of this function the memory location pointed to by `qp` should _not_ contain a valid `qubit` handle. |

### Set qubit

We assume only classical states can be prepared trivially.

```C
void set_qubit(qubit qh, cstate cs);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qh`      | `qubit`  | In     | Handle to the qubit which is to be prepared in a classical state. |
| `cs`      | `cstate` | In     | The classical state in which the qubit is to be prepared. |

### Allocate qubit register

```C
void alloc_qureg(qubit * qrp, size_t N);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qrp`     | `qubit *`| Out    | On return of this function the memory location pointed to by `qrp` should contain _N_ valid `qubit` handles in an array. |
| `N`       | `size_t` | In     | This function should allocate valid and unique handles for `N` qubits. |

### Deallocate qubit register

```C
void free_qureg(qubit * qrp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qrp`      | `qubit *` | Out | On return of this function the memory location pointed to by `qrp` should not contain any valid qubit handles, and any held resources should have been released.

### Set qubit register

```C
void set_qureg(qubit * qrp, cstate * crp, size_t N);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qrp`     | `qubit *` | In    | The qubit register which should be prepared in a classical state. |
| `crp`     | `cstate *` | In   | An array of classical states in which the quantum register should be prepared. |
| `N`       | `size_t` | In     | The number of qubits in `qrp` and classical states in `crp`. Should be equal to `sizeof(qrp)/sizeof(qubit)` _and_ `sizeof(crp)/sizeof(cstate)`. |

## Gates 

## Observations