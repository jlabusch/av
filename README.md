# NAME

`av` - aws-vault shortcuts

# SYNOPSIS

```shell
av help
av show [--only-running] [ec2|rds]
av profile NEW_AWS_PROFILE
av COMMAND
```

# DESCRIPTION

This command bundles shortcuts for aws-vault invocation.

In simplest form,

```shell
av COMMAND
```
is equivalent to
```shell
aws-vault exec $AWS_PROFILE -- COMMAND
```

`av show` contains shortcuts for common filters to wade through verbose aws CLI responses.

Multiple `av` invocations can be combined, but there can only be one COMMAND and it has to be the last thing you do. For example:

```shell
av profile staging show ec2 rds aws sts get-caller-identity
```
breaks down as
 - set profile to "staging"
 - list EC2 instances
 - list RDS clusters
 - run the `aws sts get-caller-identity` command

# ADDITIONAL EXAMPLES

Stop all of the `testing-1/2/3` EC2 instances:

```
av profile testing aws ec2 stop-instances --instance-ids \
    $(av profile testing show ec2 2>/dev/null | jq -r '.[]|select(.name | test("testing-[0-9]-"))|.id')
```
