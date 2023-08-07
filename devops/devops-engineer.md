# DevOps Engineer task

The goal of this task is to configure a web server inside a VPC in the AWS cloud.
Please, read the task carefully, focus on the big picture first and then proceed with the details. If the description does not mention something, the implementation is up to you.

1. Configure a VPC network on AWS with Terraform that:
    - contains only 32 IP addresses
    - is in a class B private IP address range
    - has two /28 subnets in this VPC: one public and one private
    - has an instance in a public network with a static IP
    - the instance should be accessible over the Internet to all addresses except:
      - 37.57.87.0 - 37.57.87.255
      - 37.57.86.0 - 37.57.86.255
      - 37.57.85.0 - 37.57.85.255
      - 37.57.80.0 - 37.57.83.255

   Keep Terraform scripts as simple as possible (e.g. no modules).

2. A host in a public subnet should have a web server and an application server configured with Ansible. The application to run is in the repository in a directory with this file. Use existing Ansible roles and customize their configuration to implement the following setup:

    - allow owners of [this](./devops-test-task.pub) ssh keypair to access the server
    - latest __release/*__ tag in this repository contains a version of the application to run
    - use Nginx as a web server
    - use php-fpm, version 8.0
    - Nginx should connect to php-fpm via a socket file
    - php-fpm must expose `/status` and `/ping` endpoints for status and health checks. `/status` and `/ping` endpoints must be available for _localhost_ connections only
    - webserver should be available for HTTP connections on port 8883
    - Nginx should respond to 404 error with a json response:

    ```json
      {
        "error": {
          "status_code": 404,
          "status": "Not Found"
        }
      }
    ```

3. Configure additional AWS services:

   - S3 service enpoint in private VPC
   - send Nginx access and error logs to CloudWatch
