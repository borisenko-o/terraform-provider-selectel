---
layout: "selectel"
page_title: "Selectel: selectel_global_router_vpc_network_v1"
sidebar_current: "docs-selectel-resource-global-router-vpc-network-v1"
description: |-
  Creates and manages a global router network that connects a cloud platform private network to a global router in the Selectel Global Router service using public API v1.
---

# selectel\_global\_router\_cloud\_network\_v1

Creates and manages a global router network that connects a cloud platform private network to a global router in the Selectel Global Router service using public API v1. The resource does not create a cloud platform network, it must be created before the connection, use the [openstack_networking_network_v2](https://docs.selectel.ru/en/terraform/openstack-provider-reference/networking-neutron/resources/openstack_networking_network_v2/) resource. 

For more information about cloud platform private networks and subnets, see the [official Selectel documentation](https://docs.selectel.ru/en/cloud-servers/cloud-networks/private-networks-and-subnets/). For more information about global router, see the [official Selectel documentation](https://docs.selectel.ru/en/global-router/).

## Example Usage

```hcl
resource "selectel_global_router_vpc_network_v1" "global_router_vpc_network_1" {
  router_id     = "73bc417f-bc6b-45c1-8e06-ea9d5cce061c"
  zone_id       = "36e1d82a-ea93-43cf-93ef-0e54b73daf14"
  os_network_id = "d0cc6469-3aab-4804-92fa-9b264798313f"
  project_id    = "d33dc10b-c76b-4322-b60e-7d45f043cf42"
  name          = "my_super_vpc_net"
  tags          = ["blue", "red"]
}
```

## Argument Reference

* `name` - (Required) Name of the global router network. Does not have to match the name of the cloud platform network.
* `router_id` - (Required) Unique identifier of the global router the network will be connected to. Retrieved from the [global_router_router_v1](https://registry.terraform.io/providers/selectel/selectel/latest/docs/resources/global_router_router_v1) resource. Changing this deletes the global router network, connected subnets and static routes and recreates them with the new argument value.
* `zone_id` - (Required) Unique identifier of the zone the network will be connected to. Retreived from the [selectel_global_router_zone_v1](https://registry.terraform.io/providers/selectel/selectel/latest/docs/data-sources/global_router_zone_v1) data source. For cloud platform networks, must be a zone from the `vpc` service. Changing this deletes the global router network, connected subnets and static routes and recreates them with the new argument value.
* `os_network_id` - (Required) Unique identifier of the cloud platform network, retrieved from the [openstack_networking_network_v2](https://docs.selectel.ru/en/terraform/openstack-provider-reference/networking-neutron/data-sources/openstack_networking_network_v2/) data source. Changing this deletes the global router network, connected subnets and static routes and recreates them with the new argument value.
* `project_id` - (Required) Unique identifier of the associated project. Retrieved from the [selectel_vpc_project_v2](https://registry.terraform.io/providers/selectel/selectel/latest/docs/resources/vpc_project_v2) resource. Learn more about [Projects](https://docs.selectel.ru/en/control-panel-actions/projects/about-projects/). Changing this deletes the global router network, connected subnets and static routes and recreates them with the new argument value.
* `tags` - (Optional) List of global router network tags in string format.

## Attributes Reference

* `id` - Unique identifier of the global router network.
* `name` - Name of the global router network.
* `router_id` - Unique identifier of the global router the network is connected to.
* `zone_id` - Unique identifier of the zone the network is connected to.
* `os_network_id` - Unique identifier of the connected cloud platform network.
* `project_id` - Unique identifier of the associated project.
* `tags` - List of global router network tags.
* `vlan` - Network VLAN. Matches the `segmentation_id` attribute of the openstack network.
* `created_at` - Time when the global router network was created.
* `updated_at` - Time when the global router network was updated.
* `status` - Global router network status.
* `account_id` - Selectel account ID.
* `netops_vlan_uuid` - Option for internal usage.
* `sv_network_id` - Option for internal usage.

## Import {#import}

You can import a global router network:

```shell
export OS_DOMAIN_NAME=<account_id>
export OS_USERNAME=<username>
export OS_PASSWORD=<password>
export INFRA_PROJECT_ID=<selectel_project_id>
export INFRA_REGION=<selectel_pool>
terraform selectel_global_router_vpc_network_v1.global_router_vpc_network_1 <network_id>
```

where:

*   `<account_id>` — Selectel account ID. The account ID is in the top right corner of the [Control panel](https://my.selectel.ru/). Learn more about [Registration](https://docs.selectel.ru/en/account/registration/).

*   `<username>` — Name of the service user. To get the name, in the [Control panel](https://my.selectel.ru/iam/users_management/users?type=service), go to **Identity & Access Management** ⟶ **User management** ⟶ the **Service users** tab ⟶ copy the name of the required user. Learn more about [Service users](https://docs.selectel.ru/en/access-control/access-management/).

*   `<password>` — Password of the service user.

*   `<selectel_project_id>` — Unique identifier of the associated project. To get the ID, in the [Control panel](https://my.selectel.ru/vpc/default/networks), go to **Products** ⟶ **Cloud Servers** ⟶ **Network** ⟶ project name ⟶ copy the ID of the required project. Learn more about [Projects](/access-control/projects/about-projects/).

*   `<selectel_pool>` — Pool where the network is located, for example, `ru-3`. To get information about the pool, in the [Control panel](https://my.selectel.ru/network/localnetwork/l3/), go to **Products** ⟶ **Global Router** ⟶ the global router page. The pool is on the network card.

*   `<network_id>` — Unique identifier of the global router network, for example, `4784d52d-bc14-4329-af1b-6fa6e81994d2`. To get the network ID in the [Control panel](https://my.selectel.ru/network/localnetwork/l3/), go to **Products** ⟶ **Global Router** ⟶ the global router page. The network ID is on the network card.