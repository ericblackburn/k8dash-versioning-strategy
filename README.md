# Versioning and Release Strategy
# TODO
- v or no v is version tag?
- which branch should people develop on?
- Can we use tags for automation of merges?
-- maybe if there are no merge conflicts, but what if there are
- Should we tag issues, even before they are worked on, as breaking changes?

# Versioning Strategy
Once K8Dash releases version 1.0.0, it will use the following strategy.  Until then, breaking changes may occur without
an uptick to the Major version.

The goal is to provide a [Semantic Versioning](https://semver.org/spec/v2.0.0.html) for the Master branch.
A second branch, Latest, will be used to merge in anything that is known to-be a [breaking change](#breaking-changes-guide) 
or anything that is hard to verify isn't a breaking change.  The Latest branch can receive breaking changes at any time, 
but the Master branch will follow the [Major Release Cadence](#major-release).

Again, all changes are to be merged into the Latest branch. Any non-breaking changes should also be added into the Master
branch.

## Reasoning for the Strategy
- To support agile feature additions at a monthly pace, users of K8dash will receive 6 months of updates until having to 
review any breaking changes.  Also, this is helpful when needing to perform security patches for previous Major releases.
Users can subscribe a particular Major version and always pull the latest changes for that version without worry.
- Users have the option of leveraging Latest as well, if they don't mind reacting to a breaking change.

# Release Strategy
## Major Release
###  Cadence
Approximately every 6 months

### Method
1. Release current content in Master branch as a Minor/Patch release
2. Replace Master branch content with everything from the Latest branch
3. Release updated Master branch as a Major release

## Minor/Patch Release
### Cadence
Approximately once a month

### Method
Release current content in Master branch as a Minor/Patch release

# Breaking Changes Guide
## Breaking Changes
#### In General, removing/changing any particular interface
In general if logic interfaces with anything outside of k8dash, removing that logic or changing the contract is likely
a breaking change. User interfaces are included as things in the outside world.

The rest of this section will go into more detailed examples.

#### Dropping Support for a K8s Cluster Version
Reasons this might happen
- Adding a feature that only works on newer k8s clusters and not having a feature switch that would allow for the new
feature to go dark on older cluster versions and maintain the functionality that was present before the feature add. 
- Removing any logic that is needed to support a particular k8s cluster version

#### Dropping Support for a CRD or Version of a CRD
HPA for example.  If HPA support is removed or logic only supports the HPA yaml structure for K8s 1.17 because it expects
the fields added in K8s 1.18, then this would be a breaking change.

#### Browser Support
Browser support is an important interface because companies often leverage a single browser for all needs and sometimes 
they get locked into old versions for a variety of reasons.  The following are examples of breaking changes
- Removing support for a browser type (mozilla, chrome, etc)
- Removing support for a browser version
- Adding a feature that won't work old browser, aka the deprecations mentioned above

#### OpenId Connect Support
Deprecation of a previously supported version of [OpenId Connect](https://github.com/indeedeng/k8dash#oidc) 

#### Metrics-Server Support
Deprecation of a previously supported version of [metrics-server](https://github.com/indeedeng/k8dash#metrics) 

#### Noteworthy Performance Changes
Changes in the minimum resource requirements for the deployment, due to a feature change.  

A new feature might be more CPU intensive or require caching that increases the Mem requirements.  Without this new
minimum, K8dash might crash. 

This can be hard to identify, but ideally is considered in obvious cases.