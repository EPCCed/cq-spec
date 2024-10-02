# CQ host operations

Operations which manage resources which are shared between host and device.

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
| `qrp`     | `qubit *` | Out | On return of this function the memory location pointed to by `qrp` should not contain any valid qubit handles, and any held resources should have been released.

### Register quantum kernel 

This function provides a hook for implementers to ensure that the quantum kernel can be accessed by the **quantum** device.

```C
void register_qkern(qkern kernel);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `kernel`  | `qkern`  | In     | Should point to a valid quantum kernel. Quantum kernels should _only_ contain quantum operations. |