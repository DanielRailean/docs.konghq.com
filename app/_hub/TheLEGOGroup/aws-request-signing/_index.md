---
# This file is for documenting an individual Kong plugin.
#
# 1. Duplicate this file in your own *publisher path* on your own branch.
# Your publisher path is relative to app/_hub/.
# The path must consist only of alphanumeric characters and hyphens (-).
#
# 2. Duplicate the versions.yml file into your new plugin directory.
# Set the Kong Gateway version that the plugin is being added to.

# 3. Add a 64x64px icon for the plugin to app/_assets/images/icons/hub.
# The name of the file must be in the following format: <publisher>_<plugin-directory-name>.png
# For example, for the rate limiting plugin the icon name is kong-inc_rate-limiting.png
# If your plugin doesn't have an icon yet, you can duplicate the default_icon.png file.
#
# 4. Fill in the template in this file.
#
# The following YAML data must be filled out as prescribed by the comments
# on individual parameters. Also see documentation at:
# https://github.com/Kong/docs.konghq.com/app/_hub for examples.
# Remove inapplicable entries and comments as needed.

name: AWS Request Signing
  # (required) The name of your extension.
  # Use capitals and spaces as needed.
publisher: The LEGO Group
  # (required) The name of the entity publishing this extension.
  # Use capitals and spaces as needed.
  # If you are an individual, you might choose to use your GitHub handle, or your name.
  # If this is being published and supported by a company, please use your company name.
  # Note that every extension by a given publisher must have the exact same value.

categories: 
  # (required) Uncomment ONE that applies.
  #- authentication
  #- security
  #- traffic-control
  - serverless
  #- analytics-monitoring
  #- transformations
  #- logging
  #- deployment
# Array format only; if your plugin applies to multiple categories,
# uncomment the most applicable category.

type: plugin 
  # (required) String, one of:
  # plugin          | extensions of the core platform
  # integration     | extensions of the Kong Admin API

desc: Sign requests with AWS SIGV4 and temp credentials for secure use of AWS Lambdas in Kong. # (required) 1-liner description; max 80 chars
description: |
  This plugin allows for secure communication with AWS Lambdas. It signs requests with AWS SIGV4 and temporary credentials obtained from sts.amazonaws.com using an OAuth token. 
  This eliminates the need for an AWS API Gateway and simplifies the use of Lambdas as upstreams in Kong. 
  
  However, in order to use this plugin, there is an AWS setup required.
  Specifically, you will need to add your token issuer to the "Identity Providers" in your AWS account, this way the plugin can request temporary credentials. 
 
  For more information on the required AWS setup, visit the [plugin repo.](https://github.com/LEGO/kong-aws-request-signing#aws-setup-required)
  
  Once this is done, you can use the plugin to communicate with your Lambda HTTPS endpoint.

support_url: https://github.com/LEGO/kong-aws-request-signing/issues
  # (Optional) A specific URL of your own for this extension.
  # Defaults to the url setting in your publisher profile.

source_code: https://github.com/LEGO/kong-aws-request-signing
  # (Optional) If your extension is open source, provide a link to your code.

license_type: Apache-2.0 (modified)
  # (Optional) For open source, use the abbreviations in parentheses at:
  # https://opensource.org/licenses/alphabetical

license_url: https://github.com/LEGO/kong-aws-request-signing/blob/main/LICENSE
  # (Optional) Link to your custom license.

# COMPATIBILITY
# Uncomment at least one of 'community_edition' (Kong Gateway open-source) or
# 'enterprise_edition' (Kong Gateway Enterprise) and set `compatible: true`.

kong_version_compatibility: # required
  community_edition: # optional
    compatible: true
  enterprise_edition: # optional
    compatible: true

#########################
# PLUGIN-ONLY SETTINGS below this line
# If your plugin is an extension of the core platform, ALL of the following
# lines must be completed.
# If NOT defined as a 'plugin' in line 32, delete all lines up to '# BEGIN MARKDOWN CONTENT'

params: # Metadata about your plugin
  name: aws-request-signing # Name of the plugin in Kong (may differ from name: above)
  service_id: true # Boolean
    # Specifies whether this plugin can be applied to a Service.
    # Affects generation of examples and config table.
  consumer_id: true # Boolean
    # Specifies whether this plugin can be applied to a Consumer.
    # Affects generation of examples and config table.
  route_id: true # Boolean
    # Specifies whether this plugin can be applied to a Route.
    # Affects generation of examples and config table.
  protocols:
    # List of protocols this plugin is compatible with, in array format.
    # Valid values: "http", "https", "tcp", "tls", "tls-passthrough", "grpc",
    # "grpcs", "udp", "ws", and "wss".
    # Example:
    - name: http
    - name: https
  dbless_compatible: yes
    # Degree of compatibility with DB-less mode. Three values allowed:
    # 'yes', 'no' or 'partially'.
  dbless_explanation:
    # Optional free-text explanation, usually containing details about the degree of
    # compatibility with DB-less.
  yaml_examples: true # Boolean
    # Enables or disables autogenerated examples in declarative YAML
    # format. Default is `true`; set to `false` to disable only declarative
    # YAML examples.
  k8s_examples: true # Boolean
    # Enables or disables autogenerated examples in Kubernetes YAML
    # format. Default is `true`; set to `false` to disable only K8s examples.
  konnect_examples: true # Boolean
    # Enables or disables autogenerated examples for the Konnect UI
    # Default is `true`; set to `false` to disable only Konnect UI examples.
  manager_examples: true # Boolean
    # Enables or disables autogenerated examples for the Kong Manager UI
    # Default is `true`; set to `false` to disable only Kong Manager UI examples.
  examples: true # Boolean
    # Enables or disables all autogenerated examples (cURL, YAML, and
    # Kubernetes). Default is `true`; set to `false` to disable ALL
    # autogenerated examples.

  config: # Configuration settings for your plugin
    - name: aws_assume_role_arn
      required: yes
      datatype: string
      encrypted: true
      value_in_examples: "arn:aws:iam::123456789012:role/example-role"
      description: The ARN of the AWS IAM Role to assume before making the request to the AWS service.
    - name: aws_assume_role_name
      required: yes
      datatype: string
      encrypted: true
      value_in_examples: "example-role"
      description: The name of the AWS IAM Role to assume before making the request to the AWS service.
    - name: aws_region
      required: yes
      datatype: string
      encrypted: false
      value_in_examples: "us-east-1"
      description: The AWS region in which the service is located.
    - name: aws_service
      required: yes
      datatype: string
      encrypted: false
      value_in_examples: "lambda"
      description: The name of the AWS service to be called.
    - name: override_target_host
      required: no
      datatype: string
      encrypted: false
      value_in_examples: "example.com"
      description: An optional hostname or IP to use instead of the one specified in the service's endpoint.
    - name: override_target_port
      required: no
      datatype: number
      encrypted: false
      value_in_examples: 443
      description: An optional port to use instead of the one specified in the service's endpoint.
    - name: override_target_protocol
      required: no
      datatype: string
      one_of: ["http", "https"]
      encrypted: false
      value_in_examples: "https"
      description: An optional protocol to use instead of the one specified in the service's endpoint.

  extra:
---
