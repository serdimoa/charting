The Charting Library supports saving/loading charts and study templates using 2 levels of API:

1. **Low-Level**: save/load functionality is enabled by widget's `save()` / `load()` [methods](Widget-Methods#savecallback) and `createStudyTemplate()` / `applyStudyTemplate()` [methods](Chart-Methods#createstudytemplateoptions).
    One should take of the storage on the server.
    You are able to save the JSONs where you wish. For example, you may embed them to your saved pages or user's working area etc.

1. **High-Level**: Charting Library is able to save / load charts and study templates from the storage that you'll point it to.
    We created a tiny storage sample with Python and PostgreSQL that can be found in [our GitHub](https://github.com/tradingview/saveload_backend).
    You can use it and run on your own server so that you'll be able to have control over all your users' saved data.

## Using High-Level Save/Load

Here are a few steps for those who want to have their own chart storage:

1. Clone our repository to your host
1. Run the data service or use our demo service. Here is a short to-do list for anyone who is not familiar with Django.
    1. Install Python 3.x and Pip.
    1. Install PostgreSQL or some other Django-friendly database engine.
    1. Go to you chart storage folder and run `pip install -r requirements.txt`
    1. Go to charting_library_charts folder and set up your database connection in settings.py (see `DATABASES` @ line #12). Please remember to create the appropriate database in your PostgreSQL.
    1. Run `python manage.py migrate` . This will create the database schema without any data.
    1. Run `python manage.py runserver` to run a TEST instance of your database. Don't use the command above in production environment. Use some other program (i.e., Gunicorn).
1. Set up your Charting Library page: set `charts_storage_url = url-of-your-charts-storage`, also set `client_id` and `user_id` (see details below) in the widget constructor.
1. Enjoy!

**Remark**: Manual database filling/editing is not the intended usage. Please avoid doing this as you you may hurt the Django infrastructure.

## Developing your own backend

* Charting Library sends HTTP/HTTPS commands to `charts_storage_url/charts_storage_api_version/charts?client=client_id&user=user_id`. `charts_storage_url`, `charts_storage_api_version`, `client_id` and `user_id` are the arguments of the [widget constructor](Widget-Constructor).
* You should implement the processing of 4 requests: save chart / load chart / delete chart / list charts.

### LIST CHARTS

GET REQUEST: `charts_storage_url/charts_storage_api_version/charts?client=client_id&user=user_id`

RESPONSE: JSON Object

1. `status`: `ok` or `error`
1. `data`: Array of Objects
    1. `timestamp`: UNIX time when the chart was saved (example, `144908432`1)
    1. `symbol`: base symbol of the chart (example, `AA`)
    1. `resolution`: resolution of the chart (example, `D`)
    1. `id`: unique integer identifier of the chart (example, `9163`)
    1. `name`: chart name (example, `Test`)

### SAVE CHART

POST REQUEST: `charts_storage_url/charts_storage_api_version/charts?client=client_id&user=user_id`

1. `name`: name of the chart
1. `content`: content of the chart
1. `symbol`: chart symbol (example, `AA`)
1. `resolution`: chart resolution (example, `D`)

RESPONSE: JSON Object

1. `status`: `ok` or `error`
1. `id`: unique integer identifier of the chart (example, `9163`)

### SAVE AS CHART

POST REQUEST: `charts_storage_url/charts_storage_api_version/charts?client=client_id&user=user_id&chart=chart_id`

1. `name`: name of the chart
1. `content`: content of the chart
1. `symbol`: chart symbol (example, `AA`)
1. `resolution`: chart resolution (example, `D`)

RESPONSE: JSON Object

1. `status`: `ok` or `error`

### LOAD CHART

GET REQUEST: `charts_storage_url/charts_storage_api_version/charts?client=client_id&user=user_id&chart=chart_id`

RESPONSE: JSON Object

1. `status`: `ok` or `error`
1. `data`: Object
    1. `content`: saved content of the chart
    1. `timestamp`: UNIX time when the chart was saved (example, `1449084321`)
    1. `id`: unique integer identifier of the chart (example, `9163`)
    1. `name`: name of the chart

### DELETE CHART

DELETE REQUEST: `charts_storage_url/charts_storage_api_version/charts?client=client_id&user=user_id&chart=chart_id`

RESPONSE: JSON Object

1. `status`: `ok` or `error`

## Using Demo Charts and Study Templates Storage

We're running a demo chart storage service to let you save/load charts as soon as you build your Charting Library.
Here is the link <http://saveload.tradingview.com>. Note that it's provided as-is since it's a demo.

We do not guarantee its stability. Also, note that we delete the data in the storage on a regular basis.

## Managing Access to Saved Charts

You are responsible for the charts that your users are able to see and load.
A user can see/load charts that have the same `client_id` and `user_id` that the user has.
`client_id` is an identifier of user's group.
The intended use is when you have a few groups of users or when you have a few sites that use the same chart storage.
So the common practice is to set `client_id = your-site's-URL`. It's up to you to decide.

`user_id` is a unique identifier of a user. Users ID belongs to a particular `client_id` group.
You can either set it for each user individually (private storage for each user) or set it for all users or user groups (public storage).

Here are a few examples:

`client_id`|`user_id`|Effect
---|---|---
Your site URL or other link|Unique user ID|Each user has a private chart storage that other users can't see.
Your site URL or other link|The same value for all users|Each user can see and load any saved chart.
Your site URL or other link|Unique user ID for registered users along with a separate setting for anonymous users|Each registered user has a private chart storage that other users can't see. All anonymous users share a single storage.
