# multi-docker-demo

[![travis-ci](https://travis-ci.com/CleberOliveira87/multi-docker-demo.svg?branch=main)](https://travis-ci.com/github/CleberOliveira87/multi-docker-demo)

→ This is a demo application for testing CI/CD flow with multiple containers on AWS Elastic Beanstalk + Travis CI. 

## FLOW

1. Push code to github
2. Travis automatically pulls repo
3. Travis builds a test image, tests code
4. Travis builds prod images
5. Travis pushes built prod images to Docker Hub
6. Travis pushes project to AWS EB
7. EB pulls images from Docker Hub, deploys

## APPLICATION SETUP DEVELOPMENT 
```html
                                            ---------> nginx/client (React App) / port:3000 
                                            |            |
browser ------>  nginx/proxy (port:80) ----              |
                                            |            v
                                            ---------> API (Node) ----> Redis <-----> Worker (Node)
                                                                  | 
                                                                  |
                                                                  ----> Postgres 
```

## APPLICATION SETUP PRODUCTION 
```html
                                            ---------> nginx/client (Container)  Worker (Container)
                                            |            |
browser ------>  nginx/proxy (Container) ----            |
                                            |            v
                                            ---------> API (Container) ------------------------------> Redis (AWS Elastic Cache)  
                                                                      | 
                                                                      |
                                                                      -------------------------------> Postgres (AWS Relational Database Service) 
```

```html

                   delegates to...                     
Elastic Beanstalk -----------------> Elastic Container Service (ECS) 
                                       |
                                       ----> receive multi task definition (Container Definitions): instructions on how to run a single container.
                                       ----> through the file Dockerrun.aws.json 
                                          

```

## AWS Configuration Cheat Sheet for Demo

1. Create EBS Application (In Platform Branch, select Multi-Container Docker running on 64bit Amazon Linux)
2. Create RDS Database (PostgreSQL | VPC is set to Default VPC)
3. Create ElastiCache Redis (Make sure VPC is set to default VPC | Make sure Cluster Mode Enabled is NOT ticked | Change Node type to 'cache.t2.micro' | Change Replicas per Shard to 0)
4. Create a Custom Security Group (Make sure VPC is set to default VPC | Click Add Rule - Set Port Range to 5432-6379 - Postgres and Redis)
5. Apply Custom Security Groups to ElastiCache, RDS and Elastic Beanstalk
6. Create IAM user for deployment
7. Set IAM Keys as Environment Variables in Travis CI Platform
8. Add AWS configuration details to .travis.yml file's deploy script
9. Set Environment Variables to EBS Application (database credentials, etc)
10. Deploying App with git push

## Doc Links

[AWS - Task definition parameters DOC](https://docs.aws.amazon.com/AmazonECS/latest/userguide/task_definition_parameters.html) | 
[AWS - Container definitions DOC](https://docs.aws.amazon.com/AmazonECS/latest/userguide/task_definition_parameters.html#container_definitions)