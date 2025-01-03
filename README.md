# oracle-k8s
solving lets encrypt on oracle k8s

## purpose
getting a nice kuberenetes set up on oracle cloud

## process
here are my conversations with claude on the topic - sometimes my prompts are summarized



I'll help you create a simple webpage for testing SSL with Oracle Cloud Kubernetes. Let's start with a minimal but cute web application.



Now, let's create a simple Kubernetes deployment file for this webpage. I'll create a new artifact for the Kubernetes manifests.

Now, let's create a GitHub workflow file to deploy this to Oracle Kubernetes Engine (OKE).

To get started with this setup, you'll need to:

1. Create a new GitHub repository
2. Create the following directory structure:
```
.
├── .github
│   └── workflows
│       └── deploy.yml    # GitHub workflow file
├── k8s
│   └── deployment.yml    # Kubernetes manifests
└── src
    └── index.html        # Web page
```

3. Set up the following GitHub secrets:
- OCI_CONFIG: Your OCI CLI config file content
- OCI_KEY: Your OCI API private key
- OKE_CLUSTER_ID: Your Oracle Kubernetes Engine cluster OCID
- OCI_REGION: Your OCI region

Would you like me to help you with any specific part of this setup? Once we verify the initial deployment works, we can move on to setting up Let's Encrypt SSL.

Maybe better with Docker

Ah yes, absolutely! We can create a Docker image of the webpage instead, push it to Oracle Cloud Infrastructure Registry (OCIR), and have the Kubernetes deployment pull from there. This is a much better practice. Let me revise the approach.

Now let's update the Kubernetes manifest to use the container image instead of a ConfigMap:

And here's the updated GitHub workflow that builds and pushes the Docker image:

Now you'll need these additional secrets in your GitHub repository:
- OCIR_REPO: Your full OCIR repository path (e.g., iad.ocir.io/yourtenancy/yourrepo)
- OCIR_REGISTRY: Your OCIR registry endpoint (e.g., iad.ocir.io)
- OCIR_USERNAME: Your OCIR username (format: tenancy/oracleidentitycloudservice/email)
- OCIR_TOKEN: Your auth token for OCIR

The directory structure would be:
```
.
├── .github
│   └── workflows
│       └── deploy.yml    # GitHub workflow file
├── k8s
│   └── deployment.yml    # Kubernetes manifests
├── src
│   └── index.html       # Web page
└── Dockerfile
```

This approach is much better because:
1. Every change to the webpage triggers a new Docker build
2. Each version is tagged with the Git commit SHA
3. You get proper versioning and rollback capability
4. The image is stored in OCIR, which is more secure and faster to pull
5. You can test the image locally before deploying

Would you like me to explain any part of this setup in more detail?

Maybe using configs and key

Ah yes, this is a cleaner approach using GitHub Container Registry instead of OCIR! Let me update our workflow to match your existing setup.

Let's also update the Kubernetes manifest to match this setup:

Now the setup will use your existing secrets:
- CR_PAT: For GitHub Container Registry authentication
- KUBE_CONFIG: Your kubeconfig file for the Oracle Cloud Kubernetes cluster
- OCI_CONFIG: Your OCI configuration
- OCI_PRIVATE_KEY: Your OCI private key

The directory structure remains the same:
```
.
├── .github
│   └── workflows
│       └── deploy.yml    # GitHub workflow file
├── k8s
│   └── deployment.yml    # Kubernetes manifests
├── src
│   └── index.html       # Web page
└── Dockerfile
```

This approach:
1. Uses GitHub Container Registry instead of OCIR
2. Matches your existing authentication setup
3. Includes multi-arch builds (amd64 and arm64)
4. Uses GitHub's cache for faster builds
5. Has the same debugging steps as your existing workflow
6. Uses GitHub's metadata action for proper tagging

Would you like me to explain any part of this setup or shall we move forward with testing the deployment?


