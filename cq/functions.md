# CQ functions

## Resource management

### Allocate qubit

```C
void alloc_qubit(qubit * qp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qp`      | `qubit *` | Out | On return of this function the memory location pointed to by `qp` should contain a valid and unique `qubit` handle.

### Deallocate qubit

```C
void free_qubit(qubit * qp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qp`      | `qubit *` | Out | On return of this function the memory location pointed to by `qp` should _not_ contain a valid `qubit` handle. |


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


## Gates 

## Observations