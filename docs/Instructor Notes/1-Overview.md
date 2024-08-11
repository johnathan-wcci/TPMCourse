# Tutorial

1. Launch with `docker compose run --rm tpm`
2. Let's generate a random hex number: `tpm2_getrandom 32 --hex`
    - [About This Command](https://github.com/tpm2-software/tpm2-tools/blob/master/man/tpm2_getrandom.1.md)
## Random Numbers
3 Simple exercises. Number #3 requires some bash/csh script programming.
    1.  Generate a single random value 
        / # tpm2_getrandom 20 --hex
        7dbea6c8ccd8f801021c75d858486e7b10d41f28
    2.  What is the largest amount of random values your TPM can generate?
        / # tpm2_getrandom 1024 --hex
        ERROR: TPM getrandom is bounded by max hash size, which is: 64
        Please lower your request (preferred) and try again or use --force (advanced)
        ERROR: Unable to run tpm2_getrandom
    3.  Write a small script that simulates a TPM-Backed Dice Roll. This script should output a random value between, 1 and 6.
        #!/bin/sh
        SEED=$(tpm2_getrandom 1 --hex)
        echo $((1 + 16#$SEED % 6))

3. Let's Take a look at the objects stored in the TPM.
## Objects
    1. What are the properties of your TPM?
    `tpm2_getcap properties-fixed`
    TPM2_PT_FAMILY_INDICATOR:
    raw: 0x322E3000
    value: "2.0"
    TPM2_PT_LEVEL:
    raw: 0
    TPM2_PT_REVISION:
    raw: 0xA2
    value: 1.62
    TPM2_PT_DAY_OF_YEAR:
    raw: 0x35
    TPM2_PT_YEAR:
    raw: 0x7E4
    TPM2_PT_MANUFACTURER:
    raw: 0x49424D20
    value: "IBM "
    TPM2_PT_VENDOR_STRING_1:
    raw: 0x53572020
    value: "SW"
    TPM2_PT_VENDOR_STRING_2:
    raw: 0x2054504D
    value: "TPM"
    TPM2_PT_VENDOR_STRING_3:
    raw: 0x0
    value: ""
    TPM2_PT_VENDOR_STRING_4:
    raw: 0x0
    value: ""
    TPM2_PT_VENDOR_TPM_TYPE:
    raw: 0x1
    TPM2_PT_FIRMWARE_VERSION_1:
    raw: 0x20191023
    TPM2_PT_FIRMWARE_VERSION_2:
    raw: 0x163636
    TPM2_PT_INPUT_BUFFER:
    raw: 0x400
    TPM2_PT_HR_TRANSIENT_MIN:
    raw: 0x3
    TPM2_PT_HR_PERSISTENT_MIN:
    raw: 0x7
    TPM2_PT_HR_LOADED_MIN:
    raw: 0x3
    TPM2_PT_ACTIVE_SESSIONS_MAX:
    raw: 0x40
    TPM2_PT_PCR_COUNT:
    raw: 0x18
    TPM2_PT_PCR_SELECT_MIN:
    raw: 0x3
    TPM2_PT_CONTEXT_GAP_MAX:
    raw: 0xFFFFFFFF
    TPM2_PT_NV_COUNTERS_MAX:
    raw: 0x0
    TPM2_PT_NV_INDEX_MAX:
    raw: 0x800
    TPM2_PT_MEMORY:
    raw: 0x6
    TPM2_PT_CLOCK_UPDATE:
    raw: 0x1000
    TPM2_PT_CONTEXT_HASH:
    raw: 0xD
    TPM2_PT_CONTEXT_SYM:
    raw: 0x6
    TPM2_PT_CONTEXT_SYM_SIZE:
    raw: 0x100
    TPM2_PT_ORDERLY_COUNT:
    raw: 0xFF
    TPM2_PT_MAX_COMMAND_SIZE:
    raw: 0x1000
    TPM2_PT_MAX_RESPONSE_SIZE:
    raw: 0x1000
    TPM2_PT_MAX_DIGEST:
    raw: 0x40
    TPM2_PT_MAX_OBJECT_CONTEXT:
    raw: 0xA84
    TPM2_PT_MAX_SESSION_CONTEXT:
    raw: 0x194
    TPM2_PT_PS_FAMILY_INDICATOR:
    raw: 0x322E3000
    TPM2_PT_PS_LEVEL:
    raw: 0x0
    TPM2_PT_PS_REVISION:
    raw: 0xA2
    TPM2_PT_PS_DAY_OF_YEAR:
    raw: 0x35
    TPM2_PT_PS_YEAR:
    raw: 0x7E4
    TPM2_PT_SPLIT_MAX:
    raw: 0x80
    TPM2_PT_TOTAL_COMMANDS:
    raw: 0x73
    TPM2_PT_LIBRARY_COMMANDS:
    raw: 0x6F
    TPM2_PT_VENDOR_COMMANDS:
    raw: 0x4
    TPM2_PT_NV_BUFFER_MAX:
    raw: 0x400
    TPM2_PT_MODES:
    raw: 0x1
    value: TPMA_MODES_FIPS_140_2
    `tpm2_getcap properties-variable`
    TPM2_PT_PERMANENT:
    ownerAuthSet:              0
    endorsementAuthSet:        0
    lockoutAuthSet:            0
    reserved1:                 0
    disableClear:              0
    inLockout:                 0
    tpmGeneratedEPS:           1
    reserved2:                 0
    TPM2_PT_STARTUP_CLEAR:
    phEnable:                  1
    shEnable:                  1
    ehEnable:                  1
    phEnableNV:                1
    reserved1:                 0
    orderly:                   1
    TPM2_PT_HR_NV_INDEX: 0x0
    TPM2_PT_HR_LOADED: 0x0
    TPM2_PT_HR_LOADED_AVAIL: 0x3
    TPM2_PT_HR_ACTIVE: 0x0
    TPM2_PT_HR_ACTIVE_AVAIL: 0x40
    TPM2_PT_HR_TRANSIENT_AVAIL: 0x3
    TPM2_PT_HR_PERSISTENT: 0x0
    TPM2_PT_HR_PERSISTENT_AVAIL: 0xA
    TPM2_PT_NV_COUNTERS: 0x0
    TPM2_PT_NV_COUNTERS_AVAIL: 0x19
    TPM2_PT_ALGORITHM_SET: 0xFFFFFFFF
    TPM2_PT_LOADED_CURVES: 0x3
    TPM2_PT_LOCKOUT_COUNTER: 0x0
    TPM2_PT_MAX_AUTH_FAIL: 0x3
    TPM2_PT_LOCKOUT_INTERVAL: 0x3E8
    TPM2_PT_LOCKOUT_RECOVERY: 0x3E8
    TPM2_PT_NV_WRITE_RECOVERY: 0x0
    TPM2_PT_AUDIT_COUNTER_0: 0x0
    TPM2_PT_AUDIT_COUNTER_1: 0x0

    2. Using `tpm2_getcap` find which PCR banks your TPM supports
    `tpm2_getcap pcrs`
    selected-pcrs:
    - sha1: [ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 ]
    - sha256: [ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 ]
    - sha384: [ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 ]
    - sha512: [ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 ]

    3. What permanent objects are stored in the TPM? What are they exactly?
    `tpm2_getcap handles-permanent`
    - 0x40000001 - TPM_RH_OWNER: Represents the storage primary seed, used in the storage hierarchy.
    - 0x40000007 - TPM_RH_LOCKOUT: Represents the lockout authorization, used to control the dictionary attack lockout counter.
    - 0x40000009 - TPM_RH_ENDORSEMENT: Represents the endorsement hierarchy, typically used for identity and attestation.
    - 0x4000000A - TPM_RH_PLATFORM: Represents the platform hierarchy, used for platform-specific keys and operations.
    - 0x4000000B - TPM_RH_PLATFORM_NV: Represents the platform NV (non-volatile) storage, part of the platform hierarchy.
    - 0x4000000C - TPM_RH_NULL: Represents a handle that does not reference any hierarchy, often used as a placeholder.
    - 0x4000000D - TPM_RH_UNASSIGNED: This handle is reserved and currently unassigned.
    - 0x40000110 - TPM_RH_AUTH_00: Represents an authorization role for primary objects, used for managing access.
    - 0x4000011A - TPM_RH_AUTH_0A: Represents another authorization role, similar to TPM_RH_AUTH_00, used for different objects or contexts.
    Info: You can find the documentation in (TPM 2.0 Specifications - Structures)[https://trustedcomputinggroup.org/wp-content/uploads/TPM-2.0-1.83-Part-2-Structures.pdf] - Section 7.4

    4. What cryptographic algorithms does your TPM support?
    `tpm2_getcap algorithms`
    rsa:
    value:      0x1
    asymmetric: 1
    symmetric:  0
    hash:       0
    object:     1
    reserved:   0x0
    signing:    0
    encrypting: 0
    method:     0
    sha1:
    value:      0x4
    asymmetric: 0
    symmetric:  0
    hash:       1
    object:     0
    reserved:   0x0
    signing:    0
    encrypting: 0
    method:     0
    hmac:
    value:      0x5
    asymmetric: 0
    symmetric:  0
    hash:       1
    object:     0
    reserved:   0x0
    signing:    1
    encrypting: 0
    method:     0
    aes:
    value:      0x6
    asymmetric: 0
    symmetric:  1
    hash:       0
    object:     0
    reserved:   0x0
    signing:    0
    encrypting: 0
    method:     0
    mgf1:
    value:      0x7
    asymmetric: 0
    symmetric:  0
    hash:       1
    object:     0
    reserved:   0x0
    signing:    0
    encrypting: 0
    method:     1
    keyedhash:
    value:      0x8
    asymmetric: 0
    symmetric:  0
    hash:       1
    object:     1
    reserved:   0x0
    signing:    1
    encrypting: 1
    method:     0
    xor:
    value:      0xA
    asymmetric: 0
    symmetric:  1
    hash:       1
    object:     0
    reserved:   0x0
    signing:    0
    encrypting: 0
    method:     0
    sha256:
    value:      0xB
    asymmetric: 0
    symmetric:  0
    hash:       1
    object:     0
    reserved:   0x0
    signing:    0
    encrypting: 0
    method:     0
    sha384:
    value:      0xC
    asymmetric: 0
    symmetric:  0
    hash:       1
    object:     0
    reserved:   0x0
    signing:    0
    encrypting: 0
    method:     0
    sha512:
    value:      0xD
    asymmetric: 0
    symmetric:  0
    hash:       1
    object:     0
    reserved:   0x0
    signing:    0
    encrypting: 0
    method:     0
    rsassa:
    value:      0x14
    asymmetric: 1
    symmetric:  0
    hash:       0
    object:     0
    reserved:   0x0
    signing:    1
    encrypting: 0
    method:     0
    rsaes:
    value:      0x15
    asymmetric: 1
    symmetric:  0
    hash:       0
    object:     0
    reserved:   0x0
    signing:    0
    encrypting: 1
    method:     0
    rsapss:
    value:      0x16
    asymmetric: 1
    symmetric:  0
    hash:       0
    object:     0
    reserved:   0x0
    signing:    1
    encrypting: 0
    method:     0
    oaep:
    value:      0x17
    asymmetric: 1
    symmetric:  0
    hash:       0
    object:     0
    reserved:   0x0
    signing:    0
    encrypting: 1
    method:     0
    ecdsa:
    value:      0x18
    asymmetric: 1
    symmetric:  0
    hash:       0
    object:     0
    reserved:   0x0
    signing:    1
    encrypting: 0
    method:     0
    ecdh:
    value:      0x19
    asymmetric: 1
    symmetric:  0
    hash:       0
    object:     0
    reserved:   0x0
    signing:    0
    encrypting: 0
    method:     1
    ecdaa:
    value:      0x1A
    asymmetric: 1
    symmetric:  0
    hash:       0
    object:     0
    reserved:   0x0
    signing:    1
    encrypting: 0
    method:     0
    ecschnorr:
    value:      0x1C
    asymmetric: 1
    symmetric:  0
    hash:       0
    object:     0
    reserved:   0x0
    signing:    1
    encrypting: 0
    method:     0
    kdf1_sp800_56a:
    value:      0x20
    asymmetric: 0
    symmetric:  0
    hash:       1
    object:     0
    reserved:   0x0
    signing:    0
    encrypting: 0
    method:     1
    kdf2:
    value:      0x21
    asymmetric: 0
    symmetric:  0
    hash:       1
    object:     0
    reserved:   0x0
    signing:    0
    encrypting: 0
    method:     1
    kdf1_sp800_108:
    value:      0x22
    asymmetric: 0
    symmetric:  0
    hash:       1
    object:     0
    reserved:   0x0
    signing:    0
    encrypting: 0
    method:     1
    ecc:
    value:      0x23
    asymmetric: 1
    symmetric:  0
    hash:       0
    object:     1
    reserved:   0x0
    signing:    0
    encrypting: 0
    method:     0
    symcipher:
    value:      0x25
    asymmetric: 0
    symmetric:  0
    hash:       0
    object:     1
    reserved:   0x0
    signing:    0
    encrypting: 0
    method:     0
    camellia:
    value:      0x26
    asymmetric: 0
    symmetric:  1
    hash:       0
    object:     0
    reserved:   0x0
    signing:    0
    encrypting: 0
    method:     0
    cmac:
    value:      0x3F
    asymmetric: 0
    symmetric:  1
    hash:       0
    object:     0
    reserved:   0x0
    signing:    1
    encrypting: 0
    method:     0
    ctr:
    value:      0x40
    asymmetric: 0
    symmetric:  1
    hash:       0
    object:     0
    reserved:   0x0
    signing:    0
    encrypting: 1
    method:     0
    ofb:
    value:      0x41
    asymmetric: 0
    symmetric:  1
    hash:       0
    object:     0
    reserved:   0x0
    signing:    0
    encrypting: 1
    method:     0
    cbc:
    value:      0x42
    asymmetric: 0
    symmetric:  1
    hash:       0
    object:     0
    reserved:   0x0
    signing:    0
    encrypting: 1
    method:     0
    cfb:
    value:      0x43
    asymmetric: 0
    symmetric:  1
    hash:       0
    object:     0
    reserved:   0x0
    signing:    0
    encrypting: 1
    method:     0
    ecb:
    value:      0x44
    asymmetric: 0
    symmetric:  1
    hash:       0
    object:     0
    reserved:   0x0
    signing:    0
    encrypting: 1
    method:     0

4. Let's Generate, Store, and Recall a Key
## Keys
Step 1: Generate a Primary Key
First, generate a primary key in the storage hierarchy (owner hierarchy).
```sh
# Generate a primary key
tpm2_createprimary -C o -c primary.ctx
```
This command generates a primary key under the owner hierarchy (-C o) and saves the context of this key to primary.ctx.

Step 2: Generate an RSA Key
Next, create an RSA key under the primary key.
```sh
# Create an RSA key
tpm2_create -C primary.ctx -G rsa -u key.pub -r key.priv
```
This command creates an RSA key with the public portion saved to key.pub and the private portion saved to key.priv.

Step 3: Load the Key into the TPM
Load the newly created RSA key into the TPM.
```sh
# Load the RSA key
tpm2_load -C primary.ctx -u key.pub -r key.priv -c key.ctx
```
This command loads the key and saves the key context to key.ctx.

Step 4: Make the Key Persistent
If you want to store the key persistently so it remains available across TPM resets, assign it a persistent handle.
```sh
# Make the key persistent
tpm2_evictcontrol -C o -c key.ctx 0x81010002
```
This command assigns the handle 0x81010002 to the key, making it persistent.

Step 5: Use the Key
You can now use the key for cryptographic operations. For example, to sign data:
```sh
# Create a data file to sign
echo "Hello, TPM!" > data.txt
# Sign the data file
tpm2_sign -c key.ctx -g sha256 -o signature.bin data.txt
```
This command signs data.txt using the key and saves the signature to signature.bin.

Step 6: Recall the Key
To recall and use the persistent key in future sessions, you can reference it by its persistent handle.
```sh
# Use the persistent key handle to sign data
tpm2_sign -c 0x81010002 -g sha256 -o signature2.bin data.txt
```
This command uses the key with the handle 0x81010002 to sign data.txt and saves the signature to signature2.bin.
The hex value we assign to the signature is part of the endorsement hierarchy handles.
The endorsement hierarchy enables secure storage of objects relevant for proving the identity of a given TPM-enabled platform.

Cleaning Up
If you need to remove the persistent key from the TPM, use the following command:
```sh
# Remove the persistent key
tpm2_evictcontrol -C o -c 0x81010002
```
This command removes the key associated with the handle 0x81010002.