---
subcategory: "File System (FSx)"
layout: "aws"
page_title: "AWS: aws_fsx_openzfs_volume"
description: |-
  Manages an Amazon FSx for OpenZFS volume.
---

# Resource: aws_fsx_openzfs_volume

Manages an Amazon FSx for OpenZFS volume.
See the [FSx OpenZFS User Guide](https://docs.aws.amazon.com/fsx/latest/OpenZFSGuide/what-is-fsx.html) for more information.

## Example Usage

```terraform
resource "aws_fsx_openzfs_volume" "test" {
  name             = "testvolume"
  parent_volume_id = aws_fsx_openzfs_file_system.test.root_volume_id
}
```

## Argument Reference

The following arguments are supported:

* `name` - (Required) The name of the Volume. You can use a maximum of 203 alphanumeric characters, plus the underscore (_) special character.
* `parent_volume_id` - (Required) The volume id of volume that will be the parent volume for the volume being created, this could be the root volume created from the `aws_fsx_openzfs_file_system` resource with the `root_volume_id` or the `id` property of another `aws_fsx_openzfs_volume`.
* `origin_snapshot` - (Optional) The ARN of the source snapshot to create the volume from.
* `copy_tags_to_snapshots` - (Optional) A boolean flag indicating whether tags for the file system should be copied to snapshots. The default value is false.
* `data_compression_type` - (Optional) Method used to compress the data on the volume. Valid values are `NONE` or `ZSTD`. Child volumes that don't specify compression option will inherit from parent volume. This option on file system applies to the root volume.
* `nfs_exports` - (Optional) NFS export configuration for the root volume. Exactly 1 item. See [NFS Exports](#nfs-exports) Below.
* `read_only` - (Optional) specifies whether the volume is read-only. Default is false.
* `storage_capacity_quota_gib`  - (Optional) The maximum amount of storage in gibibytes (GiB) that the volume can use from its parent.
* `storage_capacity_reservation_gib`  - (Optional) The amount of storage in gibibytes (GiB) to reserve from the parent volume.
* `user_and_group_quotas` - (Optional) - Specify how much storage users or groups can use on the volume. Maximum of 100 items. See [User and Group Quotas](#user-and-group-quotas) Below.
* `tags` - (Optional) A map of tags to assign to the file system. If configured with a provider [`default_tags` configuration block](/docs/providers/aws/index.html#default_tags-configuration-block) present, tags with matching keys will overwrite those defined at the provider-level.

### NFS Exports

* `client_configurations` - (Required) - A list of configuration objects that contain the client and options for mounting the OpenZFS file system. Maximum of 25 items. See [Client Configurations](#client configurations) Below.

### Client Configurations

* `clients` - (Required) - A value that specifies who can mount the file system. You can provide a wildcard character (*), an IP address (0.0.0.0), or a CIDR address (192.0.2.0/24. By default, Amazon FSx uses the wildcard character when specifying the client.
* `options` - (Required) -  The options to use when mounting the file system. Maximum of 20 items. See the [Linix NFS exports man page](https://linux.die.net/man/5/exports) for more information. `crossmount` and `sync` are used by default.

### User and Group Quotas

* `id` - (Required) - The ID of the user or group. Valid values between `0` and `2147483647`
* `storage_capacity_quota_gib` - (Required) - The amount of storage that the user or group can use in gibibytes (GiB). Valid values between `0` and `2147483647`
* `Type` - (Required) - A value that specifies whether the quota applies to a user or group. Valid values are `USER` or `GROUP`.

## Attributes Reference

In addition to all arguments above, the following attributes are exported:

* `arn` - Amazon Resource Name of the file system.
* `id` - Identifier of the file system, e.g., `fsvol-12345678`
* `tags_all` - A map of tags assigned to the resource, including those inherited from the provider [`default_tags` configuration block](/docs/providers/aws/index.html#default_tags-configuration-block).

## Timeouts

`aws_fsx_openzfs_file_system` provides the following [Timeouts](https://www.terraform.io/docs/configuration/blocks/resources/syntax.html#operation-timeouts)
configuration options:

* `create` - (Default `30m`) How long to wait for the file system to be created.
* `update` - (Default `30m`) How long to wait for the file system to be updated.
* `delete` - (Default `30m`) How long to wait for the file system to be deleted.

## Import

FSx Volumes can be imported using the `id`, e.g.,

```
$ terraform import aws_fsx_openzfs_volume.example fsvol-543ab12b1ca672f33
```