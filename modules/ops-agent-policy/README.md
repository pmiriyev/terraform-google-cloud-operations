# Agent Policy

This module is used to install/uninstall the ops agent in Google Cloud Engine VM's using [ops agent policies](https://cloud.google.com/stackdriver/docs/solutions/agents/ops-agent/agent-policies).

## Usage

Basic usage of this module is as follows:

Sample module to install [Ops Agent](https://cloud.google.com/stackdriver/docs/solutions/ops-agent) on all Debian 12 VMs with the label "goog-ops-agent-policy=enabled".
```hcl
module "ops_agent_policy" {
  source          = "github.com/terraform-google-modules/terraform-google-cloud-operations/modules/ops-agent-policy"
  project         = "<PROJECT ID>"
  zone            = "<ZONE>"
  assignment_id   = "example-ops-agent-policy"
  agents_rule = {
    package_state = "installed"
    version = "latest"
  }
  instance_filter = {
    all = false
    inventories = [{
      os_short_name = "debian"
      os_version = "12"
    }]
    inclusion_labels = [{
      labels = {
        goog-ops-agent-policy = "enabled"
      }
    }]
  }
}
```

Functional examples are included in the [examples](./../../examples) directory with the prefix `ops_agent_policy`.

<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| agents\_rule | Whether to install or uninstall the agent, and which version to install. | `object({ package_state : string, version : string })` | <pre>{<br>  "package_state": "installed",<br>  "version": "latest"<br>}</pre> | no |
| assignment\_id | Resource name. Unique among policy assignments in the given zone | `string` | n/a | yes |
| instance\_filter | Filter to select VMs. Structure is documented below here: https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/os_config_os_policy_assignment. | <pre>object({<br>    all : optional(bool),<br>    // excludes a VM if it contains all label-value pairs for some element in the list<br>    exclusion_labels : optional(list(object({<br>      labels : map(string)<br>    })), []),<br>    // includes a VM if it contains all label-value pairs for some element in the list<br>    inclusion_labels : optional(list(object({<br>      labels : map(string)<br>    })), []),<br>    // includes a VM if its inventory data matches at least one of the following inventories<br>    inventories : optional(list(object({<br>      os_short_name : string,<br>      os_version : string<br>    })), []),<br>  })</pre> | n/a | yes |
| project | The ID of the project in which to provision resources. If not present, uses the provider ID | `string` | `null` | no |
| zone | The location to which policy assignments are applied to. | `string` | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| ops\_agent\_policy | The generated policy for installing/uninstalling the ops agent. |

<!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->

## Troubleshooting

The [GA agent policies public documentation](https://cloud.google.com/logging/docs/agent/ops-agent/agent-policies#troubleshooting) shows different errors that can appear while creating policies using the ops-agent-policy module.
