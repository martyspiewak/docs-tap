# Release Notes

This topic contains release notes for Tanzu Application Platform beta.

**Releases**


## <a id='0-3-0'></a> v0.3.0 Beta

**Release Date**: November, XX, 2021

### New Features

The following components are new in Tanzu Application Platform v0.3.0:

Tanzu Packages:

- Supply Chain Choreographer for VMware Tanzu
  - Cartographer v0.0.7
  - Out of the Box Supply Chain Basic v0.3.0
  - Out of the Box Supply Chain with Testing v0.3.0
  - Out of the Box Supply Chain with Testing and Scanning v0.3.0
  - Out of the Box Templates v0.3.0
- Supply Chain Security Tools for VMware Tanzu
  - Scanning v
  - Image Policy Webhook v
  - Store v
- Convention Service for VMware Tanzu
  - Conventions Controller v
  - Image Source Controller v
  - Developer Conventions v
- API Portal for VMware Tanzu v
- Services Toolkit v
- Service Bindings for Kubernetes v
- Tanzu Application Platform GUI v0.3.0
  - Workload Visibility Plugin v1.0.0
  - Application Live View Plugin v0.3.0
- Tanzu Learning Center

Tanzu CLI Plugins:

- Tanzu Accelerator CLI Plugin v
- Tanzu App CLI Plugin v
- Tanzu ImagePullSecret CLI Plugin v
- Tanzu Package CLI Plugin v

The following components have been updated in Tanzu Application Platform v

- [VMware Tanzu Build Service v1.3](https://docs.pivotal.io/build-service/1-3/)
- [Cloud Native Runtimes v1.0.2](https://docs.vmware.com/en/Cloud-Native-Runtimes-for-VMware-Tanzu/1.0/tanzu-cloud-native-runtimes-1-0/GUID-cnr-overview.html)
- [Application Accelerator for VMware Tanzu v0.3.0](https://docs.vmware.com/en/Application-Accelerator-for-VMware-Tanzu/index.html)
- [Application Live View for VMware Tanzu v0.2.0](https://docs.vmware.com/en/Application-Live-View-for-VMware-Tanzu/0.2/docs/GUID-index.html)


### Known Issues

This release has the following issues:

- Issue 1

## <a id='0-2-0'></a> v0.2.0 Beta

**Release Date**: October 07, 2021

### New Features

The following components are new in Tanzu Application Platform v0.2.0:

Tanzu Packages:

- Supply Chain Choreographer for VMware Tanzu
  - Cartographer v0.0.6
  - Default Supply Chain v0.2.0
  - Default Supply Chain with Testing v0.2.0
- Supply Chain Security Tools for VMware Tanzu
  - Scanning v1.0.0-beta
  - Image Policy Webhook v1.0.0-beta.0
  - Store v1.0.0-beta.0
- Convention Service for VMware Tanzu
  - Conventions Controller v0.4.2
  - Image Source Controller v0.1.2
  - Developer Conventions v0.2.0
- API Portal for VMware Tanzu v1.0.2
- Service Control Plane Toolkit v0.3.0
- Service Bindings for Kubernetes v0.5.0

Tanzu CLI Plugins:

- Tanzu Accelerator CLI Plugin v0.3.0
- Tanzu App CLI Plugin v0.2.0
- Tanzu ImagePullSecret CLI Plugin v0.5.0
- Tanzu Package CLI Plugin v0.5.0

The following components have been updated in Tanzu Application Platform v0.2.0

- [VMware Tanzu Build Service v1.3](https://docs.pivotal.io/build-service/1-3/)
- [Cloud Native Runtimes v1.0.2](https://docs.vmware.com/en/Cloud-Native-Runtimes-for-VMware-Tanzu/1.0/tanzu-cloud-native-runtimes-1-0/GUID-cnr-overview.html)
- [Application Accelerator for VMware Tanzu v0.3.0](https://docs.vmware.com/en/Application-Accelerator-for-VMware-Tanzu/index.html)
- [Application Live View for VMware Tanzu v0.2.0](https://docs.vmware.com/en/Application-Live-View-for-VMware-Tanzu/0.2/docs/GUID-index.html)


### Known Issues

This release has the following issues:

- If your install workflow involves [Carvel imgpkg](https://github.com/vmware-tanzu/carvel-imgpkg), use version.
  v0.19.0 or later to avoid auth errors.
- If package installation fails, use `tanzu package installed update` with the `--install` flag to continue installation.
- When you use the `Tanzu Developer Tools for VSCode` extension,
  delete the workload before performing any of the following actions to avoid workload update errors:
    - Switching between the `Live Update` & `Debug` capabilities
    - Disabling `Live Update` & re-starting `Live Update`
    
    
    You can do so by performing the following steps:
    1. Click on the `Terminal` menu and select the `Run Task` option
    2. Type `tanzuWorkload delete` in the command palette that appears and hit enter
    3. View the Terminal tab to confirm that the Workload has been deleted

## <a id='0-1-0'></a> v0.1.0 Beta

**Release Date**: September 1, 2021

Initial release.

