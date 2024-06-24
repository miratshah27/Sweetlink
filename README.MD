<h1 align="center">Sweetlink: A distributed url shortening solution 🔗</h1><br/>

## 📚 | Introduction

- Sweetlink is a distributed and highly available URL Shortening solution, built on the MERN stack.
- It leverages Redis for caching and MongoDB as a NoSQL database.
- Nginx is utilized as a load balancer and reverse proxy for backend servers.
- Apache ZooKeeper provides token generation for hashing and resolves race conditions between nodes.
- The entire application is containerized with Docker and orchestrated using Kubernetes.

### _**Disclaimer**_

- This demo application showcases a URL shortener, designed with efficient system design principles.
- This is a project demonstrating the entire development process and is meant to be run in a local environment.

<br/>

## 🚀 | Usage

- Install Docker Desktop and enable Kubernetes for a quick setup.
- Clone this repository:<br>

```sh
git clone https://github.com/miratshah27/Sweetlink.git
```

- Create a .env file with the following variables:

```js
REACT_APP_MONGODB_URI="<enter your MongoDB URI>"
REACT_APP_REDIS_PORT=1111 (defaults to 6379)
REACT_APP_REDIS_HOST='redis-server' (name it as you like, but change the same in the docker-compose.yml file)
```

- Open the project folder and start the container with docker compose:<br>

```yml
docker compose up --build

# We can also scale instances of an image for higher scalability or distribution.
Example: docker compose up --build --scale node-server=3
```
<br/>

## 🌐 | API Endpoints

```yml
GET:
    /url/:identifier : Get the shortened URL from DB
    /del : Delete the Zookeeper token
POST:
    /url [body : {"OriginalUrl" : "url"}] : Shorten the URL and store in DB


Access the API using the following URLs:
    Client: http://localhost:3000/
    Load Balanced Server: http://localhost:4000/
```

<br/>

## 📘 | System Design Schematic

<p align = center>
    <img alt="getURL" src="https://raw.githubusercontent.com/miratshah27/Sweetlink/master/client/src/assets/images/getURLs.png" target="_blank" />
    <img alt="redirect" src="https://raw.githubusercontent.com/miratshah27/Sweetlink/master/client/src/assets/images/redirect.png" target="_blank" />
</p>

<br/>

## ⌛ | Architectural Discussion
- _**Availability**_: Improved by using multiple ZooKeeper instances, database replicas, and a distributed cache to enhance fault tolerance.
- _**Performance**_: Load balancers between components (client-server, server-database, server-cache) improve performance and distribute load.
- _**CAP Theorem**_:
  - Opt for an _**eventually consistent approach**_ for low latency and high throughput, crucial for URL redirection. <br/>
  - Redirection of the user to the original URL should always have low latency as it directly impacts the business aspect of the application.
  - Here, I don't opt for a _**strongly consistent approach**_, as it would have to wait for the data to be replicated across the cluster, which decreases the availability and increases the latency of the application, thus impacting the user experience negatively.

<br/>

## 💻 | References

- [TinyURL System Design](https://www.codekarle.com/system-design/TinyUrl-system-design.html)
- [System Design : Scalable URL shortener service like TinyURL](https://medium.com/@sandeep4.verma/system-design-scalable-url-shortener-service-like-tinyurl-106f30f23a82)
- [An Illustrated Proof of the CAP Theorem](https://mwhittaker.github.io/blog/an_illustrated_proof_of_the_cap_theorem/)
- [What is eventual consistency and why should you care about it?](https://www.keboola.com/blog/eventual-consistency)
- [Redis Documentation](https://redis.io/documentation)
- [Apache Zookeeper Documentation](https://zookeeper.apache.org/doc/r3.7.0/index.html)
- [Nginx HTTP Load Balancing Documentation](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/)
- [Docker Documentation](https://docs.docker.com/language/nodejs/)
- [Kompose Documentation](https://kompose.io/user-guide/)

<br/>