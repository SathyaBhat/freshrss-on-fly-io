### Running FreshRSS on Fly.io

FreshRSS is a free, self-hostable RSS feeds aggregator. [Fly.io](https://fly.io/) is super amazing platform that runs application servers close to end users. 

Fly.io can take Docker images (or Dockerfiles, or buildpacks) and boot them into [Firecracker](https://aws.amazon.com/blogs/aws/firecracker-lightweight-virtualization-for-serverless-computing/) powered microVMs. When you deploy with Fly.io, you get an Anycast IP, along with TLS offloading by them.

These are the steps if you wish to run FreshRSS on your fly.io account

* Sign up for Fly.io
* Install [flyctl](https://fly.io/docs/hands-on/install-flyctl/)
* Sign into your Fly.io account by typing `flyctl auth login`
* Create a `fly.toml` file similar to the one in [the repo](/fly.toml). Make sure to update the appname to something else.
* Review the environment variables listed in the `[env]` section, and update if required. You can check [FreshRSS' docs](https://github.com/FreshRSS/FreshRSS/blob/edge/Docker/freshrss/example.env) on the environment variables that can be updated 
* Create a volume for persisting data using the command `fly volumes create freshrss_data --size 1`
    * In this command, we create a volume of size 1GB. This can be increased later, so I selected the lowest possible number, as flyctl expects size in GB and doesn't accept fractional numbers.
* Save the `fly.toml` and from the same directory where `fly.toml` is present, launch the application using `fly launch`. flyctl will pull the Docker image and launch the VM. The output should be similar to below: 

```bash

fly launch
An existing fly.toml file was found for app freshrss
App is not running, deploy...
Deploying freshrss
==> Validating app configuration
--> Validating app configuration done
Services
TCP 80/443 ⇢ 80
Searching for image 'freshrss/freshrss' remotely...
image found: img_lj9x4d7jkwe4wo1k
Image: registry-1.docker.io/freshrss/freshrss:latest
Image size: 75 MB
==> Creating release
Release v1 created

You can detach the terminal anytime without stopping the deployment
Monitoring Deployment

1 desired, 1 placed, 1 healthy, 0 unhealthy
--> v1 deployed successfully
```

See the full blog post on my [blog](https://sathyasays.com/2022/09/05/self-hosting-freshrss-fly-io-free/)