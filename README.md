# Versioning and Release Strategy
## Table of Contents
**[Versioning Strategy](#versioning-strategy)**<br>
**[Release Strategy](#release-strategy)**<br>
**[Branch and Merge Strategy](#branch-and-merge-strategy)**<br>
**[Breaking Changes Guide](#breaking-changes-guide)**<br>

## Versioning Strategy
Once K8Dash releases version v1.0.0, releases with follow [Semantic Versioning](https://semver.org/spec/v2.0.0.html).  Until then, breaking changes may occur without
an uptick to the Major version.  

Versions will use the `v<semantic version>` tag pattern.

## Release Strategy
### Major Release
**Cadence**: Approximately every 6 months

**Method**:
1. Release current content in Stable branch as a Minor/Patch release
2. Replace Stable branch content with everything from Master branch
3. Release updated Stable branch as a Major release

### Minor/Patch Release
**Cadence**: Approximately once a month

**Method**: Release Stable branch as a Minor/Patch release

## Branch and Merge Strategy
### Contribution workflow
1. Contributor: Branch from Master
2. Contributor: Submit MR against Master
3. Maintainer: Perform code review and identify whether there are breaking changes
   1. If there are [breaking changes](#breaking-changes-guide) , the MR might not be merged until the [Major Release](#major-release).  Alternatively,
   if backwards compatibility can be added to the change, efforts will be made to remediate the breaking change before
   completing the merge to Master.
   2. This project typically doesn't have breaking changes, so ideally this is rare.
4. Maintainer: If the change is not a breaking change, merge to Stable.

### Strategy Reasoning
- To support agile feature additions at a monthly pace, users of K8dash will receive 6 months of Stable updates until 
having to review any breaking changes. Users can simply subscribe a particular Major version and always pull the latest 
changes for that version without worry.
- If users want to be on the cutting edge, they have the option of subscribing to the latest changes, Master.
- Helpful when needing to perform security patches for previous Major releases, fewer Major releases means less work.

## Breaking Changes Guide
### In General, removing/changing any particular interface in a non-additive way
In general if logic interfaces, user interfaces included, with anything outside of k8dash, removing that logic or changing the contract is likely
a breaking change.  Added content/features to a user interface is additive and not a breaking change, unless it results
in a major negative performance impact.

The rest of this section will go into more detailed examples of breaking changes.

#### Dropping Support for a K8s Cluster Version
Reasons this might happen
- Adding a feature that only works on newer K8s clusters and not having a feature switch that would allow for the new
feature to go dark on older cluster versions, maintaining the functionality that was present before the feature add. 
- Removing any logic that is needed to support a particular K8s cluster version

#### Dropping Support for a CRD or Version of a CRD
HPA for example:
- If HPA support is removed
- Logic is added to scrape the new HPA CRD fields provided in K8s 1.18 and isn't able to continue supporting the 
limited information from 1.17.

#### Browser Support
Browser support is an important interface because companies often leverage a single browser for all needs and sometimes 
they get locked into old versions for a variety of reasons.  The following are examples of breaking changes
- Removing support for a browser type (mozilla, chrome, etc)
- Removing support for a browser version
- Adding a feature that won't work on an older browser, aka the deprecations mentioned above

#### OpenId Connect Support
Deprecation of a previously supported version of [OpenId Connect](https://github.com/indeedeng/k8dash#oidc) 

#### Metrics-Server Support
Deprecation of a previously supported version of [metrics-server](https://github.com/indeedeng/k8dash#metrics) 

#### Noteworthy Performance Changes
Changes in the minimum resource requirements for the deployment, due to a feature change.  This can be hard to identify, 
but ideally is considered in obvious cases.

A new feature might be more CPU intensive or require caching that increases the Mem requirements.  Without this new
minimum, K8dash might crash. Consider the following questions:

- If pulling or presenting a list of N items, how will this change perform on large and active clusters
- If pulling a large dataset, even if only a few items, depending on the auto-refresh frequency and use frequency, this 
could cause congestion even on a small cluster.
- Any added caching might result in a need to increase the MEM minimum.

 