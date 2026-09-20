# SIPAH Evaluator BuildType v1

TypeURI: `https://darktian1977.github.io/sipah/attestations/buildtypes/evaluator/v1`

## External parameters
Must validate against `schemas/EVALUATOR_BUILD_PARAMS_v1.schema.json`:
- source_tree_sha256
- build_recipe_sha256
- dependency_lock_sha256
- toolchain_sha256
- target_os
- target_arch

## Internal parameters
Closed object:
- `environment_image_sha256` SHA-256
- `builder_config_sha256` SHA-256

These are not user-controlled external parameters; they are recorded by the builder.

## Initiation
A build is initiated only after:
1. all external parameter digests are resolved to frozen local inputs;
2. toolchain digest is verified;
3. target OS/arch equals requested target;
4. builder id is active in SIPAH_TRUST_ROOT;
5. no undeclared input is permitted.

## Resolved dependencies
Must contain exactly the six inputs represented by this v1 reference profile:
- source tree descriptor;
- build recipe descriptor;
- dependency lock descriptor;
- toolchain descriptor;
- environment image descriptor;
- builder configuration descriptor.

Each descriptor must contain a SHA-256 digest and resolve to bytes frozen by P3. The two internal-parameter digests bind `environment.image` and `builder.config`.

## Run details
- `builder.id` exactly equals a registered SIPAH builder TypeURI.
- build metadata includes invocationId.

## Subject
The produced artifact subject digest must equal the actual output bytes.

## Verification
SIPAH verifier checks:
- buildType exact match;
- external/internal parameters closed;
- required resolvedDependencies;
- builder id registration;
- signing principal equals principal registered for builder id;
- subject digest equals actual output.


## Reference example
`example_provenance.json` is a documentary fixture. Every dependency URI resolves to bytes under `example_inputs/`, and the subject resolves to `example_output.bin`. It does not claim a production build or SLSA level.
