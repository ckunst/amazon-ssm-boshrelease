# Amazon SSM Bosh release

* Clone this repository and execute:

  `bosh init-release`


* The config directory is created with the following contents:
  ```
  ├ config
  │   ├ blobs.yml
  │   └ final.yml
  ├ jobs
  ├ packages
  └ src
  ```

* Copy Amazon SSM Agent to `src` directory, example:
  ```
  ├ src
  │   ├  amazon-ssm-agent.deb
  ```

* Register blobs or a new version using the following command:
  ```
  bosh add-blob src/amazon-ssm-agent.deb amazon-ssm-agent.deb
  ```
This populates the `blobs.yml` with blob filename, filename size, and SHA

* Update `packages > amazon-ssm-agent > spec`, to include the blob
  ```
  files:
    - amazon-ssm-agent.deb
  ```

* Update the file under `config > final.yml` to include the `blobstore_path` variable:

  ```
  blobstore:
    provider: local
    options:
      blobstore_path: /Users/ckunst/workspace/hsbc_aws/amazon-ssm-boshrelease/blobs

  name: amazon-ssm-boshrelease
  ```

* Create the Release
  ```
  bosh create-release --final --force --tarball=amazon-ssm-boshrelease.tgz
  ```

* Update the file under `runtime-config > amazon-ssm.yml` file and update the version number, from the output of the previous command
  ```
  version: 1
  ```

* Upload the bosh Release
  ```
  bosh upload-release amazon-ssm-boshrelease.tgz
  ```

* Update the runtime config
  ```
  bosh update-runtime-config --name=amazon-ssm runtime-config/amazon-ssm.yml
  ```

* Trigger `Apply Changes` from Ops Manager
