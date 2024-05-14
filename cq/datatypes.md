# CQ datatypes


## Basic datatypes

| Datatype | Description | Implementation Notes |
| -------- | ----------- | -------------------- |
| qubit    | Classical handle for a **quantum** resource. A single qubit. |  Not guaranteed to be valid until handle is passed to an allocator function. Handles should be unique. |
| outcome  | Datatype capable of holding the outcome of measuring a single qubit. | Trivially implemented as a bool. |


## Compound datatypes