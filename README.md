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

```html
## APPLICATION SETUP DEVELOPMENT 

→ It's just a project to test various containers.

                                            ---------> nginx/client (React App) / port:3000 
                                            |            |
browser ------>  nginx/proxy (port:80) ----              |
                                            |            v
                                            ---------> API (Node) ----> Redis <-----> Worker (Node)
                                                                  | 
                                                                  |
                                                                  ----> Postgres 
```

```html
## APPLICATION SETUP PRODUCTION 

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
[AWS - Task definition parameters DOC](https://docs.aws.amazon.com/AmazonECS/latest/userguide/task_definition_parameters.html)
[AWS - Container definitions DOC](https://docs.aws.amazon.com/AmazonECS/latest/userguide/task_definition_parameters.html#container_definitions)