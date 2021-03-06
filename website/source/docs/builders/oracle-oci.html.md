---
description: |
    The oracle-oci builder is able to create new custom images for use with Oracle
    Cloud Infrastructure (OCI).
layout: docs
page_title: 'Oracle OCI - Builders'
sidebar_current: 'docs-builders-oracle-oci'
---

# Oracle Cloud Infrastructure (OCI) Builder

Type: `oracle-oci`

The `oracle-oci` Packer builder is able to create new custom images for use
with [Oracle Cloud Infrastructure](https://cloud.oracle.com) (OCI). The builder
takes a base image, runs any provisioning necessary on the base image after
launching it, and finally snapshots it creating a reusable custom image.

It is recommended that you familiarise yourself with the [Key Concepts and
Terminology](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/concepts.htm)
prior to using this builder if you have not done so already.

The builder *does not* manage images. Once it creates an image, it is up to you
to use it or delete it.

## Authorization

The Oracle OCI API requires that requests be signed with the RSA public key
associated with your
[IAM](https://docs.us-phoenix-1.oraclecloud.com/Content/Identity/Concepts/overview.htm)
user account. For a comprehensive example of how to configure the required
authentication see the documentation on [Required Keys and
OCIDs](https://docs.us-phoenix-1.oraclecloud.com/Content/API/Concepts/apisigningkey.htm)
([Oracle Cloud
IDs](https://docs.us-phoenix-1.oraclecloud.com/Content/General/Concepts/identifiers.htm)).
Alternatively you can use [Instance
Principals](https://docs.cloud.oracle.com/en-us/iaas/Content/Identity/Tasks/callingservicesfrominstances.htm)
in which case you don't need the above user authorization.

## Configuration Reference

There are many configuration options available for the `oracle-oci` builder. In
addition to the options listed here, a
[communicator](/docs/templates/communicator.html) can be configured for this
builder.

### Required

-   `availability_domain` (string) - The name of the [Availability
    Domain](https://docs.us-phoenix-1.oraclecloud.com/Content/General/Concepts/regions.htm)
    within which a new instance is launched and provisioned. The names of the
    Availability Domains have a prefix that is specific to your
    [tenancy](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/concepts.htm#two).

    To get a list of the Availability Domains, use the
    [ListAvailabilityDomains](https://docs.us-phoenix-1.oraclecloud.com/api/#/en/identity/latest/AvailabilityDomain/ListAvailabilityDomains)
    operation, which is available in the IAM Service API.

-   `base_image_ocid` (string) - The OCID of the [base
    image](https://docs.us-phoenix-1.oraclecloud.com/Content/Compute/References/images.htm)
    to use. This is the unique identifier of the image that will be used to
    launch a new instance and provision it.

    To get a list of the accepted image OCIDs, use the
    [ListImages](https://docs.us-phoenix-1.oraclecloud.com/api/#/en/iaas/latest/Image/ListImages)
    operation available in the Core Services API.

-   `compartment_ocid` (string) - The OCID of the
    [compartment](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Tasks/choosingcompartments.htm)

-   `shape` (string) - The template that determines the number of CPUs, amount
    of memory, and other resources allocated to a newly created instance.

    To get a list of the available shapes, use the
    [ListShapes](https://docs.us-phoenix-1.oraclecloud.com/api/#/en/iaas/20160918/Shape/ListShapes)
    operation available in the Core Services API.

-   `subnet_ocid` (string) - The name of the subnet within which a new instance
    is launched and provisioned.

    To get a list of your subnets, use the
    [ListSubnets](https://docs.us-phoenix-1.oraclecloud.com/api/#/en/iaas/latest/Subnet/ListSubnets)
    operation available in the Core Services API.

    Note: the subnet must be configured to allow access via your chosen
    [communicator](/docs/templates/communicator.html) (communicator defaults to
    [SSH tcp/22](/docs/templates/communicator.html#ssh_port)).

### Optional

-   `use_instance_principals` (boolean) - Whether to use [Instance
    Principals](https://docs.cloud.oracle.com/en-us/iaas/Content/Identity/Tasks/callingservicesfrominstances.htm)
    instead of User Principals. If this key is set to true, setting any one of the `access_cfg_file`,
    `access_cfg_file_account`, `region`, `tenancy_ocid`, `user_ocid`, `key_file`, `fingerprint`, 
    `pass_phrase` will result in configuration validation errors.
    Defaults to `false`.

-   `access_cfg_file` (string) - The path to the [OCI config
    file](https://docs.us-phoenix-1.oraclecloud.com/Content/API/Concepts/sdkconfig.htm).
    This cannot be used along with the `use_instance_principals` key.
    Defaults to `$HOME/.oci/config`.

-   `access_cfg_file_account` (string) - The specific account in the [OCI config
    file](https://docs.us-phoenix-1.oraclecloud.com/Content/API/Concepts/sdkconfig.htm) to use.
    This cannot be used along with the `use_instance_principals` key.
    Defaults to `DEFAULT`.

-   `region` (string) - An Oracle Cloud Infrastructure region. Overrides value provided by the
    [OCI config file](https://docs.us-phoenix-1.oraclecloud.com/Content/API/Concepts/sdkconfig.htm)
    if present. This cannot be used along with the `use_instance_principals` key.

-   `tenancy_ocid` (string) - The OCID of your tenancy. Overrides value provided by the [OCI config
    file](https://docs.us-phoenix-1.oraclecloud.com/Content/API/Concepts/sdkconfig.htm) if present.
    This cannot be used along with the `use_instance_principals` key.

-   `user_ocid` (string) - The OCID of the user calling the OCI API. Overrides value provided by the
    [OCI config file](https://docs.us-phoenix-1.oraclecloud.com/Content/API/Concepts/sdkconfig.htm)
    if present. This cannot be used along with the `use_instance_principals` key.

-   `key_file` (string) - Full path and filename of the OCI API signing key. Overrides value provided
    by the [OCI config file](https://docs.us-phoenix-1.oraclecloud.com/Content/API/Concepts/sdkconfig.htm)
    if present. This cannot be used along with the `use_instance_principals` key.

-   `fingerprint` (string) - Fingerprint for the OCI API signing key. Overrides value provided by the
    [OCI config file](https://docs.us-phoenix-1.oraclecloud.com/Content/API/Concepts/sdkconfig.htm) if
    present. This cannot be used along with the `use_instance_principals` key.

-   `pass_phrase` (string) - Pass phrase used to decrypt the OCI API signing key. Overrides value provided
    by the [OCI config file](https://docs.us-phoenix-1.oraclecloud.com/Content/API/Concepts/sdkconfig.htm)
    if present. This cannot be used along with the `use_instance_principals` key.

-   `image_name` (string) - The name to assign to the resulting custom image.

-   `instance_name` (string) - The name to assign to the instance used for the image creation process.
    If not set a name of the form `instanceYYYYMMDDhhmmss` will be used.

-   `use_private_ip` (boolean) - Use private ip addresses to connect to the
    instance via ssh.

-   `metadata` (map of strings) - Metadata optionally contains custom metadata
    key/value pairs provided in the configuration. While this can be used to
    set metadata\["user\_data"\] the explicit "user\_data" and
    "user\_data\_file" values will have precedence. An instance's metadata can
    be obtained from at
    <a href="http://169.254.169.254" class="uri">http://169.254.169.254</a> on
    the launched instance.

-   `user_data` (string) - User data to be used by cloud-init. See [the Oracle
    docs](https://docs.us-phoenix-1.oraclecloud.com/api/#/en/iaas/20160918/LaunchInstanceDetails)
    for more details. Generally speaking, it is easier to use the
    `user_data_file`, but you can use this option to put either the plaintext
    data or the base64 encoded data directly into your Packer config. Packer
    will not automatically wait for a user script to finish before shutting
    down the instance this must be handled in a provisioner.

-   `user_data_file` (string) - Path to a file to be used as user data by
    cloud-init. See [the Oracle
    docs](https://docs.us-phoenix-1.oraclecloud.com/api/#/en/iaas/20160918/LaunchInstanceDetails)
    for more details. Example: `"user_data_file": "./boot_config/myscript.sh"`

-   `tags` (map of strings) - Add one or more freeform tags to the resulting
    custom image. See [the Oracle
    docs](https://docs.cloud.oracle.com/iaas/Content/Identity/Concepts/taggingoverview.htm)
    for more details. Example:

``` {.yaml}
"tags":
  "tag1": "value1"
  "tag2": "value2"
```

-   `defined_tags` (map of map of strings) - Add one or more defined tags for a given namespace to the resulting
    custom image. See [the Oracle
    docs](https://docs.cloud.oracle.com/iaas/Content/Identity/Concepts/taggingoverview.htm)
    for more details. Example:

``` {.yaml}
"tags":
  "namespace": {
    "tag1": "value1",
    "tag2": "value2"
  }
```

## Basic Example

Here is a basic example. Note that account specific configuration has been
substituted with the letter `a` and OCIDS have been shortened for brevity.

```json
{
    "availability_domain": "aaaa:PHX-AD-1",
    "base_image_ocid": "ocid1.image.oc1.phx.aaaaaaaa5yu6pw3riqtuhxzov7fdngi4tsteganmao54nq3pyxu3hxcuzmoa",
    "compartment_ocid": "ocid1.compartment.oc1..aaa",
    "image_name": "ExampleImage",
    "shape": "VM.Standard1.1",
    "ssh_username": "opc",
    "subnet_ocid": "ocid1.subnet.oc1..aaa",
    "type": "oracle-oci"
}
```

## Using Instance Principals

Here is a basic example. Note that account specific configuration has been
substituted with the letter `a` and OCIDS have been shortened for brevity.

```json
{
    "use_instance_principals": "true",
    "availability_domain": "aaaa:PHX-AD-1",
    "base_image_ocid": "ocid1.image.oc1.phx.aaaaaaaa5yu6pw3riqtuhxzov7fdngi4tsteganmao54nq3pyxu3hxcuzmoa",
    "compartment_ocid": "ocid1.compartment.oc1..aaa",
    "image_name": "ExampleImage",
    "shape": "VM.Standard2.1",
    "ssh_username": "opc",
    "subnet_ocid": "ocid1.subnet.oc1..aaa",
    "type": "oracle-oci"
}
```

```
[opc@packerhost ~]$ packer build packer.json
oracle-oci: output will be in this color.

==> oracle-oci: Creating temporary ssh key for instance...
==> oracle-oci: Creating instance...
==> oracle-oci: Created instance (ocid1.instance.oc1.phx.aaa).
==> oracle-oci: Waiting for instance to enter 'RUNNING' state...
==> oracle-oci: Instance 'RUNNING'.
==> oracle-oci: Instance has IP: 10.10.10.10.
==> oracle-oci: Using ssh communicator to connect: 10.10.10.10
==> oracle-oci: Waiting for SSH to become available...
==> oracle-oci: Connected to SSH!
==> oracle-oci: Creating image from instance...
==> oracle-oci: Image created.
==> oracle-oci: Terminating instance (ocid1.instance.oc1.phx.aaa)...
==> oracle-oci: Terminated instance.
Build 'oracle-oci' finished.

==> Builds finished. The artifacts of successful builds are:
--> oracle-oci: An image was created: 'ExampleImage' (OCID: ocid1.image.oc1.phx.aaa) in region 'us-phoenix-1'
[opc@packerhost ~]$
```
