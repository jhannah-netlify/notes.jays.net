---
title: Terraform Lifecycles (Airflow + DBT)
description: Sometimes you need more kicking.
date: 2025-10-12
# tags: second tag
---

Terraform is neato. You change stuff locally (or wherever), and Terraform will go apply those
changes to your infrastructure. I'm working in an AWS shop, but it could be Azure cloud or
Google cloud, bare metal, wherever.

Easy. But I hit a problem. We use
[Amazon Managed Workflows for Apache Airflow](https://docs.aws.amazon.com/mwaa/latest/userguide/what-is-mwaa.html) (MWAA)
for orchestration. Airflow has a very special script called `startup.sh`
that runs to provision your Airflow Worker nodes.

(e.g. We're using [DBT](https://www.getdbt.com/product/what-is-dbt) as our ETL engine. And we want Airflow to
run our DBT transformations. And DBT needs a bunch of Python dependencies. We use `startup.sh` to install all those
Python dependencies into our Airflow Workers so they can run `dbt`.)

No sweat! Our `startup.sh` is already Terraform managed! We change it however we want, and Terraform
installs those changes out to AWS (S3) for us.

But wait... Our Airflow Workers still can't run `dbt`. What the heck?

Oh... So our problem is that simply replacing `startup.sh` out in AWS doesn't restart our Airflow
environment. So our Airflow Workers are still their old selves, not their new Python phat selves
so they can run `dbt`. Oh dear.

If, in AWS Console, we shut down our Airflow environment, and restart it, then our new `startup.sh` IS used
to build new Workers. That's good. (It takes ~an hour, which isn't great, but there's no getting around that,
that's just AWS being AWS.)

So Terraform diligently swapping out `startup.sh` isn't sufficient. We need to also
force a spin down and spin up of the Airflow environment.

How can we do that? Enter
[Terraform Lifecycles](https://developer.hashicorp.com/terraform/tutorials/state/resource-lifecycle),
which has a `replace_triggered_by` feature. First we'll explicitly capture an MD5 hash of our `startup.sh`
so we can use that elsewhere:

```diff
+++ b/terraform/main.tf
module "mwaa" {
     source      = "./modules/applications/utilities/mwaa"
     ...
+   startup_script_hash  = filemd5("${path.root}/../mwaa/mwaa_env/startup.sh")
}
```

Now we can define a `lifecycle { }` in our `aws_mwaa_environment`:

```diff
+++ b/terraform/modules/applications/utilities/mwaa/main.tf
+# This null resource will be replaced whenever startup script changes
+resource "null_resource" "startup_script_trigger" {
+    triggers = {
+        startup_script_hash = var.startup_script_hash
+    }
+}
+
resource "aws_mwaa_environment" "elt_environment" {
     name = local.mwaa_environment_name
     ...
+   lifecycle {
+       replace_triggered_by = [
+           # Force replacement when startup script content changes
+           null_resource.startup_script_trigger
+       ]
+   }
+
     tags = {
         Environment = var.environment,
         Name        = local.mwaa_environment_name,
+       startup_script_hash = var.startup_script_hash  # Track changes but force replacement via lifecycle
     }
}

+++ b/terraform/modules/applications/utilities/mwaa/variables.tf
+variable "startup_script_hash" {
+    description = "Hash of the startup script content - used to trigger MWAA redeployment when startup script changes"
+    type        = string
+}
```

And joy of joys, now when we change `startup.sh` Terraform destroys Airflow and creates it again. 🎉
Which seems extreme, but is the only way I'm aware of to get Airflow to restart.

Now that one `.sh` script in your Terraform is so special and sensitive, you might want
to add a big warning to it:

```
startup.sh:
# -----------------------------------------------------------------------------------------
# WARNING: Try not to make trivial no-op changes to this file.
# Every change to this file will cause the MWAA environment to be DESTROYED and RE-CREATED.
# Which takes ~1 hour, and locks Terraform for all users during that time.
#   Uses Terraform Lifecycles "replace_triggered_by" to force replacement of MWAA env.
# -----------------------------------------------------------------------------------------
```

All done! Enjoy!

🤔 Perhaps there's an AWS CLI command we could have had Terraform issue instead?
Either way, we're still waiting an hour for AWS to do it, so even if that's possible
it's no better? (Other than maybe Terraform wouldn't be locked? Would that be a feature
or a bug? If that AWS CLI command is async, and fails... what then?)
