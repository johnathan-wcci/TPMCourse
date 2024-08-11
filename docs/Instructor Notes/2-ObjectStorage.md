Using tpm2-tools, you can query the TPM to find out what permanent objects are stored in it. Permanent objects are those that are always present in the TPM and are part of the TPM's defined structure. These objects include keys, platform configuration registers (PCRs), and other entities that are integral to the TPM's functionality.

Here’s a step-by-step guide on how to identify these permanent objects using tpm2-tools:

TPM2 Start-up: Ensure the TPM is started. This is typically done via the tpm2_startup tool.

```sh
tpm2_startup -c
Read Permanent Handles: Use the tpm2_getcap tool to query the TPM for its permanent handles.
```

```sh
tpm2_getcap handles-persistent
```

### Read Hierarchies: The TPM has several hierarchies, each potentially containing permanent objects. You can query these hierarchies using tpm2_getcap.
```sh
tpm2_getcap handles-transient
tpm2_getcap handles-persistent
tpm2_getcap handles-permanent
tpm2_getcap handles-pcr
tpm2_getcap handles-nv-index
```
## Permanent objects in the TPM typically include:

### Storage Root Key (SRK): A key in the storage hierarchy, responsible for protecting storage keys.
```sh
tpm2_getcap handles-persistent
```
The output will include handles such as 0x81000001, which typically corresponds to the SRK.

### Endorsement Key (EK): A unique asymmetric key pair created in the Endorsement hierarchy.
```sh
tpm2_getcap handles-permanent
```
The output will include handles such as 0x81010001, which corresponds to the EK.

### Platform Configuration Registers (PCRs): Registers used for secure boot and other platform integrity checks.
```sh
tpm2_getcap handles-pcr
```
PCRs are always present and their handles usually range from 0x00000000 to 0x00000017 depending on the TPM specification.

### Permanent NV Indices: Specific non-volatile (NV) storage indices that are permanently defined.
```sh
tpm2_getcap handles-nv-index
```

### By using the tpm2_getcap command with the handles-permanent option, you will list all the permanent handles. The output will show a set of handles which correspond to these permanent objects:
```sh
tpm2_getcap handles-permanent
```
Sample output might look like:
```plaintext
- 0x40000001
- 0x4000000b
- 0x4000000c
```
Each handle represents a permanent object:
- 0x40000001: The handle for the Platform Persistent Data (PPD).
- 0x4000000b: The handle for the Storage Hierarchy (SH).
- 0x4000000c: The handle for the Endorsement Hierarchy (EH).
These permanent objects are defined by the TPM specification and are integral to the TPM's functionality. They include keys (such as the SRK and EK) and configuration structures (such as PCRs). The exact objects and their handles can be checked against the TPM's specifications or documentation provided by the TPM manufacturer.