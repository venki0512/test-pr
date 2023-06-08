# ECS Service Stop

This GitHub Actions workflow allows you to stop an ECS service in an AWS cluster. It automates the process of stopping running tasks, creating an SSM parameter, and terminating EC2 instances associated with the service.

## Usage

To use this workflow, follow these steps:

1. Create a workflow file (e.g., `stop-service.yml`) in the `.github/workflows/` directory of your repository.

2. Copy the code from this repository's [stop-service.yml](./.github/workflows/stop-service.yml) file and paste it into your workflow file.

3. Customize the workflow by modifying the inputs section in the `workflow_dispatch` event. Provide the ECS cluster name and the ECS service name as inputs.

```yaml
inputs:
  cluster-name:
    description: 'ECS Cluster Name'
    required: true
  service-name:
    description: 'ECS Service Name'
    required: true
```
4. Set up AWS credentials and permissions:
  * Ensure that the necessary permissions are granted to the IAM role associated with your GitHub Actions workflow. The permissions section in the workflow file specifies the required permissions for the workflow.

  * Store the AWS role ARN in the repository's secrets with the name ROLE_TO_ASSUME. This secret will be used to configure AWS credentials.

  * Make sure to set the AWS region appropriately in the workflow file. The default region is set to us-east-1.

5.Commit and push the changes to your repository.

6. Trigger the workflow manually by going to the "Actions" tab of your repository and selecting the "ECS Service Stop" workflow. Provide the ECS cluster name and service name as inputs.

7. The workflow will execute the following steps:
  * Checkout code from the repository.
  * Configure AWS credentials using the provided role ARN and region.
  * Get the count of running tasks in the specified ECS cluster and service.
  * Create an SSM parameter with the running task count if there are any running tasks.
  * Retrieve EC2 instance details associated with the running tasks.
  * Stop the running tasks by setting the desired count to 0.
  * Sleep for 1 minute to allow the tasks to stop gracefully.
  * Terminate EC2 instances without running tasks and downsize associated Auto Scaling Groups if necessary.

8. The workflow will provide status updates in the GitHub Actions logs and perform the necessary actions based on the state of the ECS service.
