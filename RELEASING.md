# Releasing

This document covers the process of creating a new release in this 
repository and then updating the version in 
[`splunk-otel-java`](https://github.com/signalfx/splunk-otel-java).

This does not address any of the internal building/releasing process.

## Create GitHub release

1. Navigate to the internal AppDynamics artifactory download site (url redacted) 
and download the version of the Secure App extension for which you will be making
a release. The file will be named something like `secureapp-otel-java-extension-25.7.0-1367.zip`. Download it to your laptop.
2. Unzip the jar file from the zip file. It will have the name `oss-agent-mtagent-extension-deployment.jar`. Optionally capture the sha256 of the jar to compare with later: `sha256sum oss-agent-mtagent-extension-deployment.jar`.
3. Navigate to the [Releases](https://github.com/signalfx/csa-releases/releases)
and click the "Draft a new release" button. 
4. In the tag dropdown, enter the full version number of the CSA release. For 
example, `25.7.0-1367` and then choose "Create new tag on publish".
5. Leave `Target: main` unchanged.
6. For the `Release title` enter `secureapp-otel-java-extension-<version>` where `<version>` is the version string (from step 4 above).
7. We are not currently publishing a changelog nor release notes, so for the description simply enter 
```
This release contains the <version>> version of the Secure Application extension for OpenTelemetry Java.

Changelog:

TBD
```
8. Attach the jar file from step 2 to the release. You can do this by dragging from the Finder or by clicking the "Attach binaries by dropping them here or selecting them." button and navigating. Upload only the single jar file.
9. Leave "Set as a pre-release" unchecked and verify that "Set as the latest release" is still checked.
10. Click the "Publish Release" button.
11. This is just a confirmation/safety step: Verify that the sha256 shown in GitHub next to the jar file for the new release matches the sha256 generated in step #2 above.

## PR into splunk-otel-java

In order for the new version of the CSA jar to be bundled with the Splunk OTel
javaagent, a PR must be created. See [this section of `CONTRIBUTING.md`](https://github.com/signalfx/splunk-otel-java/blob/main/CONTRIBUTING.md#contributing-via-pull-requests) for details.

Assumptions:

1. You are a member of the `signalfx` org on GitHub.
2. You have commit signing turned on.
3. You have signed (or will sign) the [Contributor License Agreement (CSA)](https://github.com/splunk/cla-agreement/blob/main/CLA.md).
4. You have forked the `splunk-otel-java` repo in the GitHub UI.

Process:

1. Clone your fork of `splunk-otel-java`:
```
git clone git@github.com:<your.username>/splunk-otel-java.git
cd splunk-otel-java
```
2. Make a new branch: `git checkout -b update_csa_version`.
3. Edit the `agent-csa-bundle/build.gradle.kts` file and change the `csaVersion` variable value. Save.
4. Commit: `git commit -am 'Update CSA version to <version>"`.
5. Push: `git push origin update_csa_version`.
6. Navigate to https://github.com/signalfx/splunk-otel-java/ and use the UI to create a pull request. You can make it look basically [like this one](https://github.com/signalfx/splunk-otel-java/pull/2391), just with a different version.

After the PR is merged by a maintainer, subsequent builds will contain the new
CSA extension version.
