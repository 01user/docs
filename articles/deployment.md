# Auth0 Deployment Models

Auth0 is offered in 4 deployment models:

1. As a __multi-tenant cloud service__ running on Auth0's cloud.
2. As a __dedicated cloud service__ running on Auth0's cloud.
3. As a __dedicated cloud service__ running on Customer's cloud infrastructure.
4. As an __on-premises virtual appliance__ running on Customer's data centers.

The following table describes operational and feature differences between each of these models.

<table class="table">
    <thead>
        <tr>
            <th class="info">Where It Runs</th>
            <th class="info">Auth0's Infrastructure</th>
            <th class="info"></th>
            <th class="info">Customer's Infrastructure</th>
            <th class="info"></th>
        </tr>
        <tr>
            <th>How It Runs</th>
            <th>Multi-Tenant</th>
            <th>Dedicated</th>
            <th>Cloud</th>
            <th>On-Premises</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th>Public Facing</th>
            <td>Yes</td>
            <td>Yes</td>
            <td>Configurable</td>
            <td>Configurable</td>
        </tr>
        <tr>
            <th>Updates</th>
            <td>Unscheduled. <br /> Multiple times per day. <br /><br />Staged in two zones.        </td>
            <td>Cumulative, deployed post multi-tenant update, and coordinated with Customer.</td>
            <td>Scheduled with Customer. <br /><br />Minimum 1/month, except critical updates (e.g. vulnerabilities, security updates)</td>
            <td>Scheduled with Customer. <br /><br />Minimum 1/month, except critical updates (e.g. vulnerabilities, security updates)</td>
        </tr>
        <tr>
            <th>Service & Uptime Reporting</th>
            <td>http://status.auth0.com<br />http://uptime.auth0.com</td>
            <td>Dedicated uptime URL</td>
            <td>Monitored by Auth0 and Customer's tools</td>
            <td>Monitored by Auth0 and Customer's tools</td>
        </tr>
        <tr>
            <th>Geolocation</th>
            <td>Yes</td>
            <td>Yes</td>
            <td>Yes</td>
            <td>Yes</td>
        </tr>
        <tr>
            <th>SLA Provided</th>
            <td>Yes</td>
            <td>Yes</td>
            <td>No</td>
            <td>No</td>
        </tr>
        <tr>
            <th>Support Channels & Levels</th>
            <td>Same across all models</td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <th class="info">Features</th>
            <td class="info"></td>
            <td class="info"></td>
            <td class="info"></td>
            <td class="info"></td>
        </tr>
        <tr>
            <th>SSO Lifetime</th>
            <td>Default Settings</td>
            <td>Configurable</td>
            <td>Configurable</td>
            <td>Configurable</td>
        </tr>
        <tr>
            <th>Search</th>
            <td>Elastic Search</td>
            <td>DB-based</td>
            <td>DB-based</td>
            <td>DB-based</td>
        </tr>
        <tr>
            <th>Code Sandbox</th>
            <td>Multi-Language</td>
            <td>JS Only</td>
            <td>JS Only</td>
            <td>JS Only</td>
        </tr>
        <tr>
            <th>Connecting IP Address Filtering Restrictions</th>
            <td>No</td>
            <td>No</td>
            <td>Yes</td>
            <td>Yes</td>
        </tr>
        <tr>
            <th>Custom Domains</th>
            <td>No</td>
            <td>Yes</td>
            <td>Yes</td>
            <td>Yes</td>
        </tr>
    </tbody>
</table>
