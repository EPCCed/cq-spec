# CQ executors

These functions instruct the backend to execute a given quantum gate or circuit. What that actually means is, naturally, an implementation detail. 

### Synchronous one-shot forward execution on default backend

```C
void so_qrun()
```

### Asynchronous one-shot forward execution on default backend

```C
void ao_qrun()
```

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