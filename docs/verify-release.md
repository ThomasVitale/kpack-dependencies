# Verifying the Tekton Pipelines Package Release

This package is published as an OCI artifact, signed with Sigstore [Cosign](https://docs.sigstore.dev/cosign/overview), and associated with a [SLSA Provenance](https://slsa.dev/provenance) attestation.

Using `cosign`, you can display the supply chain security related artifacts for the `ghcr.io/kadras-io/kpack-dependencies` images. Use the specific digest you'd like to verify.

```shell
cosign tree ghcr.io/kadras-io/kpack-dependencies
```

The result:

```shell
📦 Supply Chain Security Related artifacts for an image: ghcr.io/kadras-io/kpack-dependencies
└── 💾 Attestations for an image tag: ghcr.io/kadras-io/kpack-dependencies:sha256-e1e4147f4cd9b020dc0e785e9e516435e295ed78a0a190425840ec488b3b1f77.att
   └── 🍒 sha256:a7d9b0c02ac0049bfc71db365c44ee736b3d1a41046c971035811d0550ca4866
└── 🔐 Signatures for an image tag: ghcr.io/kadras-io/kpack-dependencies:sha256-e1e4147f4cd9b020dc0e785e9e516435e295ed78a0a190425840ec488b3b1f77.sig
   └── 🍒 sha256:bafec0a9767656f69c50c12cc7bd889dfe5391ab80a32ef1d503731e21929293
```

You can verify the signature and its claims:

```shell
cosign verify \
   --certificate-identity-regexp https://github.com/kadras-io \
   --certificate-oidc-issuer https://token.actions.githubusercontent.com \
   ghcr.io/kadras-io/kpack-dependencies | jq
```

You can also verify the SLSA Provenance attestation associated with the image.

```shell
cosign verify-attestation --type slsaprovenance \
   --certificate-identity-regexp https://github.com/slsa-framework \
   --certificate-oidc-issuer https://token.actions.githubusercontent.com \
   ghcr.io/kadras-io/kpack-dependencies | jq .payload -r | base64 --decode | jq
```
