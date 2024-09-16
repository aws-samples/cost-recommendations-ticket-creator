## My Project

TODO: Fill this README out!

Be sure to:

* Change the title in this README
* Edit your repository description on GitHub





*build code for jira
https://community.atlassian.com/t5/Jira-questions/Jira-Next-Gen-Python-API-Create-Issue-with-Custom-Field/qaq-p/2036396


* build layer
https://towardsdatascience.com/python-packages-in-aws-lambda-made-easy-8fbc78520e30

mkdir python
cd python
cp -r ../v-env/lib64/python3.7/site-packages/* .
cd ..
zip -r jira_layer.zip python
aws lambda publish-layer-version --layer-name jira --zip-file fileb://jira_layer.zip --compatible-runtimes python3.11

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

