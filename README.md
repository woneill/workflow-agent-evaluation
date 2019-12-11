# workflow-agent-evaluation
Hackathon project evaluating various OSS tools that work similar to Yahoo Pipes, IFTTT, or Zapier


Alas poor Yahoo Pipes! I used to use them, Horatio...

Sure, you can use IFTTT or Zapier for making workflows like good old Yahoo Pipes but there's nothing better than running a service yourself where YOU get to decide when to pull the plug on it! This project will be a quick comparison of some alternatives that can be run locally. I'm thinking of [Huginn](https://github.com/huginn/huginn), [Node-Red](https://nodered.org/), [n8n.io](https://n8n.io/), [Beehive](https://github.com/muesli/beehive).... any others?

## Huginn

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
