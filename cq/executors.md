# CQ executors

These functions instruct the backend to execute a given quantum gate or circuit. What that actually means is, naturally, an implementation detail. 

### Synchronous one-shot forward execution on default backend

This function is _synchronous_ so it does not return until the circuit has been run, and the outcome measured and returned to host.

```C
void so_qrun(struct gate const * const CIRC, const size_t N, cstate * const crp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `CIRC` | `struct gate const * const` | In | A read-only array of gates, constituting a **quantum** circuit. |
| `N` | `const size_t` | In | The total width of `CIRCUIT`. Highest qubit index is `N_QUBITS`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the circuit. Array size should match `N_QUBITS`.

### Asynchronous one-shot forward execution on default backend

This function is _asynchronous_ so it returns immediately. The classical register which holds the measurement outcome for the circuit can only safely be accessed once the executor handle is waited on.

```C
void ao_qrun(struct gate const * const CIRC, const size_t N, cstate * const crp, handle * ehp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `CIRC` | `struct gate const * const` | In | A read-only array of gates, constituting a **quantum** circuit. |
| `N` | `const size_t` | In | The total width of `CIRCUIT`. Highest qubit index is `N`-1. |
| `crp` | `cstate * const` | Out | A constant pointer to a cstate array in which to store the outcome of the circuit. Array size should match `N`. |
| `ehp` | `struct exec *` | Out | A pointer to a CQ execution handle. This handle can be used to ensure the circuit has been executed on the **quantum** device, and the measurement outcome returned to the host. |

### Synchronous multi-shot forward execution on default backend

```C
void sm_qrun()
```

### Asynchronous multi-shot forward execution on default backend

```C
void am_qrun()
```

### Synchronous one-shot reverse execution on default backend

```C
void sor_qrun()
```

### Asynchronous one-shot reverse execution on default backend

```C
void aor_qrun()
```

### Synchronous multi-shot reverse execution on default backend

```C
void smr_qrun()
```

### Asynchronous multi-shot reverse execution on default backend

```C
void amr_qrun()
```

### Synchronous one-shot forward execution on specified backend

```C
void sob_qrun()
```

### Asynchronous one-shot forward execution on specified backend

```C
void aob_qrun()
```

### Synchronous multi-shot forward execution on specified backend

```C
void smb_qrun()
```

### Asynchronous multi-shot forward execution on specified backend

```C
void amb_qrun()
```

### Synchronous one-shot reverse execution on specified backend

```C
void sorb_qrun()
```

### Asynchronous one-shot reverse execution on specified backend

```C
void aorb_qrun()
```

### Synchronous multi-shot reverse execution on specified backend

```C
void smrb_qrun()
```

### Asynchronous multi-shot reverse execution on specified backend

```C
void amrb_qrun()
```