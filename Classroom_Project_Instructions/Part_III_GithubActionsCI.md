# Part 3 - Set Up GitHub Actions Continuous Integration Pipeline

To enable CI/CD for your multi-container Kubernetes application, we will be setting up a GitHub Actions workflow to build and push our application Docker images to DockerHub whenever changes are pushed to the main branch in our GitHub repository.

### Create Dockerhub Repositories

Firstly, make sure you have created your DockerHub repositories as outlined before:

* `reverseproxy`
* `udagram-api-user`
* `udagram-api-feed`
* `udagram-frontend`

Ensure that the repository names on DockerHub match the image names specified in your *docker-compose-build.yaml* file.

### Set Up GitHub Actions Workflow

To implement our workflow using GitHub Actions:

1. In the root directory of your GitHub repository, create a directory named `.github/workflows` if it does not already exist.

2. Create a new YAML file inside this directory. This could be named `main.yml` or something similar to reflect its purpose. The name of the file will serve as the name of the workflow.

3. Define the workflow with the necessary steps to build and push images to your DockerHub registry. Here's a basic template for a GitHub Actions workflow:

    ```yaml
    name: CI to DockerHub

    on:
      push:
        branches: [ main ]

    jobs:
      build-and-push:
        runs-on: ubuntu-latest
        steps:
          - name: Check Out Repo
            uses: actions/checkout@v2

          - name: Log in to DockerHub
            uses: docker/login-action@v1
            with:
              username: ${{ secrets.DOCKERHUB_USERNAME }}
              password: ${{ secrets.DOCKERHUB_PASSWORD }}

          - name: Build and Push Docker Images
            run: |
              docker build -t your-dockerhub-username/reverseproxy ./reverseproxy
              docker push your-dockerhub-username/reverseproxy
              # Repeat build and push steps for each service
    ```

4. Make sure to replace `your-dockerhub-username` with your actual DockerHub username, and always use tags to differentiate image versions.

5. To securely pass credentials to actions, use GitHub secrets. You need to set `DOCKERHUB_USERNAME` and `DOCKERHUB_PASSWORD` in the repository secrets which can be found under "Settings" > "Secrets" of your repo.

6. Commit this new `.github/workflows/main.yml` file and push it to your repository. This will automatically trigger the workflow on the push event.

### Confirming Workflow Execution

To confirm everything is working:

1. Go to the "Actions" tab of your GitHub repository after you’ve committed changes to observe the workflow running.
2. Once completed, you should see if it passed or failed, along with output logs for each step.
   
3. Check DockerHub to see if the new images have been pushed successfully.


### Additional Resources

- [Introduction to GitHub Actions](https://docs.github.com/en/actions/learn-github-actions/introduction-to-github-actions)
- [Docker Hub](https://hub.docker.com/)
- [Docker Documentation](https://docs.docker.com/)