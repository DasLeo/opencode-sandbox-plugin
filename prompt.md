Patch this opencode-sandbox-plugin checkout to forward
network.allowMachLookup to @anthropic-ai/sandbox-runtime.

Bun is already installed. Keep the change minimal:

1. Inspect the current source and package.json scripts.
2. Add optional allowMachLookup?: string[] to the network config type
   and forward it unchanged into the runtime network configuration.
   Verify the installed runtime supports it.
3. Back up and update ~/.config/opencode-sandbox/config.json by adding
   this inside its existing network object:
     "allowMachLookup": ["com.apple.trustd*"]
   Preserve all other settings and restrictions.

From the repository directory, run the following after confirming the
scripts exist:
  bun install --frozen-lockfile
  bun run typecheck
  bun test
  bun run build

Fix issues caused by the patch, but do not silently update dependencies
or regenerate the lockfile if installation fails. Report the cause first.

Deploy the built plugin:
  mkdir -p ~/.config/opencode/plugins
  cp dist/index.js ~/.config/opencode/plugins/opencode-sandbox.js

Inspect the built file's external imports. Ensure its required runtime
dependencies resolve from ~/.config/opencode/ by merging compatible
versions into ~/.config/opencode/package.json and installing them there.
Preserve existing dependencies; do not copy repository devDependencies.

Back up OpenCode configuration before removing the official sandbox
package entry from its plugin array. Preserve all other plugins and
ensure the sandbox loads only once. Do not delete the cache.

Show the diff, actual test/build results, and installed paths.
Do not read credentials, run kubectl, disable TLS verification, or restart
OpenCode. I will restart it and test the cluster connection myself.
