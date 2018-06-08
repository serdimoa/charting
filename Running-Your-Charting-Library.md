The Charting Library works out of the box, but you have to set up your HTTP server to bind Library’s folder to a certain domain name.

# Let’s start the Charting Library on your local computer

## 1. Clone this repository

[Cloning a repository](https://help.github.com/articles/cloning-a-repository/)

## 2. Start an HTTP server

You may set up any HTTP server to listen to any of your host’s free ports and refer to the folder containing the Charting Library. Below you will find instructions on how to start several most common HTTP servers.

### Python 2.x

If you have Python installed on your computer then you can use it to start an HTTP server in a quick manner.

Execute the following command in the Charting Library folder:

`python -m SimpleHTTPServer 9090`

### NodeJS

First install `http-server`:

`npm install http-server -g`

Start `http-server` using the following command in the Charting Library folder:

`http-server -p 9090`

### NGINX

- Install NGINX
- Open the config file `nginx.conf` and write the following text in the `http` section of the file:

    ```javascript
    server {
        listen       9090;
        server_name  localhost;

        location / {
            root ABSOLUTE_PATH_TO_THE_CHARTING_LIBRARY;
        }
    }
    ```

- Replace `ABSOLUTE_PATH_TO_THE_CHARTING_LIBRARY` with the absolute path to the charting library folder.
- Run `nginx`

## 3. Open the Charting Library in your browser

Once you are done running the HTTP server, you will have your Charting Library ready.

Open `http://127.0.0.1:9090` in your browser.

# Online Example

You may see the working example of Charting Library [here](http://demo_chart.tradingview.com). This example is based on our [sample data feed.](http://demo_feed.tradingview.com)

**IMPORTANT**: This datafeed is just a sample. It contains a dozen of symbols (from Quandl) and provides DWM only. It does however support quotes. Please use it for testing purposes only.
