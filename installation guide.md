# Installation Guide

1. Download the prometheus tile from [here](https://s3.amazonaws.com/pcf-softwares-57/prometheus-19.0.1.pivotal).
2. Upload it to Ops Manager. (Note. **ERT 1.11.+ needs to be installed**)
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

#### To retrieve admin clients from Bosh Credhub:
1.
