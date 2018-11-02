# Installation Guide

1. Download the prometheus tile from [here](https://s3.amazonaws.com/pcf-softwares-57/prometheus-23.3.0.pivotal).
2. Upload it to Ops Manager. (Note. **PAS 2.3.+ needs to be installed**)
3. Add it and configure **AZ and Network Assignments**.
4. Configure BOSH UAA Monitor Client (Note. you can use existing credentials from BOSH Director tile. i.e. Credentials->BOSH Director->Health Monitor Credentials)
5. (optional) configure **Resource Config** if require
6. Errands: Create UAA Client for Prometheus integrate ERT UAA for Single Sign-on (SSO)
7. Apply changes

Login to ERT UAA

References:
* [Pivotal Elastic Runtime/Credentials/UAA/Admin Client Credentials](/images/ert_uaa_admin_client.png)
* [Prometheus/Credentials/Grafana/Uaa Clients Grafana](/images/grafana_client.png)


#### To retrieve or rotate admin clients from Bosh Credhub:
Login to BOSH UAA

``` $ uaac target https://<Director IP>:8443 --skip-ssl-validation ```

``` $ uaac token owner get login admin -s <grab the password from opsmanager console -> Ops Manager Director/Ops Manager Director/Uaa Login Client Credentials> -p <grab the password from opsmanager console -> Ops Manager Director/Ops Manager Director/Uaa Admin User Credentials> ```

``` $ uaac client add credhub_admin --name credhub_admin -s <some password you can remember> --scope uaa.none --authorized_grant_types client_credentials --authorities "credhub.write credhub.read" --access_token_validity 600 --refresh_token_validity 86400 --no-interactive ```

Login to Credhub via credhub_admin client created above

``` $ credhub login -s https://<Director IP>:8844 --client-name credhub_admin --client-secret <password from above step> --ca-cert ./<root_ca_certificate from opsmanager> ```

``` $ credhub find -a ```

``` $ credhub find -p /p-bosh/<prometheus deployment name> ```

``` $ credhub get --name '/p-bosh/<prometheus deployment name>/grafana_admin_credentials' ```

``` $ credhub get --name '/p-bosh/<prometheus deployment name>/prometheus_admin_credentials' ```

``` $ credhub get --name '/p-bosh/<prometheus deployment name>/alertmanager_admin_credentials' ```
