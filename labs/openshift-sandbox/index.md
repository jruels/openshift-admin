# OpenShift Sandbox

## Prerequisites

## Red Hat Developer account

Go to the [Red Hat Developer portal](https://developers.redhat.com/about), click "Join now," and fill out the form. 

Provide the following: 

* Username 
* Email address 
* Job role 
* Password 

### Get started with OpenShift in the Developer Sandbox

Here are the steps for accessing an OpenShift instance through the Developer Sandbox for Red Hat OpenShift.

#### Log in to the Developer Sandbox

1. Use your web browser to navigate [here](https://developers.redhat.com/developer-sandbox) and select 

2. **Start your sandbox for free**

   [![The entry point for access to the Developer Sandbox for Red Hat OpenShift.](https://developers.redhat.com/sites/default/files/styles/article_floated/public/foundations_1.50_figure1.png?itok=MBFAp0nX)](https://developers.redhat.com/sites/default/files/foundations_1.50_figure1.png)

3. Select **Start your sandbox for free.** You will be directed to log into your Red Hat Developer account.

4. Provide your username and password for your Red Hat Developer account in the log in web page.

5. If you don’t have a Red Hat account, click the **Register** icon at the upper right of the web page. From there, you can register with Red Hat to create an account

6. Log into your Red Hat account to access the Developer Sandbox for Red Hat OpenShift.

   [![Log into your Red Hat account to access the Developer Sandbox for Red Hat OpenShift.](https://developers.redhat.com/sites/default/files/styles/article_floated/public/foundations_1.10_fig3.png?itok=4nvTPT8g)](https://developers.redhat.com/sites/default/files/foundations_1.10_fig3.png)

7. You can return to your sandbox later using the same URL listed above. After you log in, you are prompted to get started. A short description of how it works is included

8. Provision your developer sandbox instance.

   [![Alt text: The Hybrid Cloud Console is shown with the prompt to get started with developer sandbox and a description of how developer sandbox works. The user can click the get started button to begin.](https://developers.redhat.com/sites/default/files/styles/article_floated/public/foundations_1.10_fig4b.png?itok=OHOnVUaL)](https://developers.redhat.com/sites/default/files/foundations_1.10_fig4b.png)

9. Simply click **Get started** to begin the process of creating your own space in the Developer Sandbox. 

10. When you select **Get Started**, you will briefly see a spinning “working” icon, and then the screen will return. At this point, patience is key. Just wait.

11. Within a minute or two,  you will receive a prompt that your sandbox is ready. You will also receive an email noting that your sandbox has been provisioned.

    [![Prompt informing user that their developer sandbox is ready. They can click the Got it button to access their sandbox.](https://developers.redhat.com/sites/default/files/styles/article_floated/public/foundations_1.10_fig5b.png?itok=LRpUuqMG)](https://developers.redhat.com/sites/default/files/foundations_1.10_fig5b.png)

12. When you click the **Got it** button, you are presented with the Developer Sandbox **Available services** screen.

13. **Note:** In the future, when you want to return to the sandbox, this screen is where you’ll start.

14. Click the **Launch** button for Red Hat OpenShift and log into your sandbox.

    [![The Developer Sandbox web page is shown, listing the available services for the user. The services listed are: Red Hat OpenShift, Red Hat Dev Spaces, and Red Hat OpenShift AI. Each option has a button to launch the service.](https://developers.redhat.com/sites/default/files/styles/article_floated/public/foundations_1.10_fig6b.png?itok=z_UgwBxF)](https://developers.redhat.com/sites/default/files/foundations_1.10_fig6b.png)







### View the OpenShift web console

The first time you enter the Developer Sandbox for Red Hat OpenShift, you will be presented with the option to take an interactive tour of the various features available in the OpenShift web console. The option will appear the first time you log in.

To take the interactive tour, select **Get started**. To navigate directly to the main page of the web console in the sandbox, select **Skip tour**.

[![Options for the web console once you log in.](https://developers.redhat.com/sites/default/files/styles/article_floated/public/foundations_1.10_fig5.png?itok=6kiBU3XX)](https://developers.redhat.com/sites/default/files/foundations_1.10_fig5.png)



### Sandbox Quotas and Limits

Your private OpenShift environment on Developer Sandbox includes quotas and limits of:

- 7 GB RAM
- 15GB storage

which is enough to run these labs

There are two fixed projects (namespaces):

- `<your_username>-dev`
- `<your_username>-stage`

In this shared environment, creating a new project is impossible.

Please work in the `<your_username>-dev` project.



### Review

Congratulations! You’ve learned about the basic relationship between OpenShift and Kubernetes. You've also learned how to access the Developer Sandbox for Red Hat OpenShift and get hands-on experience working with OpenShift.

### Next

In the next section, you will receive an overview of the OpenShift web console and learn about the **Administrator** and **Developer** perspectives. Also, you will learn about the features you will use to create, configure, network, deploy, and maintain Kubernetes web applications running under OpenShift.



## Overview of the OpenShift web console

### View Administrator and Developer Perspectives

The OpenShift web console is designed to let administrators and developers perform the relevant tasks to their respective roles. Developers execute tasks relevant to installing and configuring applications and services, while administrators ensure that applications operating in a given cluster run efficiently and safely while paying attention to resource allocation and security administration. 

OpenShift separates developer tasks from administrator tasks in the web console by providing specific visual perspectives for each group. Developers see the **Developer** perspective, and administrators see the **Administrator** perspective.

The following steps show you how to access each perspective:

If you aren't logged into the sandbox, follow these steps.

1. Using your web browser, navigate [here](https://developers.redhat.com/developer-sandbox) and select **Start your sandbox for free**

2. [![The entry point for access to the Developer Sandbox for Red Hat OpenShift.](https://developers.redhat.com/sites/default/files/styles/article_floated/public/foundations_1.11_figure1.png?itok=ax8qNXus)](https://developers.redhat.com/sites/default/files/foundations_1.11_figure1.png)

   

3. When the sandbox login page appears, input the username and password for your Red Hat account.

4. Once you've logged in, a vertical menu bar will appear on the left side of the web console. At the top of the vertical menu bar, there's a drop-down menu that lets you select a viewing perspective.

5. To view the **Developer** perspective, select **Developer**.

6. To view the **Administrator** perspective, select **Administrator** from the drop-down.

[![The two perspectives available in the Developer Sandbox are Developer and Administrator.](https://developers.redhat.com/sites/default/files/styles/article_floated/public/foundations_1.11_figure2.png?itok=y50WV3o5)](https://developers.redhat.com/sites/default/files/foundations_1.11_figure2.png)



Once you select a perspective, the OpenShift web console reconfigures itself to present the features, services, and permissions relevant to the perspective.

For example, notice that the Administrator perspective shown in Figure 2 displays a **User Management** listing near the bottom of the menu bar while the Developer perspective does not. 



### Work with OpenShift projects

The most important concept to understand when working with OpenShift is the notion of projects. OpenShift organizes applications and services by projects, and an OpenShift cluster can have one or more projects.

A *project* is an OpenShift representation of a Kubernetes[ namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/). As pointed out in the previous section, an OpenShift cluster is an undercover Kubernetes cluster. The benefit is that OpenShift removes a lot of the complexities of working with Kubernetes in favor of a simplified user experience.

Applications and services running in a given Kubernetes namespace cannot typically see applications and services running in another namespace. A single namespace provides the isolation and security required to run an application and service in a cluster. However, managing multiple unique namespaces can be complex.

OpenShift removes the complexity by representing the namespace as a project. Once you declare an application or service to be part of a project, full-blown instances of OpenShift running either in a local or cloud setting within [Red Hat OpenShift Local](https://developers.redhat.com/products/openshift-local/overview) lets you create any number of projects.

Note: The OpenShift instance running within the Developer Sandbox for Red Hat OpenShift supports only a single OpenShift project and a single Kubernetes namespace.

We will be working with a cloud OpenShift cluster later in class.

The default single project is named according to the naming convention `<login_name>-dev`. Thus, if your log in name is joe123, the default project in your instance of OpenShift is the Developer Sandbox will be named `joe123-dev.`

You can view projects running in an OpenShift cluster from either the **Developer** or **Administrator** perspective. Access your available projects within the Developer perspective using the **Project** option on the menu bar along the left side, as shown by the left arrow below in Figure 4. The right arrow shows you how to access a list of projects in the Administrator perspective by selecting the **Project** tab under Home.

[![The Developer and Administrator perspectives have different permissions.](https://developers.redhat.com/sites/default/files/styles/article_floated/public/foundations_1.11_figure4.png?itok=cXWToayf)](https://developers.redhat.com/sites/default/files/foundations_1.11_figure4.png)



A list of projects appears on the Project page. Select a project in the list to view its project details. An Overview pane will appear. The Administrator perspective contains an expanded set of details relevant to a project's administration, such as resource utilization, configuration, and security settings.

[![The Administrator perspective shows an expanded set of details associated with a project, like resource utilization and security settings.](https://developers.redhat.com/sites/default/files/styles/article_floated/public/foundations_1.11_figure5.png?itok=ME1pHhUz)](https://developers.redhat.com/sites/default/files/foundations_1.11_figure5.png)



### Review

Congratulations! You’ve learned about the OpenShift web console in a high-level overview. You also learned about the Developer and Administrator perspectives in the web console UI and how to access each perspective.

You also learned what an OpenShift project is and how to access information about a project from both perspectives.

### Next

In the next section, you will learn how to use the terminal window built into the OpenShift console to work with various OpenShift and Kubernetes resources.





## OpenShift terminal

### Access the terminal window

To reach the OpenShift terminal window within your no-cost OpenShift Sandbox:

1. Click the terminal prompt icon in the upper right corner of the web page to display the command-line terminal. The terminal window allows you to interact directly with the underlying Kubernetes cluster, powering the OpenShift instance.

   [![The OpenShift web console can display a terminal window that enables direct interaction with the underlying Kubernetes cluster in the OpenShift instance](https://developers.redhat.com/sites/default/files/styles/article_floated/public/foundations1.12figure2.png?itok=zsANqoZE)](https://developers.redhat.com/sites/default/files/foundations1.12figure2.png)

   

### Work with the oc and kubectl CLI tools from within the terminal window

The terminal window that’s embedded within the OpenShift web console is a full-blown instance of a Linux terminal. You can do the same command-line activities you would normally run in a standalone terminal. Also, the OpenShift web console terminal window allows you to work easily with OpenShift and its underlying Kubernetes cluster.

OpenShift has both the `oc` and `kubectl` CLI tools pre-installed and configured to access the underlying Kubernetes cluster within the OpenShift instance. Thus, you can interact with OpenShift and the underlying Kubernetes cluster from the command line.

The `oc` CLI tool has much of the same functionality that `kubectl` provides. However, `oc` can go a bit further and allow you to execute behavior that is special to OpenShift. For example, you can use `oc` to install an application into OpenShift without having to negotiate the complexities of application deployment in Kubernetes.

The following exercises demonstrate how to use the `oc` and `kubectl` CLI tools to execute basic tasks from the terminal window’s command line. The exercises also demonstrate the close relationship between OpenShift and Kubernetes.

### Get version information

Run the following command in the OpenShift terminal to get version information about `oc` CLI tool:

```bash
oc version
```



You will see output similar to the following:

```
Client Version: 4.10.6

Server Version: 4.10.28

Kubernetes Version: v1.23.5+012e945
```



The results of the `oc` `version` command display the version numbers of the `oc` CLI client, the server powering the current OpenShift instance, and the underlying Kubernetes instance used by the cluster.

### View resources using oc or kubectl

Run the following command in the OpenShift terminal to get a list of pods running in the current instance of OpenShift using the `oc`CLI tool: 

```bash
oc get pods
```



You will receive output similar to the following:

```
NAME                                        READY   STATUS    RESTARTS   AGE

workspace1e8db31dd59c43ef-8585ff4cc-24ww6   2/2     Running   0          22s
```



Run the following command in the OpenShift terminal to get a list of pods running in the current instance of OpenShift using the `kubectl` CLI tool: 

```bash
kubectl get pods
```



You will receive output similar to the following:

```
NAME                                        READY   STATUS    RESTARTS   AGE

workspace1e8db31dd59c43ef-8585ff4cc-24ww6   2/2     Running   0          22s
```



Notice that the output from executing `oc` get pods and `kubectl` get pods is identical. This is because both `oc` and `kubectl` are querying the same underlying Kubernetes cluster to get a list of pods. This is direct evidence of the tight relationship between OpenShift and Kubernetes. 

Remember, OpenShift is a layer of components that “sit on top” of an underlying Kubernetes cluster. The important thing to understand is that the `oc` CLI tool provides functionality available in the `kubectl` CLI tool. In many cases, you can use `oc` and `kubectl` interchangeably.

### Review

In this section, you learned how to access the terminal window that’s embedded in the OpenShift web console. Also, you learned how to use the terminal window to execute commands using the `oc` and `kubectl` CLI tools. You executed both `oc get pods` and `kubectl get pods` to list the same set of pods running in the Kubernetes cluster that’s foundational to OpenShift.

### Next

This is the last of a set of labs intended to introduce you to OpenShift, the OpenShift web console, and the `oc` CLI tool for OpenShift. The labs that follow will demonstrate how to use the OpenShift web console to install, configure, and maintain applications from source code or container image. Also, a set of labs will demonstrate how to install and maintain applications from source code or a container image using the `oc` CLI tool at the command line.













