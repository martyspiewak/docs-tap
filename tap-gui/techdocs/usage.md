# TechDocs

This guide explains how to generate and publish TechDocs for catalogs. You can also leverage the [Backstage.io documentation](https://backstage.io/docs/features/techdocs/techdocs-overview) as necessary.

## Creating an AWS S3 bucket

1. Navigate to [Amazon S3](https://s3.console.aws.amazon.com/s3/home):
    - Click `Create bucket`
    - Give the bucket a name
    - Select the AWS region
    - Keep `Block all public access` checked on for now
    - Click `Create bucket`
1. Click the newly created bucket in the bucket list
    - Navigate to the `Permissions` tab
    - Click `Edit` under `Block public access`
    - Uncheck `Block all public access`
    - Check on `Block public access to buckets and objects granted through new access control lists (ACLs)`
    - Check on `Block public access to buckets and objects granted through any access control lists (ACLs)`
    - Check off `Block public access to buckets and objects granted through new public bucket or access point policies`
    - Check off `Block public and cross-account access to buckets and objects through any public bucket or access point policies`
    - Click `Save changes` and confirm the settings
1. Click `Edit` under `Bucket policy`
    - Copy this json into the policy with the bucket name replacing `<BUCKET_NAME>`
    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "PublicRead",
                "Effect": "Allow",
                "Principal": "*",
                "Action": [
                    "s3:ListBucket",
                    "s3:GetObject"
                ],
                "Resource": [
                    "arn:aws:s3:::<BUCKET_NAME>",
                    "arn:aws:s3:::<BUCKET_NAME>/*"
                ]
            }
        ]
    }
    ```
    - Click `Save changes`
1. Click `Edit` under `Block public access`
    - Check on `Block public access to buckets and objects granted through new public bucket or access point policies`
    - Click `Save changes` and confirm the settings


## Configuring AWS S3 access

The TechDocs will be published to the S3 bucket that was just created. You will need an AWS user's access key to read from the bucket when viewing TechDocs.

1. Create an [AWS IAM User Group](https://console.aws.amazon.com/iamv2/home#/groups):
    - Click `Create Group`
    - Give the group a name
    - Click `Create Group`
    - Click the new group and navigate to `Permissions`
    - Click `Add permissions` and click `Create Inline Policy`
    - Click the `JSON` tab and replace contents with this json replacing `<BUCKET_NAME>` with the bucket name
    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "ReadTechDocs",
                "Effect": "Allow",
                "Action": [
                    "s3:ListBucket",
                    "s3:GetObject"
                ],
                "Resource": [
                    "arn:aws:s3:::<BUCKET_NAME>",
                    "arn:aws:s3:::<BUCKET_NAME>/*"
                ]
            }
        ]
    }
    ```
    - Click `Review policy`
    - Give the policy a name and click `Create policy`
1. Create an [AWS IAM User](https://console.aws.amazon.com/iamv2/home#/users) to add to this group
   - Click `Add users`
   - Give the user a name
   - Check the box for `Access key - Programmatic access` and click `Next: Permissions`
   - Check the box for the IAM Group to add the user to and click `Next: Tags`
   - Click `Next: Review` then click `Create user`
   - Note the `Access key ID` (`<AWS_READONLY_ACCESS_KEY_ID>`) and the `Secret access key` (`<AWS_READONLY_SECRET_ACCESS_KEY>`) and click `Close`


## Find the catalog locations and their entities' namespace/kind/name

TechDocs are generated for catalogs that have markdown source files for TechDocs.

1. The catalogs that will appear in the Tanzu Application Platform GUI are listed in the `tap-gui-values.yaml` under `catalog.locations`
1. For a given catalog, clone the catalog's repo to the local filesystem
1. Find the `mkdocs.yml` that is at the root of the catalog. There should be a yaml file describing the catalog at the same level. It may be called `catalog-info.yaml`
    - Note the values for `namespace`, `kind`, and `metadata.name` as well as the directory path containing the yaml file
1. Note the `spec.targets` in that file
    - For each of the targets, determine the namespace/kind/name.
        - Navigate to the target's yaml file
        - The `namespace` value is the value of `namespace`
            - if it isn't specified then it will have the value `default`
        - The `kind` value is the value of `kind`
        - The `name` value is the value of `metadata.name`
        - Note the directory path containing the yaml file

## Use the TechDocs CLI to generate and publish TechDocs

We will use `npx` to run the TechDocs CLI, which requires `Node.js` and `npm`.

1. [Download and install Node.js and npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
1. Install `npx`
    ```shell
    npm install -g npx
    ```
1. Generate the TechDocs for the root of the catalog. This will create a temporary `site` directory in your current working directory that contains the generated TechDocs files.
    ```shell
    npx @techdocs/cli generate --source-dir <DIRECTORY_CONTAINING_THE_ROOT_YAML_FILE> --output-dir ./site
    ```
1. Review the contents of the `site` directory to verify the TechDocs were generated successfully.
1. Set environment variables for authenticating with AWS S3 with an account that has read/write access:
    ```shell
    export AWS_ACCESS_KEY_ID=<AWS_ACCESS_KEY_ID>
    export AWS_SECRET_ACCESS_KEY=<AWS_SECRET_ACCESS_KEY>
    export AWS_REGION=<AWS_REGION>
    ```
1. Publish the TechDocs for the root of the catalog to the AWS S3 bucket you created earlier.
    - The `<NAMESPACE/KIND/NAME>` will be the values for `namespace`, `kind`, and `metadata.name` you noted earlier. For example `default/location/yelb-catalog-info`
    ```shell
    npx @techdocs/cli publish --publisher-type awsS3 --storage-name <BUCKET_NAME> --entity <NAMESPACE/KIND/NAME> --directory ./site
    ```
1. For each of the `spec.targets` found earlier repeat the generate and publish commands. Note that the generate command will erase the contents of the `site` directory before creating new TechDocs files so the publish command must follow the generate command for each target.

## Update app-config.yaml techdocs section to point to the AWS S3 bucket

We will update the `tap-gui-values.yaml` you used at install-time to point to the AWS S3 bucket that has the published TechDocs files.

1. Replace the `techdocs` section in `tap-gui-values.yaml` with the following yaml, substituting appropriate values for the placeholders.
    ```yaml
    techdocs:
      builder: 'external'
      publisher:
        type: 'awsS3'
        awsS3:
          bucketName: <BUCKET_NAME>
          credentials:
            accessKeyId: <AWS_READONLY_ACCESS_KEY_ID>
            secretAccessKey: <AWS_READONLY_SECRET_ACCESS_KEY>
          region: <AWS_REGION>
          s3ForcePathStyle: false
    ```

1. Update your installation using the `tanzu` CLI:

```shell
$ tanzu package installed update tap-gui \
  --version <package-version> \
  -f <values-file>
```
You can check the status of this update with:
```shell
$ tanzu package installed list
```

1. Navigate to the `Docs` section of your catalog and view the TechDocs pages to verify that the content is being loaded from the S3 bucket successfully.

