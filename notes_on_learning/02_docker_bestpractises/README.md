## Best Practices

1. Use a Docker linter, for example [hadolint](https://github.com/hadolint/hadolint) or [dockle](https://github.com/goodwithtech/dockle):

    ```
    $ find . -iname Dockerfile | xargs -I {} bash -c "echo {}; docker run --rm -i hadolint/hadolint < {}"
    ```

    You do not need neccessary to run it on CI/CD on every commit.

2. Please use smallest possible images:

   - [ubuntu](https://hub.docker.com/_/ubuntu/)
   - [alpine](https://hub.docker.com/_/alpine)

   Caveat: take the OS you know.

   See: [baseimage](https://phusion.github.io/baseimage-docker/)

2. Do not use random docker images, please.

3. Do not forget about <code>.dockerignore</code>, see [example for the prometheus project](https://github.com/prometheus/golang-builder/blob/master/.dockerignore).

4. Use a specific tag for your base docker image. Do not use *latest*.

5. Let CI/CD to generate the tag for the version.

6. Reduce the number of layers.

   What is the difference?

   <pre><code>RUN apt-get -y update && \
       apt-get install -y python
   </code></pre>

   vs

   <pre><code>RUN apt-get -y update
      RUN apt-get install -y python
   </code></pre>

   Notice: every <i>RUN</i>, <i>COPY</i>, <i>ADD</i> create a layer.

7. Reduce the size:

   - Be specific what you copy - <i>COPY</i>
   - Do not install, what you do not need, e.g., use <code>-no-install-recommends</code> for apt
   - Remove the cache in the same run as install:

     <pre><code>RUN apt-get -y update \
      && apt-get install -y vim \
      rm -rf /var/lib/apt/lists/*
     </code></pre>

8. Order. Things that changes the most often should be the last to be added.

9. Development time: optimize the Docker to your work, do not be afraid to add as many layers as you need to speed up your development process.

10. Do you need to access a private git repositories inside Docker?

    <pre><code>docker build -t my_docker . \
    -f Dockerfile \
    --build-arg GITHUB_TOKEN=TOKEN
    </code></pre>

    and multistage.

11. Use Multi-stage builds if possible, see [example](multi-stage/).

12. Try not to run your application as a <i>root</i> user.

13. Mountable secrets for building Docker Images (beta), see [example](secret-mount).

14. Others:

    - rebuild your base images / pull new ones
    - security scanner
    - caching, reusing previous docker images in CI/CD
    
## References

- https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
