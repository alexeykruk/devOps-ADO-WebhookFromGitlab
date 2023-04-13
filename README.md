## Trigger azure devops (ADO) pipeline from gitlab

In Gitlab let's create **Webhook**

![Gitlab Webhooks](./images/img02.jpg)

URL:
https://dev.azure.com/*PROJECT_NAME*/_apis/public/distributedtask/webhooks/gitlab-push-webhook/?api-version=6.0-preview
- [x] Trigger: Push events
- [ ] ...
- [x] Enable SSL verification

&nbsp;

Our Project hook
</br>
![Project Webhook](./images/img03.jpg)

&nbsp;

---

&nbsp;

Create ADO Service connection with type **Incoming WebHook**

![Service connection](./images/img04.jpg)

![Service connection](./images/img01.jpg)


### azure-pipelines.yml
```
resources:
  webhooks:
  - webhook: WebHook
    connection: gitlab-push-webhook

pool:
  vmImage: 'ubuntu-latest'

steps:

- task: Bash@3
  condition: eq('${{ parameters.WebHook.repository.name }}', 'REPO_NAME')
  displayName: 'Run pipeline'
  inputs:
    targetType: 'inline'
    script: |
      az pipelines run --org https://dev.azure.com/ADO_PROJECT_NAME --name 'PIPELINE_NAME' --project 'PROJECT_NAME'
  env:
    AZURE_DEVOPS_EXT_PAT: $(System.AccessToken)
```
