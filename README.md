# terraform-aws-s3-bucket


module "s3" {
  source = "git::https://../..//terraform-aws-s3.git" # needs to be corrected

  # <optional>
  # Use the app level setting to make global cleanup easier later
  force_destroy = data.terraform_remote_state.app.outputs.force_destroy

  # <optional>
  # If the KMS key exists for the application, pass it as a variable to the module
  kms_key_arn = data.terraform_remote_state.env.outputs.kms_key_arn

  # <optional>
  # You may add additional grants as in the example below
  grant_type_cu = [
    {
      id          = "abc123"
      type        = "CanonicalUser"
      permissions  = ["FULL_CONTROL"]
    },
    {
      id          = "xyz123"
      type        = "CanonicalUser"
      permissions  = ["FULL_CONTROL"]
    }
  ]
  grant_type_grp = [
    {
      uri          = "http://acs.amazonaws.com/groups/s3/LogDelivery"
      type         = "Group"
      permissions   = ["READ_ACP", "WRITE"]
    }
  ]

  # <optional>
  # If you need extra lifecycle rules beyond the two the come pre-defined
  extra_lifecycle_rules = [
    {
      id = "15-day"
      prefix = "Archive/"
      expiration_days = 15
    }
  ]

}

