# CI-CD-Github-Actions
This project is to collect information and examples to understand CI-CD with Github Actions.

---
## Definition and example using ChatGPT
- Input prompt:
```python
prompt = f"""
Your task is to explain how Github actions are working to implement a CI pipeline.

Show a small example in python how to implement a CI pipeline with unit tests and \
image deployment for Github. 

Show me the resources you used to gather these information. 
"""
response = get_completion(prompt)
print(response)
```

- Output: _Generated by ChatGPT-3.5-Turbo_

Github actions are a feature of Github that allows developers to automate tasks such as building, testing, and deploying code. In the context of a CI pipeline, Github actions can be used to automatically run tests on code changes and deploy the code to a production environment if the tests pass.

To implement a CI pipeline with Github actions, you would first need to create a workflow file in your repository. This file would define the steps that Github should take when a code change is pushed to the repository. The workflow file would typically include steps to build the code, run unit tests, and deploy the code to a production environment if the tests pass.

Here is an example of a workflow file for a Python project:

```
name: CI

on:
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Set up Python
      uses: actions/setup-python@v2
      with:
        python-version: '3.8'
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
    - name: Run tests
      run: |
        python -m unittest discover tests/
    - name: Deploy to production
      if: success()
      uses: appleboy/ssh-action@master
      with:
        host: ${{ secrets.PRODUCTION_HOST }}
        username: ${{ secrets.PRODUCTION_USERNAME }}
        password: ${{ secrets.PRODUCTION_PASSWORD }}
        script: |
          cd /var/www/myapp
          git pull
          systemctl restart myapp.service
```

In this example, the workflow is triggered when a code change is pushed to the main branch. The workflow includes a single job called "build" that runs on an Ubuntu environment. The job includes several steps:

- Checkout the code from the repository
- Set up Python 3.8
- Install the project dependencies
- Run the unit tests using the `unittest` module
- Deploy the code to a production environment using an SSH action if the tests pass

The SSH action is used to connect to a remote server and run a script that pulls the latest code changes and restarts the application service.

To use this workflow in your own project, you would need to create a `.github/workflows` directory in your repository and create a file called `ci.yml` (or any other name you choose) with the contents of the example above.

Resources used:
- https://docs.github.com/en/actions/guides/building-and-testing-python
- https://github.com/appleboy/ssh-action

---
