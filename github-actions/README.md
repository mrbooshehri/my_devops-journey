GitHub Actions is a feature provided by GitHub that allows you to automate software workflows directly in your GitHub repository. It enables developers to build, test, package, release, or deploy code right from GitHub. With GitHub Actions, you can create custom software development life cycle (SDLC) processes, including continuous integration and delivery (CI/CD), without needing to set up your own CI/CD infrastructure.

Here are some key aspects of GitHub Actions:

1. **Workflow Files**: Workflows are defined using YAML files stored in the `.github/workflows` directory of your repository. These files specify the events that trigger the workflow (e.g., push, pull request, manual trigger) and the jobs that should run as part of the workflow.

2. **Jobs**: A job is a set of steps that execute on the same runner. Steps can run commands, run setup tasks, or run an action in your repository, a public repository, or an action published in a Docker registry.

3. **Steps**: Steps are individual tasks that can run commands, run setup tasks, or run an action in your repository, a public repository, or an action published in a Docker registry.

4. **Runners**: Runners are virtual environments where your jobs are executed. GitHub provides runners hosted by GitHub, which come pre-installed with common languages and tools, or you can use self-hosted runners if you need more control over the environment.

5. **Actions Marketplace**: The GitHub Actions marketplace offers a wide range of pre-built actions contributed by the community and GitHub. This makes it easy to integrate third-party services into your workflows.

6. **Security and Permissions**: GitHub Actions uses permissions to control access to secrets, tokens, and other resources. You can define these permissions at different scopes: repository, organization, or enterprise level.

### Example Workflow

Here’s a simple example of a GitHub Actions workflow file (`example.yml`) that runs every time code is pushed to the `main` branch:

```yaml
name: CI

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Run a one-line script
      run: echo Hello, world!
```

This workflow does the following:
- Triggers on every push to the `main` branch.
- Defines a single job named `build`.
- Specifies that the job runs on the latest version of Ubuntu.
- Checks out your repository under `$GITHUB_WORKSPACE`, so your workflow can access it.
- Runs a simple command `echo Hello, world`.

# What is Events

In GitHub Actions, an "event" refers to the specific activity in a GitHub repository that triggers a workflow. Events can be anything from someone pushing code to a repository, opening a pull request, creating a new issue, or even manually triggering a workflow through the GitHub UI. Each event type has its own set of data associated with it, which can be used within the workflow to perform various tasks such as building, testing, or deploying code.

Events are defined in the workflow file (`.yml` or `.yaml` file) under the `on:` section. Here, you specify the types of events that will trigger the workflow. For example, you might want a workflow to run whenever code is pushed to the `main` branch, or when a pull request is opened against the `main` branch.

## Types of Events

Some common event types include:

- `push`: Triggered when commits are pushed to a repository.
- `pull_request`: Triggered when a pull request is opened, synchronized, reopened, or closed.
- `issue_comment`: Triggered when a comment is made on an issue or pull request.
- `schedule`: Triggered according to a schedule using cron syntax.
- `workflow_dispatch`: Allows the workflow to be triggered manually from the GitHub UI.

## Event Data

Each event carries a payload of data about the event itself. For example, a `push` event includes details like the commit message, author, and the list of commits included in the push. This data can be accessed within the workflow using context objects like `github.event` for the raw JSON payload, or `github.event_name` for specific properties related to the event.

## Example: Using an Event to Trigger a Workflow

Here’s how you might configure a workflow to run only when a pull request is opened against the `main` branch:

```yaml
name: PR Workflow

on:
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Run tests
      run: make test
```

In this example:
- The workflow is triggered by the `pull_request` event.
- It specifies that the workflow should only run when a pull request is opened against the `main` branch.
- The workflow defines a single job called `build`.
- The job checks out the repository and then runs tests using a hypothetical `make test` command.

> The line `- uses: actions/checkout@v2` in a GitHub Actions workflow file is a directive that tells the workflow to use a specific action before proceeding with the rest of the steps in the job. Let's break down this line to understand its components and purpose:
> 
> ### Components Explained
> 
> - **`uses`**: This keyword is used to specify an action that the workflow should run. Actions are reusable units of code that can perform a variety of tasks, such as checking out code, running scripts, or interacting with external services.
> 
> - **`actions/checkout@v2`**: This is the identifier of the action being used. It consists of three parts separated by slashes (`/`):
>   - **`actions`**: The namespace of the action. This indicates that the action comes from the GitHub Actions marketplace, specifically from the official GitHub Actions organization.
>   - **`checkout`**: The name of the action. The `checkout` action is responsible for checking out your repository's code onto the runner, making it accessible for the rest of the workflow to work with. This is typically the first step in many workflows, as it sets up the necessary environment for further operations like building, testing, or deploying code.
>   - **`v2`**: The version of the action. Specifying a version ensures that the workflow uses a consistent and known version of the action, avoiding unexpected behavior due to changes in newer versions. Versioning helps maintain compatibility and stability across different workflows.
> 
> ### Purpose and Usage
> 
> The primary purpose of the `actions/checkout@v2` action is to prepare the workflow runner with the contents of the repository. Without this step, the runner would have no knowledge of the repository's structure or content, making it impossible to execute commands that depend on the repository's files.
> 
> For example, if your workflow includes steps to build or test your code, those steps require access to the source code files. The `actions/checkout@v2` action ensures that these files are present and correctly checked out on the runner.
> 
> Here's a simple example of how it might be used in a workflow:
> 
> ```yaml
> name: Build and Test
> 
> on: [push]
> 
> jobs:
>   build:
>     runs-on: ubuntu-latest
> 
>     steps:
>     - uses: actions/checkout@v2
> 
>     - name: Set up Node.js
>       uses: actions/setup-node@v2
>       with:
>         node-version: '14'
> 
>     - name: Install Dependencies
>       run: npm ci
> 
>     - name: Run Tests
>       run: npm test
> ```
> 
> In this example, after the `actions/checkout@v2` step, the workflow proceeds to set up Node.js, install dependencies, and run tests. All these steps rely on having access to the repository's code, which is provided by the `actions/checkout@v2` action.

# Syntax Explanation

The syntax of GitHub Actions is primarily defined in YAML (Yet Another Markup Language), which is a human-readable data serialization standard. In the context of GitHub Actions, YAML files are used to define workflows, specifying the conditions under which they should run, the jobs they should execute, and the steps within those jobs. Understanding the key components of this syntax is essential for creating effective workflows.

Here's a breakdown of the most commonly used keywords in GitHub Actions workflow syntax:

### Workflow Level Keywords

- `name`: Specifies the name of the workflow.
- `on`: Defines the event(s) that trigger the workflow. Can be a push, pull request, schedule, etc.
- `env`: Sets environment variables for the entire workflow.
- `defaults`: Sets default values for inputs and outputs of jobs and steps.

### Job Level Keywords

- `jobs`: Contains a list of jobs that the workflow will execute.
- `runs-on`: Specifies the type of runner that the job will run on (e.g., `ubuntu-latest`, `windows-latest`).
- `container`: Specifies a container image to run the job in instead of a runner.
- `strategy`: Configures the strategy for parallelism and job matrix.
- `timeout-minutes`: Sets a timeout duration for the job.

### Step Level Keywords

- `steps`: Contains a list of steps to execute as part of the job.
- `uses`: Specifies an action to run. Can reference a public action, a private repository action, or a Docker image.
- `with`: Passes input parameters to the action specified in `uses`.
- `run`: Executes a shell command or script.
- `shell`: Specifies the shell to use for executing the `run` command.
- `working-directory`: Sets the working directory for the `run` command.
- `env`: Sets environment variables for the step.
- `outputs`: Defines output variables that can be used by subsequent steps or jobs.

### Contexts and Expressions

- `github`: Provides context about the current GitHub event, such as the repository, sender, and event payload.
- `env`: Accesses environment variables.
- `secrets`: Accesses encrypted secrets stored in the repository settings.
- `job`: Accesses information about the current job, such as its ID and status.
- `step`: Accesses information about the current step, such as its ID and status.
- `fromJson`: Converts a string to a JSON object.
- `toJson`: Converts a JSON object to a string.

### Control Flow

- `if`: Conditionally executes a step or job based on the result of an expression.
- `continue-on-error`: Continues the workflow even if a step fails.
- `needs`: Specifies that a job depends on the completion of another job.
- `depends-on`: Similar to `needs`, but with more options for controlling dependencies between jobs.

### Artifacts

- `artifacts`: Specifies artifacts produced by a job that should be passed to subsequent jobs.
- `paths`: Lists the paths to files/directories to be saved as artifacts.
- `cache`: Caches directories or files between jobs to speed up workflows.

### Inputs and Outputs

- `inputs`: Defines inputs for a job or step.
- `outputs`: Defines outputs for a job or step, which can be used by subsequent steps or jobs.


# `strategy` keyword

The `strategy` keyword in GitHub Actions is used within a job definition to configure how the job's steps are executed, particularly regarding parallelism and job matrices. It allows you to control the number of jobs that run simultaneously and how to distribute them across multiple runners. This is especially useful for optimizing performance and resource usage in your workflows, especially when dealing with large numbers of jobs or complex workflows.

### Basic Syntax

The `strategy` block is defined at the job level and can contain two main sub-keys:

- `max-parallel`: Specifies the maximum number of jobs that can run concurrently. If not specified, all jobs in the job matrix will run in parallel.
- `fail-fast`: Determines whether the workflow should stop as soon as any job fails. If set to `true`, the workflow stops immediately upon the failure of any job. If omitted or set to `false`, the workflow continues to run until all jobs complete, regardless of failures.

### Example Usage

Here’s an example of how to use the `strategy` keyword in a GitHub Actions workflow:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      max-parallel: 2
      fail-fast: false
    steps:
    - uses: actions/checkout@v2
    - name: Run tests
      run: |
        # Your test commands here
```

In this example:
- The `build` job is configured to run on the latest version of Ubuntu.
- The `strategy` block specifies that a maximum of 2 jobs can run in parallel (`max-parallel: 2`). This means if the job matrix results in more than 2 jobs, they will be queued and run one after the other, up to the limit.
- The `fail-fast` option is set to `false`, indicating that the workflow will continue to run even if some jobs fail. This is useful for scenarios where you want to collect results from all jobs, even if some fail.

### Advanced Strategy Options

The `strategy` block can also include more advanced options, such as:

- `matrix`: Allows you to define a matrix of configurations for the job, enabling you to run the job with different combinations of environment variables, operating systems, etc.
- `concurrency`: Limits the number of concurrent executions of the job across all forks of the repository.

### Conclusion

Using the `strategy` keyword effectively can help optimize your GitHub
Actions workflows, especially when dealing with large-scale builds or
tests that benefit from parallel execution. It gives you fine-grained
control over how jobs are scheduled and executed, allowing for efficient
use of resources and streamlined CI/CD processes.

# `matrix` keyword

The `matrix` keyword in GitHub Actions is a powerful feature that allows you to run a job multiple times with different configurations. It's particularly useful for testing applications across various environments, operating systems, or versions of dependencies. By specifying a matrix, you can easily scale your workflows to cover a broad range of scenarios without duplicating effort.

### Basic Syntax

The `matrix` keyword is used within a job definition to define a set of configurations. Each configuration is a combination of environment variables that the job will run with. Here's the basic syntax:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [12.x, 14.x, 16.x]
    steps:
    - uses: actions/checkout@v2
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v2
      with:
        node-version: ${{ matrix.node-version }}
    - run: npm ci && npm test
```

In this example, the `build` job will run three times, once for each version of Node.js specified in the matrix (12.x, 14.x, and 16.x). The `${{ matrix.node-version }}` syntax is used to dynamically pass the value of each item in the matrix to the job.

### Advanced Matrix Features

- **Combining Matrices**: You can combine multiple matrices to create complex configurations. For instance, you could have one matrix for Node.js versions and another for operating system types.

- **Matrix Strategy**: Besides specifying a matrix, you can also control how the jobs are distributed across runners using the `strategy.matrix.strategy` field. This can be set to `concurrent` to allow jobs to run in parallel, or `fixed` to run jobs sequentially.

- **Conditional Execution**: You can conditionally include items in the matrix based on certain criteria, such as the presence of a specific label in the workflow dispatch event.

### Example with Multiple Matrices

Here’s an example demonstrating the use of multiple matrices:

```yaml
jobs:
  build:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest]
        node-version: [12.x, 14.x, 16.x]
    steps:
    - uses: actions/checkout@v2
    - name: Setup Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v2
      with:
        node-version: ${{ matrix.node-version }}
    - run: npm ci && npm test
```

In this expanded example, the job will run six times, covering each combination of the operating system (`os`) and Node.js version (`node-version`). This approach is highly efficient for testing applications across a wide array of environments and configurations.

### Conclusion

The `matrix` keyword in GitHub Actions is a versatile tool for running jobs with multiple configurations, facilitating comprehensive testing and deployment scenarios. By leveraging the power of matrices, you can ensure that your applications are thoroughly vetted across a variety of setups, enhancing the reliability and robustness of your software.

# Real word example

## Docker build and push action

Docker is a popular platform used for developing, shipping, and running applications inside containers. Containers allow developers to package an application along with its environment, libraries, and dependencies, ensuring that the application runs consistently across different computing environments. GitHub Actions can be used to automate the process of building a Docker image and pushing it to a Docker registry, such as Docker Hub or GitHub Packages.

### Docker Build Action

The Docker build action is a step in a GitHub Actions workflow that compiles a Dockerfile into a Docker image. This process involves reading the Dockerfile, executing the instructions it contains (such as copying files, installing dependencies, setting environment variables, exposing ports, etc.), and finally creating a Docker image. Once the image is built, it can be tagged and pushed to a Docker registry for distribution and deployment.

### Docker Push Action

After building a Docker image, the next step is often to push it to a Docker registry. This makes the image available for deployment to servers, Kubernetes clusters, or other environments. The Docker push action uploads the Docker image to a registry, making it accessible for others to pull and use.

### Example Workflow

Below is an example of a GitHub Actions workflow that demonstrates how to build and push a Docker image:

```yaml
name: Build and Push Docker Image

on:
  push:
    branches:
      - main

jobs:
  build-and-push:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Log in to DockerHub
      uses: docker/login-action@v1
      with:
        username: ${{ secrets.DOCKERHUB_USERNAME }}
        password: ${{ secrets.DOCKERHUB_TOKEN }}

    - name: Build and push Docker image
      uses: docker/build-push-action@v2
      with:
        context:.
        push: true
        tags: user/app-name:latest
```

### Explanation

1. **Checkout Code**: The first step checks out your repository's code onto the runner.

2. **Log in to DockerHub**: This step logs into Docker Hub using credentials stored as secrets in your GitHub repository. It uses the `docker/login-action@v1` action, passing the username and token as inputs.

3. **Build and Push Docker Image**: The final step builds the Docker image using the Dockerfile in the root of your repository and pushes it to Docker Hub. The `context` is set to `.` (the current directory), meaning the Dockerfile is located in the root of your repository. The `tags` input specifies the tag for the image, in this case, `user/app-name:latest`.

### Secrets

It's important to note that storing your Docker Hub username and token as secrets in your GitHub repository is crucial for security. This way, your credentials are not exposed in your workflow files or logs.

### Conclusion

Automating the build and push of Docker images using GitHub Actions
simplifies the deployment process, making it easier to update and
distribute your applications. By integrating Docker build and push
actions into your CI/CD pipeline, you can ensure that your Docker images
are always up-to-date and readily available for deployment.

> **Note:**
> In GitHub Actions, a "secret" is a piece of data that you don't want to expose publicly, such as API keys, passwords, or tokens. Secrets are encrypted and can only be accessed by GitHub Actions running in the same repository. They provide a secure way to store and use sensitive information in your workflows without exposing it in your code or logs.
> 
> ### Setting Up Secrets
> 
> To set up secrets in GitHub, follow these steps:
> 
> 1. **Navigate to Your Repository**: Go to the main page of the repository where you want to add the secret.
> 
> 2. **Access Settings**: Click on the "Settings" tab, usually found near the top-right corner of the repository page.
> 
> 3. **Secrets Section**: Scroll down to the "Secrets" section in the left sidebar and click on it.
> 
> 4. **New Repository Secret**: Click on the "New repository secret" button.
> 
> 5. **Enter Details**: Provide a name for the secret in the "Name" field. This name is how you'll refer to the secret in your workflow files. Then, enter the actual value of the secret in the "Value" field. Be careful not to share this value with anyone outside of your team.
> 
> 6. **Add Secret**: After entering the name and value, click on the "Add secret" button to save it.
> 
> ### Using Secrets in Workflows
> 
> Once you've added a secret to your repository, you can use it in your GitHub Actions workflows by referencing it with the `secrets` context. For example, if you have a secret named `MY_SECRET_KEY`, you can use it in a workflow like this:
> 
> ```yaml
> jobs:
>   deploy:
>     runs-on: ubuntu-latest
>     steps:
>     - name: Deploy to production
>       run: echo "Deploying with secret key ${{ secrets.MY_SECRET_KEY }}"
> ```
> 
> In this example, `${{ secrets.MY_SECRET_KEY }}` is replaced with the actual value of the `MY_SECRET_KEY` secret during the workflow run. This allows you to securely use the secret's value in your workflow steps without exposing it in your workflow files or logs.
> 
> ### Best Practices
> 
> - **Limit Access**: Only grant access to secrets to individuals who absolutely need them for their work.
> - **Rotate Regularly**: Change your secrets periodically to minimize the impact if they ever get compromised.
> - **Audit Logs**: Keep an eye on the audit logs for any unauthorized access attempts to your secrets.
> 
> By using secrets effectively, you can enhance the security of your GitHub Actions workflows, protecting your repository's sensitive information from unauthorized access.


> **Note:** Only `ubuntu-latest` has the `docker` pre-installed

## Deploy build on a real server

Deploying a build project to a remote server using SSH and a specific directory involves several steps. Below is a general guide on how to achieve this using GitHub Actions. This example assumes you have already built your project and packaged it appropriately for deployment.

### Prerequisites

1. **SSH Key**: Ensure you have an SSH key generated and added to the `~/.ssh/authorized_keys` file on the target server (`10.1.1.25` in this case). Also, make sure the corresponding private key is securely stored as a GitHub secret in your repository.

2. **Server Configuration**: Verify that the server is configured to accept SSH connections and that the `/repo` directory exists and is writable by the user account you'll be connecting as.

### GitHub Actions Workflow

Create a new workflow file in your repository under `.github/workflows/deploy.yml`. Here's an example workflow that deploys your build project to the server:

```yaml
name: Deploy to Server

on:
  push:
    branches:
      - main  # Adjust this to match your default branch

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Deploy to server
      uses: appleboy/ssh-action@master
      with:
        host: ${{ secrets.SERVER_HOST }}
        username: ${{ secrets.SERVER_USER }}
        key: ${{ secrets.SSH_PRIVATE_KEY }}
        script: |
          cd /repo
          rm -rf *
          tar -xzf your_project.tar.gz  # Assuming your project is packaged as a.tar.gz file
         ./your_project/run.sh  # Replace with the appropriate command to start your project
```

### Explanation

- **Checkout Code**: This step checks out your repository's code onto the runner.

- **Deploy to Server**: Uses the `appleboy/ssh-action` action to connect to your server via SSH. You need to replace `your_project.tar.gz` and `./your_project/run.sh` with the actual filename of your project archive and the command to start your project.

### Secrets

You need to add the following secrets to your GitHub repository:

- `SERVER_HOST`: The IP address or hostname of your server (`10.1.1.25`).
- `SERVER_USER`: The username for SSH authentication.
- `SSH_PRIVATE_KEY`: The private key corresponding to the public key added to the `~/.ssh/authorized_keys` file on the server.

### Running the Workflow

Whenever you push changes to the `main` branch (or whichever branch you specify), this workflow will trigger, checking out your code, connecting to the server via SSH, and deploying your project to the `/repo` directory.

### Note

- Ensure your project is properly packaged and ready for deployment. This might involve compiling your code, bundling assets, or otherwise preparing your project for execution on the server.
- Adjust the `script` section to match the commands needed to deploy and start your specific project.
- Consider adding error handling and logging to the `script` section to better manage and troubleshoot deployments.

This workflow provides a basic framework for deploying projects to a server using GitHub Actions and SSH. Depending on your project's requirements, you may need to customize the deployment steps accordingly.
