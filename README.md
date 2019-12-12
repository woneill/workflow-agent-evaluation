# workflow-agent-evaluation
Hackathon project evaluating various OSS tools that work similar to Yahoo Pipes, IFTTT, or Zapier


Alas poor Yahoo Pipes! I used to use them, Horatio...

Sure, you can use IFTTT or Zapier for making workflows like good old Yahoo Pipes but there's nothing better than running a service yourself where YOU get to decide when to pull the plug on it! This project will be a quick comparison of some alternatives that can be run locally. I'm thinking of [Huginn](https://github.com/huginn/huginn), [Node-RED](https://nodered.org/), [n8n.io](https://n8n.io/), [Beehive](https://github.com/muesli/beehive).... any others?

## [Huginn](https://github.com/huginn/huginn)

### Setup

1. `cd huginn/docker/single-process; docker-compose up`
1. Open Huginn in the browser http://localhost:3000
1. Log in to your Huginn instance using the username `admin` and password `password`

### Demonstration App

I adopted some of the sample workflows that ship by default with Huginn and tweaked them to emulate our Friday Demo's weather report. It will check Dark Sky at 4pm, check to see if rain or snow is in the weather report and send a slack notice if so.

To get started, you will first need to configure an incoming webhook.
* Go to `https://my.slack.com/services/new/incoming-webhook`, choose a default channel and add the integration.
* Your webhook URL will look like: `https://hooks.slack.com/services/some/random/characters`

Next:

1. Import the [credentials file](huginn_configs/user_credentials.json) via http://localhost:3000/user_credentials
    * Edit the `***CHANGEME***` values as appropriate
1. Import the [scenario file](huginn_configs/demo-weather-report.json) via http://localhost:3000/scenarios/scenario_imports/new

### Overall impression

Setup was easy and being able to export/import credentials and scenarios was nice. But it feels pretty limited as a multi-user approach. You _can_ make multiple users but I found lots of Github issues about naming conflicts between agents and scenarios. As a single user tool, though, the wide range of built in agents feels pretty powerful.

## [Node-RED](https://nodered.org/)

### Setup

1. `cd nodered; docker-compose up`
1. Open Node-RED in the browser http://localhost:1880/

### Demonstration App

I tried recreating the weather report workflow from Huginn but this tool is just too low level to make it work. The nodes for making HTML requests and parsing the responses are barebones. As a result, despite it having a nice web UI for creating and linking nodes together, it feels like it'd be simpler to do a `curl | jq` or use something like Python's [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/) library.

### Overall impression

Since I gave up on the tool pretty quickly I don't think it's suitable as an IFTTT/Zapier/Yahoo Pipes replacement. It'd be more useful as a teaching tool about low level data transformations

## [n8n.io](https://n8n.io/)

### Setup

1. `cd n8n/docker/compose/withPostgres; docker-compose up`
1. Open n8n in the browser http://localhost:5678/
1. Log in to your instance using the username `changeUser` and password `changePassword`
    * These values are the defaults in [.env](n8n/docker/compose/withPostgres/.env)

### Demonstration App

Once again I tried recreating the weather report workflow from Huginn. It wasn't obvious at first how to use data from other nodes in the workflow. Initially I thought I'd have to manually create macros such as `{{$node["Set"].data["city_name"]}}` but then I learned that setting parameters to be "expressions" made it easy to drill down via a UI to the parsed JSON fields from other nodes.

To get started, you'll need to create credentials for OpenWeatherMap and Slack. There doesn't appear to be a way to import/export these.

Next, in the browser UI go to Workflows -> Import from File and chose [weather_report.json](n8n_configs/weather_report.json). You'll then need to connect the "Start" node to OpenWeatherMap in order to get the "Execute Workflow" to run outside of the cron schedule.

### Overall impression

Just like Huginn, setup was easy and being able to export/import workflows was nice. I'm disappointed that you couldn't do the same with credentials, though, since their references end up hardcoded in the workflow's JSON.

As a single user tool the wide range of built in agents feels pretty powerful. I don't think that there's even an option for logging in as a different user than the default so this wouldn't be useful as a multi-user tool.
