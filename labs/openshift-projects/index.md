# Managing Projects in OpenShift

# Projects

Projects are a top-level concept that helps you organize your deployments. An OpenShift project allows a community of users (or a user) to organize and manage their content in isolation from other communities. Each project has its own resources, policies (who can or cannot perform actions), and constraints (quotas and limits on resources, etc.). Projects act as a "wrapper" around all the application services and endpoints you (or your teams) are using for your work.

During this lab, we are going to use a few different commands to make sure that things in the environment are working as expected. Don’t worry if you don’t understand all of the terminology as we will cover it in detail in later labs.

## Create your first Project

Create a project named **<first_last-dev>**. From the **Administrator's** perspective, from the left-side menu, click on **Projects**, then from the top-right, click **Create Project**.

From the form, insert the name of your project **<first_last-dev>** and click on **Create**.

![Create Project](https://redhat-scholars.github.io/openshift-starter-guides/rhs-openshift-starter-guides/4.11/_images/prerequisites_create_project.png)

If you have multiple projects, the first thing you want to do is switch to the newly created project for future steps.

![Explore Project](https://redhat-scholars.github.io/openshift-starter-guides/rhs-openshift-starter-guides/4.11/_images/explore-webconsole2.png)

At the top of the left navigation menu, you can toggle between the **Administrator** perspective and the **Developer** perspective.

![Toggle Between Perspectives](https://redhat-scholars.github.io/openshift-starter-guides/rhs-openshift-starter-guides/4.11/_images/explore-perspective-toggle.png)

Select **Developer** to switch to the Developer perspective. Once the Developer perspective loads, you should be in the **+Add** view.

![Add New Applications](https://redhat-scholars.github.io/openshift-starter-guides/rhs-openshift-starter-guides/4.11/_images/explore-add-application.png)

Right now, there are no applications or components to view, but once you begin working on the lab, you’ll be able to visualize and interact with the components in your application in the **Topology** view.

You can also create and manage projects from the CLI as well. Let’s use our newly created project as our default one:

```bash
oc project <first_last>
```

You should an output similar to the following:

```
Now using project "dev" on server "https://api.trainer.innovtrain.org:6443"
```

We will be using a mix of command line tooling and the web console for the labs. Get ready!



