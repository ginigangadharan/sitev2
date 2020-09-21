Q. A calling module can directly access child module resource attributes.
False

Q. Can another process unlock local state files?
Local state files cannot be unlocked by another process.

Q. The parent module has to provide input variables and providers to a child module explicitly in the module block as this information can not be passed down to descendent modules implicitly through inheritance.
False

- Q. The terraform import command currently can only import one resource at a time.
True

Q. Terraform recommends using provisioners instead of configuration management tools to invoke scripts or installing software on a newly built VMs.
False

Q. The Terraform Registry is a public registry. For private use of such a browsable marketplace of modules, you must use the Private Registry available in the Terraform Cloud or Enterprise version.
True

-Q. What command you can use to get human-readable output from a state or plan file?
`terraform show`

Q. How to add provider to Terraform v0.12 configuration?
- Explicitly Add provider configuration using a provider block
- Add any resources from required provider and Terraform will automatically download the provider while running `terraform init` command

---
-Q. If you remove resources from the Terraform state, resources will be physically destroyed from real-world infrastructure.
False

Q. In Terraform 0.12  terraform init cannot automatically download third-party providers.
True

Q. Anyone can publish and share modules on the Public Terraform Registry.
True

Q. A module can access all the variables of the parent module.
False

- Q. Which of the following is the most verbose log level in Terraform?
TRACE

Q. Destroy provisioner runs only during a destroy operation. Hence destroy-time provisioner within a resource will run even if the resource is tainted manually.
False

Q. To ensure that Terraform workspace names are stored correctly and safely in all backends, the workspace name must be valid to use in a URL path segment without escaping.
True

Q. Which of the following can we use to make Terraform configuration dynamic and reusable?
Input Variables
```

Q. Can you run scripts inside a provisioner block without being attached to any "real" resource?

False

Q. When using remote state, state is only ever held in memory when used by Terraform.
True

Q.Terraform workspaces are technically equivalent to renaming your state file.
True

Q. Which command can be used to inspect the current state?
`terraform show`

Q. Hanah is writing a terraform configuration with nested modules, there are multiple places where she has to use the same conditional expression but she wants to avoid repeating the same values or expressions multiple times in the configuration,. What is a better approach to dealing with this?

Q. Which among these offerings are unique to Terraform Enterprise and are not available in Terraform Open Source as well as Terraform Cloud.
- Audit Logging
- Clustering
- Private Network Connectivity

[Ref](https://www.hashicorp.com/products/terraform/pricing/)
