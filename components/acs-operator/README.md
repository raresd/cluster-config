Update Argo CD configuration to include a health check for Central:

```
platform.stackrox.io/Central:
    health.lua: |
    hs = {}
    if obj.status ~= nil and obj.status.conditions ~= nil then
        for i, condition in ipairs(obj.status.conditions) do
            if condition.status == "True" or condition.reason == "InstallSuccessful" or condition.reason == "UpgradeSuccessful" then
                hs.status = "Healthy"
                hs.message = "Install Successful"
                return hs
            end
        end
    end
    hs.status = "Progressing"
    hs.message = "Waiting for Central to deploy."
    return hs
```