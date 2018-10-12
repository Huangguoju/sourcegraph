# Install Sourcegraph with Docker

## Prerequisites

[Docker](https://docs.docker.com/engine/installation/) is required.

## Step 1: Run Sourcegraph

<div id="docker-command-docs">

<pre class="pre-wrap"><code>docker run<span class="virtual-br"></span> --publish 7080:7080 --rm<span class="virtual-br"></span> --volume ~/.sourcegraph/config:/etc/sourcegraph<span class="virtual-br"></span> --volume ~/.sourcegraph/data:/var/opt/sourcegraph<span class="virtual-br"></span> --volume /var/run/docker.sock:/var/run/docker.sock <span class="virtual-br"></span>sourcegraph/server:<server-version-number /><span class="virtual-br"></span></code></pre>

</div>

When Sourcegraph is ready, continue at http://localhost:7080.

## Step 2: Add repositories

After creating an account, go to the **Configuration** page in the site admin area.

Click **Add GitHub.com repositories** to add all repositories associated with your GitHub.com account, or see [how to add repositories from other code hosts](/docs/config/repositories).

## Step 3: Start searching your code

**Done!** You're ready to search your code.

## Next steps

- [Configure your Sourcegraph instance](/docs/config)
- [Configure code intelligence](/docs/code-intelligence)
- [Deploy Sourcegraph on AWS](/docs/deploy/aws)
- [Deploy Sourcegraph on Google Cloud Platform](/docs/deploy/gcp)
- [Deploy Sourcegraph on Digital Ocean](/docs/deploy/digitalocean)

Questions/feedback? Contact us at [@srcgraph](https://twitter.com/srcgraph) or <mailto:support@sourcegraph.com>, or file issues on our [public issue tracker](https://github.com/sourcegraph/issues/issues).

---


## File system performance on Docker for Mac

There is a [known issue](https://github.com/docker/for-mac/issues/77) in Docker for Mac that causes slower than expected file system performance on volume mounts, which impacts the performace of search and cloning.

To achieve better performance, you can do any of the following:

- For better clone performance, clone the repository on your host machine and then [add it to Sourcegraph Server](/docs/config/repositories#add-repositories-already-cloned-to-disk).
- Try adding the `:delegated` suffix the data volume mount. [Learn more](https://github.com/docker/for-mac/issues/1592).
  ```
  --volume ~/.sourcegraph/data:/var/opt/sourcegraph:delegated
  ```
- Run Sourcegraph Server on Linux, or use the [Data Center](https://github.com/sourcegraph/deploy-sourcegraph) deployment option for even larger scale on Kubernetes.