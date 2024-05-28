# Postfix  Outgoing Mailserver
A simple Helm Chart to setup a postfix send only mailserver. Additionally
the mailserver can be configured to use opendkim and add users to allow
only authenticated access

## Introduction
This chart bootstraps a postfix deployment on a schedule to create an outgoing only mailserver. It provides values to configure users using `saslauth` and allow specific characters in mails only. Additionally
if needed, opendkim can also be enabled.

## Installing the chart
```bash
helm repo add tinkerstation https://tinkerstation.github.io/helm-charts
helm repo update
helm install mailserver tinkerstation/postfix -f override.yaml
```

## How to use it

Just create your override.yaml file and specify your schedule, retention time and pvcs as a comma separated list.

```yaml
network:
  loadBalancerIP: 10.0.0.5
  allowedIPRange: 127.0.0.1/32, 192.168.0.0/24
  domain: tinkerstation.com
auth:
  - username: mailuser
    password: mailuser123
```

## Values

| Parameter | Description | Default |
| --------- | ----------- | ------- |
| `image.pullPolicy`            | Image pull policy                                     | `IfNotPresent`    |
| `image.tag`                   | `CronJob` image tag                                   | `latest`          |
| `imagePullSecrets`            | imagePullSecrets to use for private repository        | `None`            |
| `network.loadBalancerIP`        | IPv4 for the postfix `Service` of type loadbalancer   | `None`       |
| `network.allowedIPRange`    | List of CIDR Ranges to allow the use of postfix mailserver | `None`            |
| `network.domain` | the fqdn domain name for the mailserver               | `None`               |
| `auth[*].username`   | Create User for the mailserver                     | `None`           |
| `auth[*].password`            | Create password for the User of the mailserver                             | `None` |
| `postfix.messageSizeLimit`       | max message size in bytes               | `30720000`           |
| `postfix.allowedEmailPattern`  |  list of PERL REGEX to allow characters in the used emails     | `permits use of alphabets, digits and the special characters - and _`            |
| `postfix.config`         | Additional `main.cf` config for the postfix mailserver      | `None`         |
| `networkPolicy.enabled`                 | Create a `NetworkPolicy` for the Postfix application                 | `false`            |
| `networkPolicy.egressToKubeDNSEnabled`            | Allow Egress to `kube-dns`                           | `false`            |
| `networkPolicy.egressToGoogleDNSEnabled`               |Allow Egress to `Google DNS Servers`              | `false`         |
| `networkPolicy.ingress`        | List of Custom Ingress Rules for the `NetworkPolicy`       | `None`         |
| `networkPolicy.egress`                  | List of Custom Egress Rules for the `NetworkPolicy`       | `None`       |
| `opendkim.enabled`          | Enable OpenDKIM for postfix mailserver                                  | `None`            |
| `opendkim.domains[*].domain`             | fqdn of the domain used                                     | `None`            |
| `opendkim.domains[*].selector`                | subdomain used for the mailserver                        | `None`            |
| `opendkim.domains[*].private_key`                 | private key                       | `None`            |