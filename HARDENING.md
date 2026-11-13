<!-- markdownlint-disable -->

# Hardening Report: Azure--setup-helm/v5.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Azure--setup-helm/v5.0.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed action.yml without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/action.yml b/action.yml
index 39d8cae..a1bb1c1 100644
--- a/action.yml
+++ b/action.yml
@@ -1,24 +1,25 @@
 name: 'Helm tool installer'
 description: 'Install a specific version of helm binary. Acceptable values are latest or any semantic version string like 1.15.0'
 inputs:
-   version:
-      description: 'Version of helm'
-      required: true
-      default: 'latest'
-   token:
-      description: GitHub token. Used to be required to fetch the latest version
-      required: false
-      deprecationMessage: 'GitHub token is no longer required'
-      default: '${{ github.token }}'
-   downloadBaseURL:
-      description: 'Set the download base URL'
-      required: false
-      default: 'https://get.helm.sh'
+  version:
+    description: 'Version of helm'
+    required: true
+    default: 'latest'
+  token:
+    description: GitHub token. Used to be required to fetch the latest version
+    required: false
+    deprecationMessage: 'GitHub token is no longer required'
+    default: '${{ github.token }}'
+  downloadBaseURL:
+    description: 'Set the download base URL'
+    required: false
+    default: 'https://get.helm.sh'
 outputs:
-   helm-path:
-      description: 'Path to the cached helm binary'
+  helm-path:
+    description: 'Path to the cached helm binary'
 branding:
-   color: 'blue'
+  color: 'blue'
 runs:
-   using: 'node24'
-   main: 'lib/index.js'
+  using: 'node24'
+  main: 'lib/index.js'

```

Locations:

- `action.yml`

## Iteration Notes

### Iteration 1

**Fixes applied:** prior-fix

**Notes:**

I re-applied the earlier change to action.yml, which moves the YAML to 2-space indentation. It only changes formatting, not security: the file has no scripts, no `uses:` references and no shell injection points. The action runs `node24` with `lib/index.js`.

