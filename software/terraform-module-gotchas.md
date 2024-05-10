# Terraform module gotchas

**Software is all about composition.**

## Modules

Modules are the key ingredient for writing reusable, maintainable code. When planning to deploy AWS infrastructure, it's better to leverage this functionality and be aware of the common gotchas to avoid conflicts with other Terraform configurations.

## Terraform

Terraform and Infrastructure as Code are no exceptions. We should use modules to create and compose our building blocks,
just as we use functions to compose and create our business logic.

There are two gotchas we need to be aware of when using modules in Terraform:

- File paths
- Inline blocks

## Inline blocks

The configuration of some Terraform resources can be done through inline blocks or independent resources. It's a trade-off, and we have to be careful when using inline properties because we lose some flexibility to define and customize the resource properties from the client.

**Inline blocks example**

```hcl
resource "aws_security_group" "alb" {
  name = "${var.cluster_name}-alb"

  ingress {
    from_port   = local.http_port
    to_port     = local.http_port
    protocol    = local.tcp_protocol
    cidr_blocks = local.all_ips
  }

  egress {
    from_port   = local.any_port
    to_port     = local.any_port
    protocol    = local.any_protocol
    cidr_blocks = local.all_ips
  }
}
```

There is also the possibility of conflicts between the inline blocks and the module properties, which can lead to unexpected results. Finally, inline blocks have been deprecated in Terraform, perhaps for these reasons.

**Separate resource customization example**

```hcl
module "s3_bucket" {
  source = "../modules/separate-resource"
  name   = "jorgetovar"
}


resource "aws_s3_bucket_versioning" "versioning" {
  bucket = module.s3_bucket.bucket_name
  versioning_configuration {
    status = "Enabled"
  }
}
```

Finally remember that defining Terraform configuration within both the module itself and the file that uses the module can lead to several issues.


### Complexity

We should organize our modules in a way that we can easily interact with a part of the infrastructure without having to
understand the whole system.
Our job is to remove the accidental complexity and make the system easier to understand. When we use inline blocks, we
are adding complexity to the system (properties and thing that we may not need to know about).

### Abstractions

But we can also leverage abstractions and hide some details that are unnecessary, if all the buckets that we create have
versioning, maybe it makes more sense to move that resource to the module.

## File Paths

By default, Terraform uses the path relative to the current working directory, in our case, the localhost root module.
Therefore, the module should be able to read files from its folder instead of the one where we are currently executing
the commands.

We can leverage the path variables of terraform to read the files that are in the module folder.The path variables are :

- **Path.module** : The path to the module folder
- **Path.root** : The path to the root module folder
- **Path.cwd** : The path to the current working directory

```hcl
resource "local_file" "mad_libs" {
  count    = var.num_files
  filename = "build/madlibs-${count.index}.txt"
  content  = templatefile("${path.module}/${element(local.templates, count.index)}",
    {
      nouns      = random_shuffle.random_nouns[count.index].result
      adjectives = random_shuffle.random_adjectives[count.index].result
      verbs      = random_shuffle.random_verbs[count.index].result
      adverbs    = random_shuffle.random_adverbs[count.index].result
      numbers    = random_shuffle.random_numbers[count.index].result
      name       = "Jorge Tovar"
    })
}
```

## Conclusion

Terraform modules may be the best way to deploy our resources in AWS, but we must be aware of the challenges that come with this useful feature.

Modules allow us to create abstractions and reduce the complexity of our infrastructure.


- [LinkedIn](https://www.linkedin.com/in/jorgetovar-sa)
- [Twitter](https://twitter.com/jorgetovar621)
- [GitHub](https://github.com/jorgetovar)

If you enjoyed the articles, visit my blog [jorgetovar.dev](jorgetovar.dev)

