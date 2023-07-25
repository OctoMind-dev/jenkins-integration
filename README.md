# jenkins-integration

To trigger automagically from your GitHub repo inside Jenkins, you need to:

1. Add WebHook to your GitHub Repository
2. Extend your Jenkinsfile with [script in this repo](Jenkinsfile)

## Adding webhook

1. Select your repository Settings, go to Webhooks and click Add webhook. 
(image goes here)
2. For Payload URL set `<your-jenkins-url>/github-webhook/` and select `application/json` for Content type.
Also select triggers that best suite your need. 
In general, `the push event` is enough, but for this example, we also want to trigger automagically on Pull Request.
(image goes here)
Check `Pull requesuts` and `Pushes`, after that click on Add webhook.
(image goes here)
3. If everything is ok, you should see green checkmark after few moments.
(image goes here)

## Creating Jenkins pipeline

If you're new to Jenkins, this is how you can create a pipeline to test the script

1. On your Dashboard page, click on `+ New Item`
(image goes here)
2. Give your Project a name, select Pipeline and press OK
(image goes here)
3. You can check `GitHub project` and paste url to your repo so you will have a button on your dashboard to take you to your repo.
You need to check `GitHub hook trigger for GITScm polling` so our previously created hook can trigger our pipeline.
(image goes here)
4. For the `Definition`, you should select `Pipeline script from SCM`.
For `SCM` you should select `Git`
After adding your `Repository URL`, you also need to provide credentials if it is Private repo.
You also need to specify a branch, in this example `*/*` will trigger the pipeline on pushing to any branch.
If you want to trigger pipeline only on specific branch changes, e.g. main, you should put `*/main`.
The last field asks you to set the path and name to Jenkinsfile, but in our repository, it is on top level, and it is name Jenkinsfile.
Lastly, click on save
(image goes here)
5. You should add you `AUTOMAGICALLY_TOKEN` to your secrets. Navigate to `Dashboard -> Manage jenkins -> Credentials -> System -> Global credentials (unrestricted)` and click on `Add Credentials`.
For the `Kind` you need to select `Secret text`. Be careful that your `ID` matches the `ID` that you call within the Jenkinsfile script. Put your Token value to Secret field and click `Create`.
(image goes here)

If you want, you can enable safe parsing of HTML so you can click the link to test report automagically provided for you.
Navigate to `Dashboard -> Manage jenkins -> Security` and for `Markup Formatter` select `Safe HTML`
