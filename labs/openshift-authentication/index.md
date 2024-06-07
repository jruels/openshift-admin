# Configuring OpenShift Authentication

## Objective

In this lab, you will learn how to configure the OpenShift authentication operator to use an HTPasswd identity provider (IdP). You will install the necessary tools, create a test user file, create a secret, configure the OAuth server, and test the authentication.

Steps:

1. Install the `httpd-tools` package:
   - Connect to your OpenShift cluster using SSH.
   - Run the following command to install the `httpd-tools` package using `dnf`:
     ```bash
     sudo dnf install httpd-tools -y
     ```

2. Create a test user file:
   - Use the `htpasswd` utility to add a test user to the file:
     ```bash
     htpasswd -bBc /tmp/htpasswd testuser testpasswd
     ```
   
3. Create a secret from the `htpasswd` file:
   - Use the `oc` command to create a new secret in the `openshift-config` namespace:
     ```bash
     oc create secret generic htpass-secret --from-file=htpasswd=/tmp/htpasswd -n openshift-config
     ```
   - This command creates a secret named `htpass-secret` using the `htpasswd` file.

4. Configure the OAuth server:
   - Edit the cluster-wide OAuth/cluster object using the following command:
     ```bash
     oc edit oauth cluster
     ```
   - Locate the `spec` section in the YAML file.
   - Remove the `{}` and update it to match the following:
     ```yaml
     spec:
       identityProviders:
       - htpasswd:
           fileData:
             name: htpass-secret
         name: htpassidp
         type: HTPasswd
     ```
   - Save the changes and exit the editor.
   
5. Confirm the authentication operator restarts:
   - Run the following command to check the status of the authentication operator:
     ```bash
     oc get clusteroperator authentication
     ```
   - Wait until the operator `Available` field shows `True` before proceeding to the next step.

6. Test the authentication using the oc command-line tool:
   - Log in to the OpenShift cluster using the test user credentials:
     ```bash
     oc login -u testuser -p testpasswd
     ```
     
   - If the authentication is successful, you should see a message indicating `Login successful`.

7. Test the authentication using the OpenShift web console:
   - Open a web browser and navigate to the OpenShift web console URL.
   - Click on the `htpassidp` log-in button.
   - Enter the username and password of the test user you created earlier.
   - You should be successfully authenticated and redirected to the OpenShift web console.

Verification:
- Ensure that you can log in to the OpenShift cluster using the test user credentials through both the oc command-line tool and the web console.

- Run the following command to verify the OAuth configuration:
  ```bash
  oc get oauth cluster -o yaml
  ```
  
  Why do you get a permission denied error?
  
  ### Bonus step
  
  Log out as the `testuser` and log back in as `kubeadmin` and attempt to command again.
  
- Check the output and confirm that the `htpasswd_provider` is listed under the `identityProviders` section.

### Congratulations!

You have successfully configured the OpenShift authentication operator to use an `HTPasswd` identity provider. You created a test user file, created a Kubernetes secret, configured the OAuth server, and tested the authentication using both the `oc` command and the web console. With this setup, users can now log in to the OpenShift cluster using the credentials stored in the secret.

**Note:** In a production environment, it is recommended to use a more secure and scalable identity provider such as LDAP, Active Directory, or OpenID Connect. We will configure the LDAP IdP next. 



