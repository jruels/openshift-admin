# OpenShift LDAP IdP

## Overview

In this lab you will install LDP on the jump host, configure it, add an OU, groups, and users. After completing the setup of LDAP you will configure the OpenShift authentication operator to connect to the new LDAP server.

### LDAP

#### Install and configure LDAP

Run the following commands on the jump host to install and configure LDAP

```bash
sudo yum install -y openldap openldap-servers openldap-clients
```



Start and enable the `slapd` service

```bash
sudo systemctl start slapd
sudo systemctl enable slapd
```



Generate an encrypted password for the admin user.

```bash
sudo slappasswd
```

Type in the string `password` twice



Now configure OpenLDAP to use your new password. Create `base.ldif` with the following

```ldif
dn: olcDatabase={2}mdb,cn=config
changetype: modify
replace: olcSuffix
olcSuffix: dc=innovteach,dc=com

dn: olcDatabase={2}mdb,cn=config
changetype: modify
replace: olcRootDN
olcRootDN: cn=admin,dc=innovteach,dc=com

dn: olcDatabase={2}mdb,cn=config
changetype: modify
replace: olcRootPW
olcRootPW: {SSHA}_YOUR_PASSWORD_FROM_SLAPPASSWD
```



Apply the configuration

```bash
sudo ldapmodify -Y External -H ldapi:/// -f base.ldif
```



Create the domain with `domain.ldif`

```ldif
dn: dc=innovteach,dc=com
objectClass: dcObject
objectClass: organization
objectClass: top
dc: innovteach
o: InnovTeach
```



```bash
sudo ldapadd -x -w password -D "cn=admin,dc=innovteach,dc=com" -f domain.ldif
```



Create the LDAP directory file `openshift.ldif`

**NOTE: Replace the `userPassword` with the generated password from `slappasswd` earlier.**

```ldif
dn: ou=openshift,dc=innovteach,dc=com
objectClass: organizationalUnit
ou: openshift

dn: ou=users,ou=openshift,dc=innovteach,dc=com
objectClass: organizationalUnit
ou: users

dn: cn=ocp-cluster-admins,ou=openshift,dc=innovteach,dc=com
objectClass: groupOfUniqueNames
cn: ocp-cluster-admins
uniqueMember: cn=ocpadminuser1,ou=openshift,dc=innovteach,dc=com

dn: cn=ocp-cluster-users,ou=openshift,dc=innovteach,dc=com
objectClass: groupOfUniqueNames
cn: ocp-cluster-users
uniqueMember: cn=ocpuser1,ou=openshift,dc=innovteach,dc=com

dn: cn=ocpuser1,ou=openshift,dc=innovteach,dc=com
objectClass: top
objectClass: person
objectClass: organizationalPerson
cn: ocpuser1
sn: openshift
userPassword: {SSHA}UCnTHeaY1VQROjKHly/tmCzur1ouUBIz

dn: cn=ocpadminuser1,ou=openshift,dc=innovteach,dc=com
objectClass: top
objectClass: person
objectClass: organizationalPerson
cn: ocpadminuser1
sn: openshift
userPassword: {SSHA}UCnTHeaY1VQROjKHly/tmCzur1ouUBIz
```



Apply the configuration

```bash
sudo ldapadd -x -w password -D "cn=admin,dc=innovteach,dc=com" -f openshift.ldif
```



#### Confirm the data is correct

```bash
ldapsearch -x -LLL -D "cn=admin,dc=innovteach,dc=com" -W -b "dc=innovteach,dc=com"
```



#### Confirm users are in the correct OU

Run the following `ldapsearch` command to confirm the users are in the `openshift` OU

```bash
ldapsearch -x -LLL -D "cn=admin,dc=innovteach,dc=com" -w password -b "ou=openshift,dc=innovteach,dc=com" -s sub "(&(objectClass=person)(objectClass=organizationalPerson))" dn cn
```





#### Create the bind secret

Encode the string `password` with `base64`

```bash
echo "password" | base64
```



Create `ldap-secret.yaml` with the following: 

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: ldap-secret
  namespace: openshift-config
type: Opaque
data:
  bindPassword: <BASE64 PASSWORD>
```



Create the secret

```bash
oc apply -f ldap-secret.yaml
```



#### Add new LDAP IdP in web portal 

Fill in the following fields:

**Name**: `ldap`

**URL**: `"ldap://YOUR_SERVER_PUBLIC_IP:389/ou=openshift,dc=innovteach,dc=com?cn"`

**Bind DN**: `cn=admin,dc=innovteach,dc=com`

**Bind password**: `password`

**ID**: `dn`

**Preferred username**: `cn`

**Name**: `cn`

Click **Add**



#### Confirm LDAP authentication is working

In the oAuth configuration page at the top right, click **Actions** -> **Edit OAuth**

In the YAML file, find the `ldap` identify provider section and change `insecure: false` to `insecure: true`



At this point, you should be able to log into the web console as one of the LDAP users. Try with `ocpadminuser1`



#### Congratulations

Now, you can manage your users with LDAP!
