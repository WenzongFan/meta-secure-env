### meta-secure-env
This layer provides the following common and platform-specific security
features:

#### UEFI Secure Boot
For x86 platform, UEFI secure boot is the industry standard defined in the
UEFI spec, allowing images loaded by UEFI BIOS to be verified with the trusted
key. Whenever this feature is enabled, the bootloader and kernel will be
signed automatically during the build, implying the signed binaries are
contained by the resulting RPM and rootfs image.

Refer to meta-efi-secure-boot/README.md for more details.

#### MOK Secure Boot
For x86 platform, MOK secure boot is based on the UEFI secure boot, adding
the shim loader to chainloader the second-stage bootloader. Meanwhile,
the shim will also install a protocol which permits the second-stage bootloader
to perform similar binary validation, e.g, for linux kernel.

Refer to meta-efi-secure-boot/README.md for more details.

#### User key store
By default, the signing key used by UEFI/MOK secure boot is the sample key for
the purposes of development and demonstration. It is not recommended that
this sample key be used for a production device and should be replaced by
a secret key owned by the user. 

Refer to meta-signing-key/README.md for more details about how to construct an
user key store.

#### TPM 1.x
This feature enables Trusted Platform Module 1.x support, including
kernel option changes to enable tpm drivers, and picking up TPM 1.x packages.

Refer to meta-tpm/README.md for more details.

#### TPM 2.0
This feature enables Trusted Platform Module 2.0 support, including
kernel option changes to enable tpm drivers, and picking up TPM 2.0 packages.

Trusted Platform Module (TPM 2.0) is a microcontroller that stores keys,
passwords, and digital certificates. A discrete TPM 2.0 offers the
capabilities as part of the overall platform security requirements.

Refer to meta-tpm2/README.md for more details.

#### Encrypted storage
This feature gives 2 types of granularity for storage encryption. Data volume
encryption allows the user to create encryption partition with a passphrase
typed by the end user. Root filesystem encryption enables the data encryption on
the entire rootfs except the boot partition.

Both types of storage encryption are based on device-mapper crypt target,
which provides transparent encryption of block devices using the kernel crypto
API. Additionally, the utility cryptsetup is used to conveniently setup disk
encryption based on device-mapper crypt target.

Refer to meta-encrypted-storage/README.md for more details.

#### Integrity
The Linux IMA subsystem introduces hooks within the Linux kernel to support
measuring the integrity of files that are loaded (including application code)
before it is executed or mmap()ed to memory. The measured value (hash) is then
registered in a log that can be consulted by administrators.

To support proven integrity of the files, the IMA subsystem can interact with
the TPM chip within the system to protect the registered hashes from tampering
by a rogue administrator or application. The IMA subsystem, as already
supported by the Linux kernel, supports reporting on the hashes of files and
commands ran by privileged accounts (and more if you create your own
measurement policies).

In addition, IMA appraisal can even register the measured value as an extended
attribute, and after subsequent measurement(s) validate this extended attribute
against the measured value and refuse to load the file (or execute the
application) if the hash does not match. In that case, the IMA subsystem allows
files and applications to be loaded if the hashes match (and will save the
updated hash if the file is modified) but refuse to load it if it doesn't. This
provides some protection against offline tampering of the files.

Refer to meta-integrity/README.md for more details.

#### RPM signing
This feature provides the integrity verification for the RPM5 package.

Refer to meta-rpm-signing/README.md for more details.

### Maintenance
This layer is maintained in Wind River Open Source Labs at github.
- source  
  https://github.com/WindRiver-OpenSourceLabs/meta-secure-env

### Building the meta-secure-env layer
This layer should be added to the bblayers.conf file. To enable certain
feature provided by this layer, add the feature to the local.conf file. 
