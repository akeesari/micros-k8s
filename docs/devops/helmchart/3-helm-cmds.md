Here are some of the most frequently used Helm commands and their details:

## Helm lint

`Helm lint` is like your C# source code compilation. It is always recommended to run `helm lint` before installing helm chart, `Helm Lint` is a tool that checks the syntax and structure of a Helm chart and ensures that it follows best practices and conventions

Helm Lint helps you catch errors and issues with your Helm charts before you deploy them to a Kubernetes cluster, which can save you time and avoid potential problems down the line.

When you run helm lint on a Helm chart, it checks for the following:

- Valid YAML syntax
- Valid Helm chart structure
- Properly formatted values files
- Best practices for Helm chart development, such as using templates, labels, and annotations correctly

Helm Lint can be run on a Helm chart directory like this:

``` sh
helm lint 'microservices-chart'
```

If Helm Lint finds any issues with the Helm chart, it will output them to the console along with suggestions for how to fix them.

example of output with error
``` sh
==> Linting microservices-chart
[INFO] Chart.yaml: icon is recommended
[ERROR] templates/: parse error at (microservices-chart/templates/aspnet-api/deployment.yaml:7): bad character U+002D '-'

Error: 1 chart(s) linted, 1 chart(s) failed
```

Read the error details and fix the YAML manifest accordingly before installing Helm chart, you will not be able to install Helm with errors in the Chart.

example of output without any error
``` sh
==> Linting microservices-chart
[INFO] Chart.yaml: icon is recommended

1 chart(s) linted, 0 chart(s) failed
```


## Helm template
After the `helm lint`, it is also recommended to generate the yaml manifest from helm chart and review the yaml manifest to make sure all the values are replaced correctly or not, for that we are going to use `helm template` command.

`helm template` command does not install the chart or make any changes to the Kubernetes cluster. It simply generates YAML manifests based on the chart and the values provided. This can be useful for reviewing the manifests before applying them to a Kubernetes cluster.

``` sh
helm template 'microservices-chart' './microservices-chart' --namespace='sample' > microservices-manifests.yaml
```

!!! Note
    you will notice new file `microservices-manifests.yaml` created in your folder for your review, after the reveiw you can delete this file so that you don't commit this file in your git repo.

generate with environment values.yaml file

``` sh
helm template 'microservices-chart' './microservices-chart' --namespace='sample' > microservices-manifests.yaml
helm template 'microservices-chart' './microservices-chart' --namespace='sample' --values 'microservices-chart/values-dev.yaml' > microservices-manifests.yaml
```


## --dry-run

As part of the `helm install` command you can use the --dry-run flag.

The `--dry-run` flag is used to perform a simulated installation of a chart using the helm install command without actually installing the chart or creating a new release. When this flag is used, Helm will generate the Kubernetes manifest files that would be applied to the cluster during the installation process, but it will not actually make any changes to the cluster.

The purpose of performing a dry run is to preview the changes that would be made to the cluster by the installation process before actually applying those changes. This can be useful for testing and debugging purposes, as well as for verifying that the desired configuration settings are being applied correctly.

By using the --dry-run flag, you can identify any potential issues or conflicts that may arise during the installation process before making any changes to the cluster. This can help you to avoid making unintended changes or causing downtime for your applications.

``` sh
helm install 'microservices-release' './microservices-chart' --namespace 'sample' --dry-run
helm install 'microservices-release' './microservices-chart' --namespace 'sample' --values 'microservices-chart/values-dev.yaml' --dry-run
```

output

``` yaml
NAME: microservices-release
LAST DEPLOYED: Tue Mar 14 17:16:05 2023
NAMESPACE: sample
STATUS: pending-install
REVISION: 1
TEST SUITE: None
HOOKS:
MANIFEST:
---
# Source: microservices-chart/templates/aspnet-api/service.yaml
apiVersion: v1
kind: Service
metadata:
  name: aspnet-api
  namespace: sample
  labels: {}
spec:
  type: ClusterIP
  ports:
    - port: 80
      targetPort: 80
      protocol: TCP
      name: http
  selector:
    app: aspnet-api
---
# Source: microservices-chart/templates/aspnet-api/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aspnet-api
  namespace: sample
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aspnet-api
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
  minReadySeconds: 5
  template:
    metadata:
      labels:
        app: aspnet-api
    spec:
      nodeSelector:
        "kubernetes.io/os": linux
      serviceAccountName: default
      containers:
        - name: aspnet-api-image
          image: acr1dev.azurecr.io/sample/aspnet-api:20230302.5
          imagePullPolicy: Always
          ports:
            - name: http
              containerPort: 80
              protocol: TCP
          env:
            - name: KEY
              value: value
```

## Packaging the Helm chart

Use the helm package command to create the chart package. This command takes the name of the chart directory as its argument and creates a compressed archive file with the extension .tgz.

```
helm package './microservices-chart'
```
This will create a file named microservices-chart-<version>.tgz in the current directory.

output
``` sh
Successfully packaged chart and saved it to: C:\Source\Repos\microservices\helmcharts\microservices-chart-0.1.0.tgz
```

You can use the --version flag to specify the version number for the chart package. 
``` sh
helm package --version 1.0.0 './microservices-chart'
```

Once you have created a Helm chart package, you can share it with others by hosting it in a public or private chart repository. Other users can then install and manage your application or service in their Kubernetes clusters using the helm install command and the name of your chart package.

## Rollback helm chart


If you encounter any issues during the upgrade process, you can roll back the release to the previous version using the helm rollback command. For example, to roll back the my-release release to version 1, run the following command:

first list helm releases

``` sh
helm ls -n sample
helm history 'microservices-release' -n 'sample'
```
output
```
REVISION        UPDATED                         STATUS          CHART                           APP VERSION     DESCRIPTION     
1               Tue Mar 14 22:31:08 2023        superseded      microservices-chart-0.1.0       1.16.0          Install complete
2               Tue Mar 14 22:31:22 2023        deployed        microservices-chart-0.1.0       1.16.0          Upgrade complete
```

helm rollback

``` sh
helm rollback 'microservices-release' 1 -n 'sample'
```
output
```
Rollback was a success! Happy Helming!
```
check the history again
``` sh
helm history 'microservices-release' -n 'sample'
```
output
```
REVISION        UPDATED                         STATUS          CHART                           APP VERSION     DESCRIPTION     
1               Tue Mar 14 22:31:08 2023        superseded      microservices-chart-0.1.0       1.16.0          Install complete
2               Tue Mar 14 22:31:22 2023        superseded      microservices-chart-0.1.0       1.16.0          Upgrade complete
3               Tue Mar 14 22:32:10 2023        deployed        microservices-chart-0.1.0       1.16.0          Rollback to 1
```

This will revert the release to the previous version and restore the previous configuration.

## helm chart history

The `helm history` command is used to view the revision history of a Helm release, which includes information about the dates, times, and details of previous upgrades or rollbacks of the release. This command is useful for tracking the lifecycle of a release, understanding the changes made to a release over time, and diagnosing issues that may have occurred during previous upgrades or rollbacks.

``` sh
helm history 'microservices-release' -n 'sample'
```
output
```
REVISION        UPDATED                         STATUS          CHART                           APP VERSION     DESCRIPTION     
1               Tue Mar 14 22:31:08 2023        superseded      microservices-chart-0.1.0       1.16.0          Install complete
2               Tue Mar 14 22:31:22 2023        superseded      microservices-chart-0.1.0       1.16.0          Upgrade complete
3               Tue Mar 14 22:32:10 2023        deployed        microservices-chart-0.1.0       1.16.0          Rollback to 1
```

By default, the helm history command displays the last five revisions of the release. However, you can specify a different number of revisions to display using the --max flag. For example, to display the last ten revisions of a release, you can run the following command:

``` sh
helm history 'microservices-release' -n 'sample' --max=10
```
output
```
REVISION        UPDATED                         STATUS          CHART                           APP VERSION     DESCRIPTION     
1               Tue Mar 14 22:31:08 2023        superseded      microservices-chart-0.1.0       1.16.0          Install complete
2               Tue Mar 14 22:31:22 2023        superseded      microservices-chart-0.1.0       1.16.0          Upgrade complete
3               Tue Mar 14 22:32:10 2023        superseded      microservices-chart-0.1.0       1.16.0          Rollback to 1
4               Tue Mar 14 22:37:02 2023        superseded      microservices-chart-0.1.0       1.16.0          Upgrade complete
5               Tue Mar 14 22:37:09 2023        superseded      microservices-chart-0.1.0       1.16.0          Upgrade complete
6               Tue Mar 14 22:37:14 2023        deployed        microservices-chart-0.1.0       1.16.0          Upgrade complete
```
## Helm Cheat Sheet

For more information about Helm commands look into the [Helm Cheat Sheet](../../miscellaneous/helm-cheat-sheet.md).