# multi-docker-demo

[![travis-ci](https://travis-ci.com/CleberOliveira87/multi-docker-demo.svg?branch=main)](https://travis-ci.com/github/CleberOliveira87/multi-docker-demo)

→ This is a demo application for testing CI/CD flow with multiple containers on AWS EB
→ It's just a project to test various containers.

FLOW

1. Push code to github
2. Travis automatically pulls repo
3. Travis builds a test image, tests code
4. Travis builds prod images
5. Travis pushes built prod images to Docker Hub
6. Travis pushes project to AWS EB
7. EB pulls images from Docker Hub, deploys

```html
<h2>Example of code</h2>

<pre>
    <div class="container">
        <div class="block two first">
            <h2>APPLICATION</h2>
            <div class="wrap">     
                                            ---------> nginx/client (React App) / port:3000 
                                            |            |
browser ------>  nginx/proxy (port:80) ----              |
                                            |             v
                                            ---------> API (Node) ----> Redis <-----> Worker (Node)
                                                                  | 
                                                                  |
                                                                  ----> Postgres 
            </div>
        </div>
    </div>
</pre>
```