# Docker

- Publishing container ports is insecure by default. Meaning, when you publish a container's ports
it becomes available not only to the Docker host, but to the outside world as well.
  - If you include the localhost IP address (127.0.0.1, or ::1) with the publish flag, only the
  Docker host and its containers can access the published container port.
  - [Source](https://docs.docker.com/engine/network/#published-ports)

  ```sh
  docker run -p 127.0.0.1:8080:80 -p '[::1]:8080:80' nginx
  ```

- The default bridge network is considered insecure and not production-grade. As such, use a custom
network (`docker create network mynetwork`).
- It may be necessary to utilize the `--memory` and `--cpus` `docker run` flags, particularly when
running on a shared machine.

