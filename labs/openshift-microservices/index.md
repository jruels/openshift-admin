# Deploy Microservices in OpenShift

### How to deploy full-stack JavaScript microservices in OpenShift

OpenShift is a Kubernetes distribution that makes deploying and scaling applications in the cloud easy. In this hands-on lab, you will learn how to deploy a full-stack JavaScript application in an OpenShift cluster.

This lab consists of two parts:

- Deploy the front-end container to OpenShift.

- Deploy and connect the back end to the front end using environment variables, then add a health check.

  

### Deploy the front-end image

Deploy an image hosted at Docker with the latest version of the URL shortener front end. Note that the following command does not include the registry hostname, `docker.io`. This is because if you don't specify a registry host, the default value of docker.io is used.

```bash
oc new-app nodeshift/urlshortener-front 
```

OpenShift downloads the image from your registry and creates a new application with it.



### Expose the application

The front end has been deployed to your cluster, but no one can connect to it yet from outside of the cluster. You need to expose the service created with a route to enable access.

Enter the following command:

```bash
$ oc expose svc/urlshortener-front --port=8080 

```

Your application is now fully deployed to OpenShift, and you should be able to see it in the OpenShift Topology view of your application.

[![Your application is visible in OpenShift's topology view.](https://developers.redhat.com/sites/default/files/styles/article_full_width_1440px_w/public/topology_1.png?itok=e-TEQVPg)](https://developers.redhat.com/sites/default/files/topology_1.png)



### Verify the application status

If you click on the **Open URL** button next to your application in the Topology view, it should open your front-end application.

You can view the list of redirection URLs (currently empty) from here. You can also see the form to add new URLs (currently nonworking). Finally, a third link in the navigation bar leads to an "About" page. You can see the status of the Node.js API, the Mongo database, and the redirector service, as shown below.

[![The About page displays information about the components of the application.](https://developers.redhat.com/sites/default/files/styles/article_full_width_1440px_w/public/status_1.png?itok=Y7JhwCCZ)](https://developers.redhat.com/sites/default/files/status_1.png)



All the indicators are currently red because the front end doesn't have a back end to talk to. We'll address this issue in the second part of the lab.





### Deploy the Node.js back end

Now it's time to deploy the back end and connect it to the front end using environment variables, then add a health check.

In this lab's first part, the front-end container was deployed to OpenShift.

OpenShift has several built-in options for application deployment. You will build and deploy a Node.js back end using OpenShift's Source-to-Image) (S2I) toolkit.

### Build the Node.js back end using S2I

Source-to-Image is a toolkit for building containers directly from the source code. To build the Node.js back end from the GitHub source code, you can use the `oc new-app` command you used in Part 1. This time, you must specify the GitHub repository where the source code is located; OpenShift will automatically pick the correct build image (Node.js 18 at this time). The `--context-dir` parameter specifies that the source code is located in the `/back` folder:

```
oc new-app https://github.com/nodeshift-blog-examples/urlshortener --context-dir=back 
```

You should get a message back indicating that a build has started:

```
--> Found image dfd08e2 (2 months old) in image stream "openshift/nodejs" under tag "16-ubi8" for "nodejs"

    Node.js 16
    ----------
    Node.js 16 available as container is a base platform for building and running various Node.js 16 applications and frameworks. Node.js is a platform built on Chrome's JavaScript runtime for easily building fast, scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.

    Tags: builder, nodejs, nodejs16

    * The source repository appears to match: nodejs
    * A source build using source code from https://github.com/nodeshift-blog-examples/urlshortener will be created
      * The resulting image will be pushed to image stream tag "urlshortener:latest"
      * Use 'oc start-build' to trigger a new build

--> Creating resources ...
    imagestream.image.openshift.io "urlshortener" created
    buildconfig.build.openshift.io "urlshortener" created
    deployment.apps "urlshortener" created
    service "urlshortener" created
--> Success
    Build scheduled, use 'oc logs -f buildconfig/urlshortener' to track its progress.
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose service/urlshortener'
    Run 'oc status' to view your app.
```

If you head to the **Topology** view in the OpenShift console, you can see that the application is displayed with a white ring around it. This ring indicates that the application is currently being built. The source code is cloned, and the image is built directly on the OpenShift cluster.

 

[![If the application is displayed with a white ring around it, the application image is being built on the OpenShift cluster.](https://developers.redhat.com/sites/default/files/styles/article_floated/public/learn_kubernetes_learning_path_figure_6.png?itok=yMGrCcgR)](https://developers.redhat.com/sites/default/files/learn_kubernetes_learning_path_figure_6.png)

In a few minutes, the ring should turn blue, indicating that the image was successfully built.

### Configure the environment variables

For the purpose of this lab, you'll use environment variables to indicate parameters in the back-end application. You might want to change some of the environmente production server variables in th.

We'll focus here on the back-end application's network port. The Node.js application was running on port 3001 in the development environment, which was set as an environment variable. In this case, you will change the port to 8080.

Click on the **urlshortener** circle denoting your application, and a side panel will open. In this panel, find the **Actions** menu in the top right corner and select **Edit Deployment**.

 

[![To edit the application deployment in the OpenShift console, go to the Actions menu in the top right corner and select Edit Deployment.](https://developers.redhat.com/sites/default/files/styles/article_floated/public/learn_kubernetes_learning_path_figure_7.png?itok=_5kGGSYE)](https://developers.redhat.com/sites/default/files/learn_kubernetes_learning_path_figure_7.png)

A form-based deployment editor is then displayed, where you can see the description of the URL shortener application deployment. Scroll down the page and find the **Environment** section. Add a single variable with the name PORT and the value 8080, as shown below.

 

[![To change the application's port number, locate the Environment section and add PORT 8080.](https://developers.redhat.com/sites/default/files/styles/article_floated/public/learn_kubernetes_learning_path_figure_8.png?itok=ic0kRry0)](https://developers.redhat.com/sites/default/files/learn_kubernetes_learning_path_figure_8.png)



Click **Save** and go back to the OpenShift Topology view. If you go back fast enough, you might see a double ring around the URL shortener application. This is because OpenShift is currently deploying a new version of the application with the new environment variables. Once this new version is up and running, OpenShift takes the old version down. This process ensures that there is zero downtime when you update your applications.

 

[![A double circle around the application icon shows two application versions co-existing temporarily.](https://developers.redhat.com/sites/default/files/styles/article_floated/public/learn_kubernetes_learning_path_figure_9.png?itok=Hya68OXB)](https://developers.redhat.com/sites/default/files/learn_kubernetes_learning_path_figure_9.png)



#### Expose the application

Now that you've deployed the application's back end, you can expose it using the same command you used for the front end:

```bash
oc expose svc/urlshortener 
```

There is no need to specify the port in this case because S2I assumed that the application would be running on port 8080.

If you click on the **Open URL** link, you should see a response back from the server:

```
{ "msg": "Hello" }
```

You can also try the `/health` route, which should return the server and database status:

```
{ "server": true, "database": false } 
```

You can see the code for this `/health` route below.



```javascript
 console.log("Testing database connection");
  await client.connect().then(() => {
    console.log("Connected");
    dbStatus = true;
  }).catch(e => {
    console.log("Failed to connect");
```



### Add a health check

OpenShift can periodically check your pod to see whether it is still running. This process is called a *health check*. In the side panel, when you clicked on the **urlshortener** deployment, you might have noticed a message recommending that you add health checks. Go ahead and click on **Add Health Checks**.

 

[![In the side panel, click on Add Health Checks to periodically check your pod to see whether it is still running.](https://developers.redhat.com/sites/default/files/styles/article_floated/public/learn_kubernetes_learning_path_figure_10.png?itok=Zs-Hx7ww)](https://developers.redhat.com/sites/default/files/learn_kubernetes_learning_path_figure_10.png)



From the next screen, you can add a liveness probe, a health check that monitors your application by periodically calling the specified route. As long as the route returns a 200 HTTP code, OpenShift assumes that the application is still running.

To add this health check, click **Add Liveness Probe**. Change the path to `/health` and keep all the other default values.

To save the health check, click the checkmark at the bottom of the dashed area and then the blue Add button.

To validate that the probe is working, visit the **Resources** tab from the deployment side panel and click **View Logs** next to the pod name. This screen shows you the pod logs; you should see the request to `/health` every 10 seconds.

### Link the front end and back end

Now that the back end is up and running, we need to adda value for the `$BASE_URL` environment variable.



### Add the BASE_URL environment variable

You can add environment variables using the `oc` CLI tool, but before you do that, you need to find the `BASE_URL` that the front end needs to connect to. This base URL is the route to your back end. You can find this route by running the `get route` command:

```
oc get route urlshortener

NAME           HOST/PORT                                                         PATH   SERVICES       PORT       TERMINATION   WILDCARD
urlshortener   urlshortener-joelphy-dev.apps.sandbox.x8i5.p1.openshiftapps.com          urlshortener   8080-tcp                 None
```

If you want to get only the actual route:

##### Bash:

You can use `awk` to extract it:

```bash
oc get route urlshortener | awk 'NR>1 {print $2}' 
```



##### Bash:

```bash
oc set env deployment/urlshortener-front BASE_URL=http://$(oc get route urlshortener | awk 'NR>1 {print $2}')
deployment.apps/urlshortener-front updated
```

Now that you have configured the `BASE_URL` environment variable, you can go back to the URL shortener application and check its About page again. Figure 6 shows the current service status on the About page.

 

[![The About page shows the service status is "Up and running."](https://developers.redhat.com/sites/default/files/styles/article_floated/public/learn_kubernetes_learning_path_figure_11.png?itok=emWIzIM-)](https://developers.redhat.com/sites/default/files/learn_kubernetes_learning_path_figure_11.png)



You can see that the server is now up and running.

### Conclusion to Lesson 2

The database and redirector services are still unreachable because you have not deployed them. You will add those in upcoming labs.

