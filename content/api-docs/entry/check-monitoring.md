---
Title: Check monitoring
Date: 2016-03-24T18:27:19+09:00
URL: https://mackerel.io/api-docs/entry/check-monitoring
EditURL: https://blog.hatena.ne.jp/mackerelio/mackerelio-api.hatenablog.mackerel.io/atom/entry/10328537792368374608
---

<h2 id="post">Post Monitoring Check Reports</h2>

This will transmit a monitoring check report to Mackerel. Monitoring reports are restricted to hosts.

<p class="type-post">
  <code>POST</code>
  <code>/api/v0/monitoring/checks/report</code>
</p>

The implementation described in [Adding monitors for script checks](https://mackerel.io/docs/entry/custom-checks) is used. The agent will periodically transmit the list of configured monitoring checks to the [Update Host Information](/api-docs/entry/hosts#update-information) API and any monitoring checks not included in that list and those which do not have any open alerts will be deleted from Mackerel at that time.
Posts will be ignored in the following cases:

- When the time of monitoring time proceeds the time of posting by 6 hours or more
- When a monitoring time has already been posted with the same name / host



### Required permissions for the API key

<ul class="api-key">
  <li class="label-read">Read</li>
  <li class="label-write">Write</li>
</ul>

### Input

```json
{
  "reports": [ <report>, <report>, … ]
}
```

<i>`<report>`</i> : an object that holds the following keys. Up to 100 <i>`<report>`</i> can be sent with one API request.

| KEY          | TYPE     | DESCRIPTION                      |
| ------------ | -------- | -------------------------------- |
| `source`     | *source* | refer to the note below          |
| `name`       | *string* | monitor name                     |
| `status`     | *string* | alert status of the monitor report. `"OK"`, `"CRITICAL"`, `"WARNING"`, or `"UNKNOWN"`  |
| `message`    | *string* | auxiliary text attached to the alert status of the monitor report, under 1024 characters |
| `occurredAt` | *number* | unix timestamp of execution |
| `notificationInterval` | *number* | [optional] Notifications will not be re-sent if the time interval (in minutes) is omitted. If the time interval is set for less than 10 minutes, the notifications will be re-sent at an interval of 10 minutes. |
| `maxCheckAttempts` | *number* | [optional] number of consecutive Warning/Critical instances before an alert is made[*1](#maxCheckAttempts). Default setting is 1 (1-10) |


`source` : an object that holds the following keys

| KEY          | TYPE     | DESCRIPTION                      |
| ------------ | -------- | -------------------------------- |
| `type`       | *string* | fixed character string `"host"`                     |
| `hostId`     | *string* | host ID (given by the server when the host was registered) |

### Response

#### Success

```json
{
  "success": true
}
```

#### Error

<table class="default api-error-table">
  <thead>
    <tr>
      <th class="status-code">STATUS CODE</th>
      <th class="description">DESCRIPTION</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>403</td>
      <td>when the API key doesn't have the required permissions / when accessing from outside the <a href="https://support.mackerel.io/hc/en-us/articles/360039701952" target="_blank">permitted IP address range</a></td>
    </tr>
    <tr>
      <td>other than 200</td>
      <td>any error other than the above</td>
    </tr>
  </tbody>
</table>

<h4 id="maxCheckAttempts" class="annotation">*1 maxCheckAttempts</h4>
If `maxCheckAttempts` is 2 or more, the check monitor posting interval must be within 6 hours in order to run normally.
