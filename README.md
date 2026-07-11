# Test Run Platform Cluster Scoped On Behalf

E2E test repo for the `kubernetes-run-platform-meta-environment` build in
buildon-github-actions.

Tests the on-behalf cluster-scope model: both children are namespace-only envs
(`clusterScopedDelegation: parent`) whose cluster-scoped resources are bundled
in a separate dir in each env image, distinct from the image-run manifests.
The RP build aggregates the delegated sets, dedups identical resources, errors
on same-kind+name conflicts with different content, and at deploy applies each
with field ownership as if the originating env did it itself - just done on
behalf - so moving an env between permission models causes no ownership
breakage. The RP must handle MIXED perm/bubble models per child; it cannot
control what each env delegates.

Children: `test-run-env-deployment-keel-namespace-only`,
`test-run-env-job-keelson-namespace-only`.

Pending notes:

- The draft `spec-kaptainpm-schema` must be faked into the build before this
  repo can build; the on-behalf dir needs a home in the deploy base image
  series.
- Behaviour assertions (separate dir contents, dedup/conflict handling) get
  added to the hooks once the reference scripts land.
