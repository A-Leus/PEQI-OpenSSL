# QIH19
[[QKD Application Interface.pdf](QKD_Application_Interface.pdf) or [URL](https://www.etsi.org/deliver/etsi_gs/QKD/001_099/004/01.01.01_60/gs_QKD004v010101p.pdf)]

[📖 [OpenSSL QKD integration manual](OpenSSL_QKD_integration.md)]

### Client/Server assignment process
This is a mock API which can be expanded to fit all your needs.

`Server` is the first application who managed to open connection on a given port. Therefore, `client` is the application who can't open a port (due to it's already opened) and connects to the `server`.

Limitations of the mock:
- it is only suitable for local testing only
- can only run one session at a time on the same `key_handle`
- `node_a.c` and `node_b.c` are example applications

### Getting started
Download the repo and compile example:
```
git clone https://github.com/akarazeev/QIH19
cd QIH19/
mkdir bin/
gcc node_a.c -I qkd_api/qkd_api.h qkd_api/qkd_api.c -o bin/a
gcc node_b.c -I qkd_api/qkd_api.h qkd_api/qkd_api.c -o bin/b
```

Execute `./bin/a`.

Open another terminal session in the same directory and execute `./bin/b`.

### Documentation
```c
uint32_t QKD_OPEN(ip_address_t destination, qos_t qos, key_handle_t* key_handle);
```
Establishes a set of parameters that define the expected levels of key service.
- :param `destination`: address of peer application
- :param `qos`: a structure describing the characteristics of the requested key source
- :param `key_handle`: a unique handle to identify the group of synchronized bits provided by the QKD key manager to the application
- :return: status of operation

---

```c
uint32_t QKD_CONNECT_NONBLOCKING(key_handle_t* key_handle, uint32_t timeout);
uint32_t QKD_CONNECT_BLOCKING(key_handle_t* key_handle, uint32_t timeout);
```
- :param `key_handle`: a unique handle to identify the group of synchronized bits provided by the QKD key manager to the application
- :param `timeout`:
- :return: status of operation

---

```c
uint32_t QKD_GET_KEY(key_handle_t* key_handle, uint8_t* key_buffer);
```
Obtains the required amount of key material requested for this `key_handle`. Each call returns the fixed amount of requested key stored in `key_buffer`.
- :param `key_handle`: a unique handle to identify the group of synchronized bits provided by the QKD key manager to the application
- :param `key_buffer`: buffer containing the current stream of keys
- :return: status of operation

---

```c
uint32_t QKD_CLOSE(key_handle_t* key_handle);
```
Terminates the association established for this `key_handle`.
- :param `key_handle`: a unique handle to identify the group of synchronized bits provided by the QKD key manager to the application
- :return: status of operation

---
