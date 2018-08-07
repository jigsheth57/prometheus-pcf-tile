# Installation Guide

1. Download the prometheus tile from [here](https://s3.amazonaws.com/pcf-softwares-57/prometheus-21.1.0.pivotal).
2. Upload it to Ops Manager. (Note. **ERT 2.2.+ needs to be installed**)
3. Add it and configure **AZ and Network Assignments**.
4. (optional) configure **Resource Config** if require
5. Apply changes
6. Integrate ERT UAA for Single Sign-on (SSO)

Login to ERT UAA

References:
* [Pivotal Elastic Runtime/Credentials/UAA/Admin Client Credentials](/images/ert_uaa_admin_client.png)
* [Prometheus/Credentials/Grafana/Uaa Clients Grafana](/images/grafana_client.png)


``` $ uaac target uaa.<system domain> ```

``` $ uaac token client get admin -s <grab the password from opsmanager console -> Pivotal Elastic Runtime/Credentials/UAA/Admin Client Credentials>```

Once login to ERT UAA, create grafana client, which would be used as SSO for Grafana dashboard.

``` $ uaac client add grafana --name grafana -s <grab the password from opsmanager console -> Prometheus/Credentials/Grafana/Uaa Clients Grafana> --scope openid --authorized_grant_types authorization_code --authorities "uaa.none" --access_token_validity 43200 --refresh_token_validity 43200 --redirect_uri https://grafana.<system domain> --no-interactive ```

#### To retrieve or rotate admin clients from Bosh Credhub:
Login to BOSH UAA

``` $ uaac target https://<Director IP>:8443 --skip-ssl-validation ```

``` $ uaac token owner get login admin -s <grab the password from opsmanager console -> Ops Manager Director/Ops Manager Director/Uaa Login Client Credentials> -p <grab the password from opsmanager console -> Ops Manager Director/Ops Manager Director/Uaa Admin User Credentials> ```

``` $ uaac client add credhub_admin --name credhub_admin -s <some password you can rememeber> --scope uaa.none --authorized_grant_types client_credentials --authorities "credhub.write credhub.read" --access_token_validity 600 --refresh_token_validity 86400 --no-interactive ```

Login to Credhub via credhub_admin client created above

``` $ credhub login -s https://<Director IP>:8844 --client-name credhub_admin --client-secret <password from above step> --ca-cert ./<root_ca_certificate from opsmanager> ```

``` $ credhub find -a ```

``` $ credhub find -p <path for prometheus deployment> ```

``` $ credhub get --name '<path for prometheus deployment>/grafana_admin_credentials' ```
