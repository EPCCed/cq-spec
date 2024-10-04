# CQ device operations

Device operations are valid _within_ a quantum kernel. Implementers are encouraged to use the return values for error handling.

## Resource management

### Set qubit

We assume only classical states can be prepared trivially.

```C
int set_qubit(qubit qh, cstate cs);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qh`      | `qubit`  | Out     | Handle to the qubit which is to be prepared in a classical state. |
| `cs`      | `cstate` | In     | The classical state in which the qubit is to be prepared. |

### Set qubit register using unsigned integer

```C
int set_qureg(const unsigned long long STATE_IDX, const size_t N, qubit * qrp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `STATE_IDX` | `const unsigned long long` | In | Converted to an `N`-bit binary number, which is then used to set the qubit register. |
| `N`       | `const size_t` | In | The number of qubits in `qrp`. Should be equal `sizeof(qrp)/sizeof(qubit)`. |
| `qrp`     | `qubit * ` | Out    | The qubit register which should be prepared in a classical state. |

### Set qubit register using cstate

```C
int set_qureg_cstate(cstate const * const CRP, const size_t N, qubit * qrp);
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
int abort(const unsigned int STATUS);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `STATUS`  | `const unsigned int` | In | Should be used to set the the `qdev_status` field of the execution handle. |


## Measurements

### Device measurement

The results of device measurements can be used by the **quantum** device, but are _not_ synchronised back to the host.

### Qubit measurement

```C
int dmeasure_qubit(qubit * qbp, cstate * csp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qbp`     | `qubit *` | Out   | Handle for the qubit which will be measured. |
| `csp`     | `cstate *` | Out  | Handle for the `cstate` buffer which will hold the measurement result. |

### Full qubit register measurement

```C
int dmeasure_qureg(qubit * qr, const size_t NQUBITS, cstate * cr);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qr`      | `qubit *` | Out   | Handle for the quantum register which will be measured. |
| `NQUBITS` | `const size_t` | In | The number of qubits in `qr`. |
| `cr`      | `cstate *` | Out | Handle for the `cstate` buffer which will hold the measurement results. |

### Partial qubit register measurement

```C
int dmeasure(qubit * qr, const size_t NQUBITS, size_t const * const TARGETS, const size_t NTARGETS, cstate * cr);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qr`      | `qubit *` | Out   | Handle for the quantum register which holds qubits that will be measured. |
| `NQUBITS` | `const size_t` | In | The number of qubits in `qr`. |
| `TARGETS` | `size_t const * const` | In | The indices of the qubits in `qr` which will be measured. Should be `NTARGETS` long. |
| `NTARGETS` | `const size_t` | In | The number of measurement targets. Should be the size of `cr` and `TARGETS`. |
| `cr`      | `cstate *` | Out | Handle for the `cstate` buffer which will hold the measurement results. Should be at least large enough to hold `NTARGETS` results. |

### Host measurement

The results of host measurements are stored in `cstate` buffers passed to the executor, and will be synchronised back to the host. Note that passing in the result buffer to these functions allows the programmer to determine _where_ in the buffer results are stored.

### Qubit measurement

```C
int measure_qubit(qubit * qbp, cstate * csp);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qbp`     | `qubit *` | Out   | Handle for the qubit which will be measured. |
| `csp`     | `cstate *` | Out  | Handle for the `cstate` buffer which will hold the measurement result. |

### Full qubit register measurement

```C
int measure_qureg(qubit * qr, const size_t NQUBITS, cstate * cr);
```

| Parameter | Datatype | In/Out | Notes |
| --------- | -------- | ------ | ----- |
| `qr`      | `qubit *` | Out   | Handle for the quantum register which will be measured. |
| `NQUBITS` | `const size_t` | In | The number of qubits in `qr`. |
| `cr`      | `cstate *` | Out | Handle for the `cstate` buffer which will hold the measurement results. |

### Partial qubit register measurement

```C
int measure(qubit * qr, const size_t NQUBITS, size_t const * const TARGETS, const size_t NTARGETS, cstate * cr);
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

### U Gate

```math
U(\theta, \phi, \lambda) = \frac{1}{2}
\begin{pmatrix}
  1 + \exp{(i\theta)} & -i\exp{(i\lambda)}(1+\exp{(i\theta)}) \\
  i\exp{(i\phi)}(1-\exp{(i\theta)}) & \exp{(i(\phi + \lambda))}(1+\exp{(i\theta)})
\end{pmatrix}
```

```C
int unitary(qubit * qh, const double THETA, const double PHI, const double LAMBDA);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `qh`      | `qubit *` | The qubit to which the gate is applied. |
| `THETA` | `const double` | The angle of the phase, $\theta$. |
| `PHI` | `const double` | The angle of the phase, $\phi$. |
| `LAMBDA` | `const double` | The angle of the phase, $\lambda$. |

### Global Phase Gate

```math
G(\theta) =
\begin{pmatrix}
  \exp{(i\theta)} & 0 \\
  0 & \exp{(i\theta)}
\end{pmatrix}
```

```C
int gphase(qubit * qh, const double THETA);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `qh`      | `qubit *` | The qubit to which the gate is applied. |
| `THETA` | `const double` | The angle of the phase, $\theta$. |

### Phase Gate

```math
P(\theta) = 
\begin{pmatrix}
  1 & 0 \\
  0 & \exp{(i\theta)}
\end{pmatrix}
```

```C
int phase(qubit * qh, const double THETA);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `qh`      | `qubit *` | The qubit to which the gate is applied. |
| `THETA` | `const double` | The angle of the phase, $\theta$. |

### Pauli X

```math
\sigma_{x} =
\begin{pmatrix}
  0 & 1 \\
  1 & 0
\end{pmatrix}
```

```C
int paulix(qubit * qh);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `qh`      | `qubit *` | The qubit to which the gate is applied. |

### Pauli Y

```math
\sigma_{y} =
\begin{pmatrix}
  0 & i \\
  -i & 0
\end{pmatrix}
```

```C
int pauliy(qubit * qh);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `qh`      | `qubit *` | The qubit to which the gate is applied. |

### Pauli Z

```math
\sigma_{z} =
\begin{pmatrix}
  1 & 0 \\
  0 & -1
\end{pmatrix}
```

```C
int pauliz(qubit * qh);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `qh`      | `qubit *` | The qubit to which the gate is applied. |

### Hadamard

```math
H = \frac{1}{\sqrt{2}}
\begin{pmatrix}
  1 & 1 \\
  1 & -1
\end{pmatrix}
```

```C
int hadamard(qubit * qh);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `qh`      | `qubit *` | The qubit to which the gate is applied. |

### S Gate

```math
S =
\begin{pmatrix}
  1 & 0 \\
  0 & i
\end{pmatrix}
```

```C
int sqrtz(qubit * qh);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `qh`      | `qubit *` | The qubit to which the gate is applied. |

### Adjoint S Gate

```math
S^{\dagger} =
\begin{pmatrix}
  1 & 0 \\
  0 & -i
\end{pmatrix}
```

```C
int sqrtzhc(qubit * qh);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `qh`      | `qubit *` | The qubit to which the gate is applied. |

### T Gate

```math
T =
\begin{pmatrix}
  1 & 0 \\
  0 & \frac{1}{\sqrt{2}}(1 + i)
\end{pmatrix}
```

```C
int sqrts(qubit * qh);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `qh`      | `qubit *` | The qubit to which the gate is applied. |

### Adjoint T Gate

```math
T^{\dagger} =
\begin{pmatrix}
  1 & 0 \\
  0 & \frac{1}{\sqrt{2}}(1 - i)
\end{pmatrix}
```

```C
int sqrtshc(qubit * qh);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `qh`      | `qubit *` | The qubit to which the gate is applied. |

### SX Gate

```math
SX = \frac{1}{2}
\begin{pmatrix}
  1 + i & 1 - i \\
  1 - i & 1 + i
\end{pmatrix}
```

```C
int sqrtx(qubit * qh);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `qh`      | `qubit *` | The qubit to which the gate is applied. |

### X Rotation

```math
R_{x}(\theta) =
\begin{pmatrix}
  \cos{(\theta/2)} & -i\sin{(\theta/2)} \\
  -i\sin{(\theta/2)} & \cos{(\theta/2)}
\end{pmatrix}
```

```C
int rotx(qubit * qh, const double THETA);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `qh`      | `qubit *` | The qubit to which the gate is applied. |
| `THETA` | `const double` | The angle of the rotation, $\theta$. |

### Y Rotation

```math
R_{y}(\theta) =
\begin{pmatrix}
  \cos{(\theta/2)} & -\sin{(\theta/2)} \\
  \sin{(\theta/2)} & \cos{(\theta/2)}
\end{pmatrix}
```

```C
int roty(qubit * qh, const double THETA);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `qh`      | `qubit *` | The qubit to which the gate is applied. |
| `THETA` | `const double` | The angle of the rotation, $\theta$. |


### Z Rotation

```math
R_{z}(\theta) =
\begin{pmatrix}
  \exp{(-i\theta/2)} & 0 \\
  0 & \exp{(i\theta/2)}
\end{pmatrix}
```

```C
int rotz(qubit * qh, const double THETA);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `qh`      | `qubit *` | The qubit to which the gate is applied. |
| `THETA` | `const double` | The angle of the rotation, $\theta$. |

### Controlled X

```C
int cpaulix(qubit * ctrl, qubit * target);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `ctrl`    | `qubit *` | The control qubit. |
| `target`  | `qubit *` | The qubit to which the X gate is applied. |

### Controlled Y

```C
int cpauliy(qubit * ctrl, qubit * target);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `ctrl`    | `qubit *` | The control qubit. |
| `target`  | `qubit *` | The qubit to which the Y gate is applied. |

### Controlled Z

```C
int cpauliz(qubit * ctrl, qubit * target);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `ctrl`    | `qubit *` | The control qubit. |
| `target`  | `qubit *` | The qubit to which the Z gate is applied. |

### Controlled Phase

```C
int cphase(qubit * ctrl, qubit * target, const double THETA);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `ctrl`    | `qubit *` | The control qubit. |
| `target`  | `qubit *` | The qubit to which the phase gate is applied. |
| `THETA`   | `const double` | The angle of the phase rotation, $\theta$. |

### Controlled X Rotation

```C
int crotx(qubit * ctrl, qubit *  target, const double THETA);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `ctrl`    | `qubit *` | The control qubit. |
| `target`  | `qubit *` | The qubit to which the X rotation gate is applied. |
| `THETA`   | `const double` | The angle of the rotation, $\theta$. |

### Controlled Y Rotation

```C
int croty(qubit * ctrl, qubit *  target, const double THETA);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `ctrl`    | `qubit *` | The control qubit. |
| `target`  | `qubit *` | The qubit to which the Y rotation gate is applied. |
| `THETA`   | `const double` | The angle of the rotation, $\theta$. |

### Controlled Z Rotation

```C
int crotz(qubit * ctrl, qubit *  target, const double THETA);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `ctrl`    | `qubit *` | The control qubit. |
| `target`  | `qubit *` | The qubit to which the Z rotation gate is applied. |
| `THETA`   | `const double` | The angle of the rotation, $\theta$. |

### Controlled Hadamard

```C
int chadamard(qubit * ctrl, qubit * target);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `ctrl`    | `qubit *` | The control qubit. |
| `target`  | `qubit *` | The qubit to which the Hadmard gate is applied. |

### Controlled U Gate

```C
int cunitary(qubit * ctrl, qubit * target, const double THETA, const double PHI, const double LAMBDA);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `ctrl`    | `qubit *` | The control qubit. |
| `target`      | `qubit *` | The qubit to which the U gate is applied. |
| `THETA` | `const double` | The angle of the phase, $\theta$. |
| `PHI` | `const double` | The angle of the phase, $\phi$. |
| `LAMBDA` | `const double` | The angle of the phase, $\lambda$. |

### Swap

```C
int swap(qubit * a, qubit * b);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `a`       | `qubit *` | One of the qubits to be swapped. |
| `b`       | `qubit *` | One of the qubits to be swapped. |

### Double Controlled X

```C
int ccpaulix(qubit * ctrl_a, qubit * ctrl_b, qubit * target);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `ctrl_a`  | `qubit *` | The first control qubit. |
| `ctrl_b`  | `qubit *` | The second control qubit. |
| `target`  | `qubit *` | The qubit on which the X gate will be appllied. |

### Controlled Swap

```C
int cswap(qubit * ctrl, qubit * a, qubit * b);
```

| Parameter | Datatype | Notes |
| --------- | -------- | ----- |
| `ctrl`    | `qubit *` | The control qubit. |
| `a`       | `qubit *` | One of the qubits to be swapped. |
| `b`       | `qubit *` | One of the qubits to be swapped. |
