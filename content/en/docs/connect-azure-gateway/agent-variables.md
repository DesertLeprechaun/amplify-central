---
title: Reference - Agent configuration
linkTitle: Reference - Agent configuration
draft: false
weight: 30
description: Use the following environment variables to control your Discovery
  and Traceability agents
---
As the Discovery and Traceability agents share many parameters, it is more efficient to use environment variables and reference these parameters, instead of declaring parameters twice.

To maintain a shareable collection of environment files, you can create a `da_env_vars.env` (Discovery Agent) and `ta_env_vars.env` (Traceability Agent) file per environment, which contains simple key value pairs.  By default, agent configuration files are looking for corresponding environment variables before looking on the configuration file property. This file can be used for both modes of the agent (binary VS Docker container).
  
Note that the agent (binary mode) will accept an argument pointing to the environment variables file, which you can point to the `da_env_vars.env` or `ta_env_vars.env` file. Use the --envFile `da_env_vars.env` argument with either agent, pointing to the file for that agent.

Note that the Docker image of the agent is expecting this `da_env_vars.env` or `ta_env_vars.env` as an argument of the Docker runner `docker run --env-file <PATH>/da_env_vars.env...`

Some variables/properties have a default known value so that there is no need to parameter them.

If you are either struggling with a variable value or you want to benefit from the advanced agents features (API filtering / SSL security / proxy access / logging), the following section describe all the variables the agents (Discovery / Traceability) rely on.

## Complete variable list for advance features

You can extend the previous minimum variable list with the following variables. Some are common to all agents and some are specific to an agent.

### Common variables to both agents

| Variable name                                                      | Description                                                                                                                                                                                                                                                                                                               |
| ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Azure configuration variables**                                  |                                                                                                                                                                                                                                                                                                                           |
| AZURE_SUBSCRIPTIONID                                               | The Azure subscription identifier.                                                                                                                                                                                                                                                                                        |
| AZURE_TENANTID                                                     | The tenantID of the service principal.                                                                                                                                                                                                                                                                                    |
| AZURE_CLIENTID                                                     | The appId of the service principal.                                                                                                                                                                                                                                                                                       |
| AZURE_CLIENTSECRET                                                 | The password of the service principal.                                                                                                                                                                                                                                                                                    |
| AZURE_RESOURCEGROUPNAME                                            | The container name that holds resources.                                                                                                                                                                                                                                                                                  |
| AZURE_APIMSERVICENAME                                              | The container that holds the API you want the agent to discover.                                                                                                                                                                                                                                                          |
| AZURE_PUSHTAGS                                                     | When set to TRUE, the Azure API tags will be pushed to Amplify Central.                                                                                                                                                                                                                                                   |
| AZURE_FILTER                                                       | Filter condition expression for discovering APIs based on tags. The conditional expression must have \"tag\" as the prefix/selector. Azure Discovery Agent supports only Exists() call expression-based conditions. For example, `tag.some_tag_name.Exists() == true`.                                                    |
|                                                                    |                                                                                                                                                                                                                                                                                                                           |
| **Amplify Central variables**                                      |                                                                                                                                                                                                                                                                                                                           |
| CENTRAL_DEPLOYMENT                                                 | Specifies region (default: US = `prod` / EU = `prod-eu`).                                                                                                                                                                                                                                                                 |
| CENTRAL_URL                                                        | The URL to the Amplify Central instance being used for agents (default value: US =  `<https://apicentral.axway.com>` / EU = `https://central.eu-fr.axway.com`).                                                                                                                                                           |
| CENTRAL_ORGANIZATIONID                                             | The Organization ID from Amplify Central. Locate this at Platform > User > Organization.                                                                                                                                                                                                                                  |
| CENTRAL_TEAM                                                       | The name of the team in Amplify Central that all APIs will be linked to. Locate this at Amplify Central > Access > Team Assets.                                                                                                                                                                                           |
| CENTRAL_MODE                                                       | Method to send endpoints back to Central. (`publishToEnvironment` = API Service, `publishToEnvironmentAndCatalog` = API Service and Catalog asset).                                                                                                                                                                       |
| CENTRAL_APPENDENVIRONMENTTOTITLE                                   | Set to false to skip adding the environment name to the title and description of the API.                                                                                                                                                                                                                                 |
| CENTRAL_REPORTACTIVITYFREQUENCY                                    | The time interval at which the HTTP client times out making HTTP requests and processing the response (ns - default, us, ms, s, m, h). Set to 60s.                                                                                                                                                                        |
| CENTRAL_CLIENTTIMEOUT                                              | The frequency at which the agent polls for event changes for the periodic agent status updater (ns - default, us, ms, s, m, h). Set to 5m.                                                                                                                                                                                |
| CENTRAL_APISERVICEREVISIONPATTERN                                  | The naming pattern for APIServiceRevision Title. Date formats can be `{{.Date:YYYY/MM/DD}}, {{.Date:YYYY-MM-DD}}, {{.Date:MM/DD/YYYY}}, or {{.Date:MM-DD-YYYY}}`. Current default set to `{{.APIServiceName}} - {{.Date:YYYY/MM/DD}} - r {{.Revision}}`.                                                                  |
| CENTRAL_PROXYURL                                                   | The URL for the proxy for Amplify Central `<http://username:password@hostname:port>`. If empty, no proxy is defined.                                                                                                                                                                                                      |
| CENTRAL_AUTH_URL                                                   | The Amplify login URL: `<https://login.axway.com/auth>`.                                                                                                                                                                                                                                                                  |
| CENTRAL_AUTH_REALM                                                 | The Realm used to authenticate for Amplify Central: `Broker`.                                                                                                                                                                                                                                                             |
| CENTRAL_AUTH_CLIENTID                                              | The client identifier associated to the Service Account created in Amplify Central. Locate this at Amplify Central > Access > Service Accounts > client Id.                                                                                                                                                               |
| CENTRAL_AUTH_PRIVATEKEY                                            | The private key associated with the Service Account.                                                                                                                                                                                                                                                                      |
| CENTRAL_AUTH_PUBLICKEY                                             | The public key associated with the Service Account.                                                                                                                                                                                                                                                                       |
| CENTRAL_AUTH_KEYPASSWORD                                           | The password for the private key, if applicable.                                                                                                                                                                                                                                                                          |
| CENTRAL_AUTH_TIMEOUT                                               | The timeout to wait for the authentication server to respond (ns - default, us, ms, s, m, h). Set to 10s.                                                                                                                                                                                                                 |
| CENTRAL_ENVIRONMENT                                                | Name of the Amplify Central environment where API will be hosted.                                                                                                                                                                                                                                                         |
| CENTRAL_APISERVERVERSION                                           | Version of the API Server that the agent will communicate with.                                                                                                                                                                                                                                                           |
| CENTRAL_SSL_MINVERSION                                             | String value for the minimum SSL/TLS version that is acceptable. If zero, empty TLS 1.0 is taken as the minimum. Allowed values are: TLS1.0, TLS1.1, TLS1.2, TLS1.3.                                                                                                                                                      |
| CENTRAL_SSL_MAXVERSION                                             | String value for the maximum SSL/TLS version that is acceptable. If empty, then the maximum version supported by this package is used, which is currently TLS 1.3. Allowed values are: TLS1.0, TLS1.1, TLS1.2, TLS1.3.                                                                                                    |
| CENTRAL_SSL_CIPHERSUITES                                           | An array of strings. It is a list of supported cipher suites for TLS versions up to TLS 1.2. If CipherSuites is nil, a default list of secure cipher suites is used, with a preference order based on hardware performance. See [Supported Cipher Suites](/docs/connect-api-manager/agent-security-api-manager/). |
| CENTRAL_SSL_INSECURESKIPVERIFY                                     | Controls whether a client verifies the server's certificate chain and host name. If true, TLS accepts any certificate presented by the server and any host name in that certificate. In this mode, TLS is susceptible to man-in-the-middle attacks.                                                                       |
| CENTRAL_SUBSCRIPTIONS_APPROVAL_MODE                                | The mode for approving subscriptions on Amplify Central (manual, auto, webhook; default = manual).                                                                                                                                                                                                                        |
| CENTRAL_SUBSCRIPTIONS_APPROVAL_WEBHOOK_URL                         | The url for a subscription approval webhook (if any). CENTRAL_SUBSCRIPTIONS_APPROVAL_MODE must be set to "webhook" for webhooks to be invoked.                                                                                                                                                                            |
| CENTRAL_SUBSCRIPTIONS_APPROVAL_WEBHOOK_HEADERS                     | The headers to pass to the subscription approval webhook (if any). For example, "Header=contentType,Value=application/json".                                                                                                                                                                                              |
| CENTRAL_SUBSCRIPTIONS_APPROVAL_WEBHOOK_AUTHSECRET                  | The authentication secret to pass to the subscription approval webhook (if any).                                                                                                                                                                                                                                          |
| CENTRAL_SUBSCRIPTIONS_NOTIFICATIONS_WEBHOOK_URL                    | The webhook URL that subscription data will be posted to, see Subscription webhook notifications.                                                                                                                                                                                                                         |
| CENTRAL_SUBSCRIPTIONS_NOTIFICATIONS_WEBHOOK_HEADERS                | The headers that will be used when posting data to the webhook url, see Subscription webhook notifications.                                                                                                                                                                                                               |
| CENTRAL_SUBSCRIPTIONS_NOTIFICATIONS_SMTP_HOST                      | The SMTP server that will send email notifications.                                                                                                                                                                                                                                                                       |
| CENTRAL_SUBSCRIPTIONS_NOTIFICATIONS_SMTP_PORT                      | The SMTP port to communicate to the SMTP server over.                                                                                                                                                                                                                                                                     |
| CENTRAL_SUBSCRIPTIONS_NOTIFICATIONS_SMTP_AUTHTYPE                  | The authentication type based on the email server.  You may have to refer to the email server properties and specifications. This value defaults to NONE.                                                                                                                                                                 |
| CENTRAL_SUBSCRIPTIONS_NOTIFICATIONS_SMTP_USERNAME                  | The username used to authenticate to the SMTP server, if necessary.                                                                                                                                                                                                                                                       |
| CENTRAL_SUBSCRIPTIONS_NOTIFICATIONS_SMTP_PASSWORD                  | The password used to authenticate to the SMTP server, if necessary.                                                                                                                                                                                                                                                       |
| CENTRAL_SUBSCRIPTIONS_NOTIFICATIONS_SMTP_FROMADDRESS               | The email address that will be listed in the from field.                                                                                                                                                                                                                                                                  |
| CENTRAL_SUBSCRIPTIONS_NOTIFICATIONS_SMTP_SUBSCRIBE_SUBJECT         | The subject of email sent for Subscribe events.                                                                                                                                                                                                                                                                           |
| CENTRAL_SUBSCRIPTIONS_NOTIFICATIONS_SMTP_SUBSCRIBE_BODY            | The body of the email for Subscribe events.                                                                                                                                                                                                                                                                               |
| CENTRAL_SUBSCRIPTIONS_NOTIFICATIONS_SMTP_SUBSCRIBE_OAUTH           | The body of the email for Subscribe events when the API is secured using an OAUTH token.                                                                                                                                                                                                                                  |
| CENTRAL_SUBSCRIPTIONS_NOTIFICATIONS_SMTP_SUBSCRIBE_APIKEYS         | The body of the email for Subscribe events when the API is secured using an APIKEY.                                                                                                                                                                                                                                       |
| CENTRAL_SUBSCRIPTIONS_NOTIFICATIONS_SMTP_UNSUBSCRIBE_SUBJECT       | The subject of email sent for Unsubscribe events.                                                                                                                                                                                                                                                                         |
| CENTRAL_SUBSCRIPTIONS_NOTIFICATIONS_SMTP_UNSUBSCRIBE_BODY          | The body of the email for Unsubscribe events.                                                                                                                                                                                                                                                                             |
| CENTRAL_SUBSCRIPTIONS_NOTIFICATIONS_SMTP_SUBSCRIBEFAILED_SUBJECT   | The subject of email sent for Failed to Subscribe events.                                                                                                                                                                                                                                                                 |
| CENTRAL_SUBSCRIPTIONS_NOTIFICATIONS_SMTP_SUBSCRIBEFAILED_BODY      | The body of the email for Failed to Subscribe events.                                                                                                                                                                                                                                                                     |
| CENTRAL_SUBSCRIPTIONS_NOTIFICATIONS_SMTP_UNSUBSCRIBEFAILED_SUBJECT | The subject of email sent for Failed to Unsubscribe events.                                                                                                                                                                                                                                                               |
| CENTRAL_SUBSCRIPTIONS_NOTIFICATIONS_SMTP_UNSUBSCRIBEFAILED_BODY    | The body of the email for Failed to Unsubscribe events.                                                                                                                                                                                                                                                                   |
|                                                                    |                                                                                                                                                                                                                                                                                                                           |
| **Status variables**                                               |                                                                                                                                                                                                                                                                                                                           |
| STATUS_PORT                                                        | Port used for checking the health status of the running agent.                                                                                                                                                                                                                                                            |
| STATUS_HEALTHCHECKINTERVAL                                         | Time in seconds between running periodic health checker (binary agents only). Allowed values are from 30 seconds to 5 minutes. Specify value as s or m. (default value: 30s).                                                                                                                                             |
| STATUS_HEALTHCHECKPERIOD                                           | Time in minutes allotted for services to be ready before exiting the agent. Allowed values are from 1 to 5 minutes.                                                                                                                                                                                                       |
|                                                                    |                                                                                                                                                                                                                                                                                                                           |
| **Logging variables**                                              |                                                                                                                                                                                                                                                                                                                           |
| LOG_LEVEL                                                          | The log level for output messages (debug, info, warn, error).                                                                                                                                                                                                                                                             |
| LOG_FORMAT                                                         | The format to print log messages (json, line, package).                                                                                                                                                                                                                                                                   |
| LOG_OUTPUT                                                         | The output for the log lines (stdout, file, both).                                                                                                                                                                                                                                                                        |
| LOG_MASKEDVALUES                                                   | Comma-separated list of keywords to identify within the agent config, which is used to mask its corresponding sensitive data. Keywords are matched by whole words and are case-sensitive.                                                                                                                                 |
| LOG_FILE_NAME                                                      | The name of the log files.                                                                                                                                                                                                                                                                                                |
| LOG_FILE_PATH                                                      | The path (relative or absolute) to save logs files, if output type file or both.                                                                                                                                                                                                                                          |
| LOG_FILE_ROTATEEVERYMEGABYTES                                      | The max size, in megabytes that a log file can grow to.                                                                                                                                                                                                                                                                   |
| LOG_FILE_KEEPFILES                                                 | The max number of log file backups to keep.                                                                                                                                                                                                                                                                               |
| LOG_FILE_CLEANBACKUPS                                              | The max age of a backup file, in days.                                                                                                                                                                                                                                                                                    |

Note: For logging, it is recommended to set it up in the agent configuration file to keep the log separated for each agent.

### Specific variables for Discovery Agent

| Variable name          | Description                                                                                                                      |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| CENTRAL_ADDITIONALTAGS | Additional tag names to publish while publishing the API. Could help to identified the API source. It is a comma separated list. |

### Specific variables for Traceability Agent

| Variable name                                  | Description                                                                                                                                                      |
| ---------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CENTRAL_USAGEREPORTING_PUBLISH                 | Enables/disables the sending of usage events to Amplify (default value: `true`). Takes precedence over replaced variable CENTRAL_PUBLISHUSAGE.                   |
| CENTRAL_USAGEREPORTING_INTERVAL                | The frequency in which the agent reports API usage to Amplify (default value: `300s`). Takes precedence over replaced variable CENTRAL_EVENTAGGREGATIONINTERVAL. |
| CENTRAL_USAGEREPORTING_OFFLINE                 | Set to True to enable offline usage reporting (default value: False). Ignores CENTRAL_USAGEREPORTING_INTERVAL value as that is only for online reporting.        |
| CENTRAL_USAGEREPORTING_OFFLINESCHEDULE         | Determines how often usage numbers should be saved (default value and minimum: @hourly).                                                                         |
| TRACEABILITY_PROTOCOL                          | Protocol (https or tcp) to be used for communicating with ingestion service (default value: `tcp`).                                                              |
| TRACEABILITY_PROXYURL                          | The socks5 or http URL of the proxy server for ingestion service (`<socks5://hostname:port>`). If empty, no proxy is defined.                                    |
| TRACEABILITY_COMPRESSIONLEVEL                  | The gzip compression level for the output event (default value: `3`).                                                                                            |
| TRACEABILITY_BULKMAXSIZE                       | The maximum number of events to bulk in a single ingestion request (default value: `100`).                                                                       |
| TRACEABILITY_CLIENTTIMEOUT                     | The time to wait for ingestion response (default value: `60s`).                                                                                                  |
| TRACEABILITY_WORKER                            | The number of workers collecting events and sending them to Amplify Central (default value: `1`).                                                                |
| TRACEABILITY_REDACTION_PATH_SHOW               | Determines what portions of a transactions PATH to send to Amplify Central.                                                                                      |
| TRACEABILITY_REDACTION_QUERYARGUMENT_SHOW      | Determines what Query Arguments to send to Amplify Central.                                                                                                      |
| TRACEABILITY_REDACTION_QUERYARGUMENT_SANITIZE  | Determines what portions of a Query Arguments value to sanitize.                                                                                                 |
| TRACEABILITY_REDACTION_REQUESTHEADER_SHOW      | Determines what Request Header Keys to send to Amplify Central.                                                                                                  |
| TRACEABILITY_REDACTION_REQUESTHEADER_SANITIZE  | Determines what portions of a Request Headers value to sanitize.                                                                                                 |
| TRACEABILITY_REDACTION_RESPONSEHEADER_SHOW     | Determines what Response Header Keys to send to Amplify Central.                                                                                                 |
| TRACEABILITY_REDACTION_RESPONSEHEADER_SANITIZE | Determines what portions of a Response Headers value to sanitize.                                                                                                |
| TRACEABILITY_REDACTION_MASKING_CHARACTERS      | Determines what characters are displayed as the sanitized response header values on Amplify (default value `{*}`).                                               |
| TRACEABILITY_SAMPLING_PERCENTAGE               | The percentage of all transactions that are sent to Amplify. A value of `0` reports no traffic (default value `100`).                                            |
| TRACEABILITY_SAMPLING_PER_API                  | When true, the percentage of API transactions sent will be based on each API ID in the transaction (default value: `true`).                                      |
| QUEUE_MEM_EVENTS                               | The size of the internal queue used for storing consumed events before publishing them (default value: `2048`).                                                  |
| QUEUE_MEM_FLUSH_MINEVENTS                      | The minimum number of events in queue required for publishing (default value: `100`).                                                                            |
| QUEUE_MEM_FLUSH_TIMEOUT                        | The maximum time to wait for min_events to be fulfilled (default value: `1s`).                                                                                   |
| AZURE_GETHEADERS                               | Call the Azure Gateway API to get additional transaction details (headers, useragent). Default is True.                                                          |