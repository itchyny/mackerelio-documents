---
Title: Using the official plugin pack to visualize middleware metrics
Date: 2015-02-16T18:16:25+09:00
URL: https://mackerel.io/docs/entry/howto/mackerel-agent-plugins
EditURL: https://blog.hatena.ne.jp/mackerelio/mackerelio-docs.hatenablog.mackerel.io/atom/entry/8454420450083908463
---

By using the official plugin pack, which supports many different kinds of middleware, it’s easy to gather middleware metrics. Supported environments will be the same as `mackerel-agent`, so for more information about that please refer to our [outline of supported environments](https://mackerel.io/docs/entry/overview#support-environments).

In order to monitor metrics for AWS services like ELB or RDS, consider using [AWS Integration](https://mackerel.io/docs/entry/integrations/aws). As for the posting of service metrics, please use [fluentd](http://www.fluentd.org/). For more information please refer to [Posting metrics related to ELB from AWS CloudWatch to service metrics.](https://mackerel.io/docs/entry/advanced/fluentd#example-elb)

Check out the following README for the plugin package list.

[https://github.com/mackerelio/mackerel-agent-plugins:embed:cite]

## Installing the official plugin pack (for Linux OS)

For installation we recommend using either the ‘yum’ or ‘apt’ repository according to the environment which you will be using. For information on configuring the repository, please refer to [Installing mackerel-agent][]. If you would like to acquire the ‘rpm’ and ‘deb’ files directly, please check [GitHub Releases][].

[Installing mackerel-agent]: https://mackerel.io/my/instruction-agent
[GitHub Releases]: https://github.com/mackerelio/mackerel-agent-plugins/releases

### if rpm package

```
sudo yum install mackerel-agent-plugins
```

Updates can be performed using the following command.

```
sudo yum update mackerel-agent-plugins
```

### if deb package

```
sudo apt-get install mackerel-agent-plugins
```

Updates can be performed using the following command.

```
sudo apt-get update
sudo apt-get install mackerel-agent-plugins
```

## Installing the official plugin pack (for Windows Server)

When using the official plugin pack on a Windows Server, we recommend using a plugin that comes bundled with the agent. (For details on how to install the agent, refer to [Installing Mackerel Agent][].)

For a list of the bundled plugins, refer to the [GitHub Repository Bundled Plugin List][]. Plugins that are not included in this list are not officially supported and must be built independently.

Updating the agent will also update the bundled plugins. For more information on how to update, refer to [Installing mackerel-agent on Windows](https://mackerel.io/docs/entry/howto/install-agent/msi).

[Installing Mackerel Agent]: https://mackerel.io/my/instruction-agent
[GitHub Repository Bundled Plugin List]: https://github.com/mackerelio/mackerel-agent/blob/master/wix/plugins_windows.go

## Using the official plugin pack (for Linux OS)

Each plugin has been installed in `/usr/bin` ,so please configure the mackerel-agent settings file to meet with the plugin which you will be using as shown below.

if using plugin for Apache2
```
[plugin.metrics.apache2]
command = ["mackerel-plugin-apache2"]
```

For more on how to use each plugin, refer below to the README of each plugin in GitHub.

## Using the official plugin pack (for Windows Server)

As mentioned above, when using the official plugin pack on a Windows Server, we recommend using a plugin that comes bundled with the agent. When using a bundled plugin, each is stored in the agent's installation folder and run through pass. Add the following configuration according to the plugin of use in the mackerel-agent configuration file. Then restart mackerel-agent to apply the configuration.

When using the "mackerel-plugin-mssql" plugin, the description is as follows.

```
[plugin.metrics.mssql]
command = ["mackerel-plugin-mssql"]
```

For more on how to use each plugin, refer below to the README of each plugin in GitHub.

## Plugin pack code

The official plugin pack is open to the public and can be found here [https://github.com/mackerelio/mackerel-agent-plugins:title].
We are always happy to receive Pull Requests, comments, etc. regarding plugins that support new middleware.

For creating plugins please refer to [Posting custom metrics](https://mackerel.io/docs/entry/advanced/custom-metrics).

-

If you have any questions please contact our support team at support@mackerel.io
