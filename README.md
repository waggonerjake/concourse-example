# Pipeline Webhook
This concourse pipeline has a webhook from Github that will automatically kick this off, rather than having this poll for changes. This uses a webhook token of `concourse` and uses [ngrok](https://ngrok.com/) (usage shown [here](https://docs.github.com/en/github-ae@latest/developers/webhooks-and-events/webhooks/creating-webhooks#exposing-localhost-to-the-internet)). 

Once we have made our localhost public via ngrok, we create the webhook on Github. To do this, go to `Settings > Webhooks > Add webhook`. The payload URL is the ngrok site (i.e. the public facing endpoint for your pipeline) appended with `/api/v1/teams/TEAM_NAME/pipelines/PIPELINE_NAME/resources/RESOURCE_NAME/check/webhook?webhook_token=WEBHOOK_TOKEN`. If you are using the default team, then `TEAM_NAME` is main. The `PIPELINE_NAME` is just the name of your pipeline (not the job). The `RESOURCE_NAME` is the name of the resource you want to trigger (not the resource type, but the name that is referred to throughout the .yaml). Lastly, the `WEBHOOK_TOKEN` is the token you will use in your .yaml, in this case its `concourse`. Once that all is set, click on **Add webhook**.

Once the webhook has been added on Github, you need to change your resource. Add the field `check_every: never` so it doesn't poll Github. Then add the webhook token field as well, like `webhook_token: 'concourse'`. Once these changes have been made you need to set the pipeline using fly, such as: `fly -t tutorial set-pipeline -p hello-world -c hello-world.yaml`. Once that is done you have succesfully set up your webhook.
