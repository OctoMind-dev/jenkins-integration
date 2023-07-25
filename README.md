# jenkins-integration

To trigger automagically from your GitHub repo inside Jenkins, you need to:

1. Add WebHook to your GitHub Repository
2. Extend your Jenkinsfile with [script in this repo](Jenkinsfile)

## Adding webhook

1. Select your repository `Settings`, go to `Webhooks` and click `Add webhook.` 
![Webhook-01](/docs/Webhook-01.png) <br />
2. For Payload URL set `<your-jenkins-url>/github-webhook/` and select `application/json` for Content type.
Also select triggers that best suite your need. 
In general, `the push event` is enough, but for this example, we also want to trigger automagically on `Pull Request`.
![Webhook-02](/docs/Webhook-02.png) <br />
Check `Pull requesuts` and `Pushes`, after that click on `Add webhook`.
![Webhook-03](/docs/Webhook-03.png) <br />
3. If everything is ok, you should see green checkmark after few moments.
![Webhook-04](/docs/Webhook-04.png) <br />

## Creating Jenkins pipeline

If you're new to Jenkins, this is how you can create a pipeline to test the script

1. On your Dashboard page, click on `+ New Item`
![Jenkins-01](/docs/Jenkins-01.png) <br />
2. Give your Project a name, select Pipeline and press OK
![Jenkins-02](/docs/Jenkins-02.png) <br />
3. You can check `GitHub project` and paste url to your repo so you will have a button on your dashboard to take you to your repo.
You need to check `GitHub hook trigger for GITScm polling` so our previously created hook can trigger our pipeline.
![Jenkins-03](/docs/Jenkins-03.png) <br />
4. For the `Definition`, you should select `Pipeline script from SCM`.
For `SCM` you should select `Git`
After adding your `Repository URL`, you also need to provide credentials if it is Private repo.
You also need to specify a branch, in this example `*/*` will trigger the pipeline on pushing to any branch.
If you want to trigger pipeline only on specific branch changes, e.g. main, you should put `*/main`.
The last field asks you to set the path and name to Jenkinsfile, but in our repository, it is on top level, and it is name Jenkinsfile.
Lastly, click on save
![Jenkins-04](/docs/Jenkins-04.png) <br />
5. You should add you `AUTOMAGICALLY_TOKEN` to your secrets. Navigate to `Dashboard -> Manage jenkins -> Credentials -> System -> Global credentials (unrestricted)` and click on `Add Credentials`.
For the `Kind` you need to select `Secret text`. Be careful that your `ID` matches the `ID` that you call within the Jenkinsfile script. Put your Token value to Secret field and click `Create`.
![Jenkins-05](/docs/Jenkins-05.png) <br />

If you want, you can enable safe parsing of HTML so you can click the link to test report automagically provided for you.
Navigate to `Dashboard -> Manage jenkins -> Security` and for `Markup Formatter` select `Safe HTML`
![Jenkins-06](/docs/Jenkins-06.png) <br />
