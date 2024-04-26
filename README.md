# Node.js sample application

This is a branch of the sample app with lots of dependencies added. They
are not necessary for the application itself, they are just mean to stress
test dependency fetcher during build time.

Dependencies were added like this:

```
for p in @redhat-developer/red-hat-developer-hub-theme @redhat-developer/vscode-redhat-telemetry @redhat-developer/rhcra-client @redhat-developer/page-objects @redhat-developer/locators @redhat-developer/vscode-extension-proposals @redhat-developer/kiota-wasm @redhat-developer/rhsm-client @redhat-developer/plugin-scaffolder-frontend-module-devfile-field @redhat-developer/kiota-gen @redhat-developer/plugin-scaffolder-odo-actions @redhat-developer/rhaccm-client @redhat-developer/alizer @redhat-developer/vscode-wizard @redhat-cloud-services/frontend-components @redhat-cloud-services/insights-common-typescript @redhat-cloud-services/frontend-components-notifications @redhat-cloud-services/rbac-client @redhat-cloud-services/host-inventory-client @redhat-cloud-services/chrome @redhat-cloud-services/frontend-components-remediations @redhat-cloud-services/vulnerabilities-client @redhat-cloud-services/integrations-client @redhat-cloud-services/entitlements-client @redhat-cloud-services/sources-client @redhat-cloud-services/notifications-client @redhat-cloud-services/remediations-client @redhat-cloud-services/config-manager-client @redhat-cloud-services/insights-client @redhat-cloud-services/topological-inventory-client @redhat-cloud-services/types @redhat-cloud-services/approval-client @redhat-cloud-services/catalog-client @redhat-cloud-services/quickstarts-client @redhat-cloud-services/policies-client @redhat-cloud-services/patch-client @redhat-cloud-services/frontend-components-utilities @redhat-cloud-services/frontend-components-translations @redhat-cloud-services/eslint-config-redhat-cloud-services @redhat-cloud-services/frontend-components-config-utilities @redhat-cloud-services/frontend-components-pdf-generator @redhat-cloud-services/frontend-components-config @redhat-cloud-services/frontend-components-advisor-components @redhat-cloud-services/frontend-components-inventory-patchman @redhat-cloud-services/frontend-components-testing @redhat-cloud-services/cost-management-client @redhat-cloud-services/tsc-transform-imports @redhat-cloud-services/frontend-components-sources @redhat-cloud-services/frontend-components-inventory @redhat-cloud-services/insights-standalone @redhat-cloud-services/frontend-components-inventory-insights @redhat-cloud-services/frontend-components-inventory-compliance @redhat-cloud-services/frontend-components-inventory-general-info @redhat-cloud-services/babel-plugin-transform-imports @redhat-cloud-services/frontend-components-inventory-vulnerabilities @redhat-cloud-services/keycloak-js @redhat-cloud-services/access-requests-frontend; do
    echo "### $( date --utc -Ins ) - $p"
    pp="$( echo "$p" | sed 's/[^a-zA-Z0-9]\+/_/g' )"
    if ! npm install "$p" &>log-install-$pp.log; then
        echo "ERROR: Local build for $p failed"
        git reset --hard; continue
    fi
    if ! podman build -f Dockerfile -t abc . &>log-build-$pp.log; then
        git reset --hard
        echo "ERROR: Container build for $p failed"
        continue
    fi
    git commit -am "Adding dependency $p"
done
```
