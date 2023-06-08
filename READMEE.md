# ECS Service Stop Workflow

This GitHub Actions workflow allows you to stop an ECS service in an AWS ECS cluster. It stops all running tasks associated with the service, terminates EC2 instances without running tasks, and downsizes the associated Auto Scaling Group.

## Inputs

- `cluster-name` (required): The name of the ECS cluster where the service is running.
- `service-name` (required): The name of the ECS service to stop.

## Permissions

Make sure to provide the necessary permissions for this workflow to run successfully. The following permissions are required:

- `id-token`: `write` (required for AWS OIDC connection)
- `contents`: `read` (required for actions/checkout)
- `pull-requests`: `write` (required for the GH bot to comment on PRs)

## Workflow Steps

1. **Checkout code**: Checks out the code repository.
2. **Configure AWS credentials**: Configures AWS credentials for accessing the ECS cluster.
3. **Get running task count**: Retrieves the number of running tasks for the specified ECS service and stores it as an environment variable.
4. **Create SSM parameter**: Creates an AWS SSM parameter to store the task count if it is not zero.
5. **Get EC2 instance details**: Retrieves EC2 instance details associated with the running tasks, such as instance IDs and container instance IDs.
6. **Stop running tasks**: Stops all running tasks for the ECS service by setting the desired count to 0.
7. **Sleep for 1 minute**: Waits for 1 minute to allow time for the tasks to stop gracefully.
8. **Terminate EC2 instances without running tasks and Downsize ASG**: Terminates EC2 instances that don't have any running tasks and downsizes the associated Auto Scaling Group if applicable.

## How to Use

To use this workflow, follow these steps:

1. Ensure that the workflow file containing the provided code is in your repository.
2. Modify the workflow file if needed, such as updating the AWS region or customizing the sleep duration.
3. Configure the necessary secrets in your GitHub repository settings:
   - `ROLE_TO_ASSUME`: The role to assume for AWS authentication.
4. Customize the workflow inputs and behavior as needed.
5. Trigger the workflow manually by navigating to the "Actions" tab and selecting the workflow from the list. Provide the required inputs (`cluster-name` and `service-name`) when prompted.

That's it! The workflow will execute the steps to stop the ECS service, terminate EC2 instances without running tasks, and downsize the associated Auto Scaling Group if applicable.

**Note**: Make sure you have the necessary AWS credentials and permissions set up to access the ECS cluster and perform the required actions.

Feel free to update and customize this workflow to fit your specific needs!

If you have any questions or need further assistance, please don't hesitate to reach out.

Happy coding!
