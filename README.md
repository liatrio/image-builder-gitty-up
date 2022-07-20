## What is the image for?
The intended purpose of this image is for it to be used as a Jenkins agent. By using the installed features the user is able to create Jenkins pipelines that can handle GitOps operations using [Gitty-Up](https://github.com/liatrio/gitty-up). An example of using this image as a Jenkins agent via [Kubernetes](https://plugins.jenkins.io/kubernetes/) can be seen below. 

First, an example of configuring the pod template in yaml to create the agent.

```yaml
jenkins:
  clouds:
    - kubernetes:
        name: "kubernetes"
        templates:
          - name: "image-builder-gitty-up"
            label: "image-builder-gitty-up"
            nodeUsageMode: NORMAL
            containers:
              - name: "image-gitty-up"
                image: "ghcr.io/liatrio/image-builder-gitty-up:${builder_images_version}"
```
And then specifying the agent in the Jenkinsfile for an example step.

```jenkins
stage('Build') {
  agent {
    label "image-builder-gitty-up"
  }
  steps {
    container('image-gitty-up') {
      sh "gitty-up -gitUrl=https://github.com/my-org/my-repo -gitUsername=$USERNAME -gitPassword=$PASSWORD -repoFile=testing/application.json --values=main.version=v0.0.42"
    }
  }
}
```

## What is installed on this image?
- Version [0.1.5](https://github.com/liatrio/gitty-up/releases/download/v0.1.5/gitty-up_0.1.5_Linux_x86_64.tar.gz) of GittyUp, a tool to automate updating manifests files in GitOps repositories
