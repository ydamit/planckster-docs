Data Scrapers Guide
===================

This guide covers three data scrapers: **Telegram Scraper**, **Twitter Scraper**, and **Sentinel Scraper**. Each section includes setup, configuration, usage, and examples.

Telegram Scraper
----------------

The Telegram Scraper allows you to extract messages from specific Telegram channels using the Telethon Python package. This section provides detailed instructions on setting up and running the scraper.

### Setup and Configuration

#### Required Arguments

1.  **Kernel-Planckster-specific arguments**:

    -   `job_id`: A unique identifier for the job.
    -   `tracer_id`: Used to trace the job.
    -   `kp_host`: Kernel Planckster host address.
    -   `kp_port`: Kernel Planckster port number.
    -   `kp_auth_token`: Authentication token for Kernel Planckster.
    -   `kp_scheme`: Connection scheme (either `http` or `https`).
    -   `log_level`: The level of logging information to capture.
2.  **Telegram API Configuration**:

    -   `telegram_api_id`: Your Telegram API ID.
    -   `telegram_api_hash`: Your Telegram API Hash.
    -   `telegram_phone_number`: The phone number linked to your Telegram account (optional).
    -   `telegram_password`: Password for the Telegram account (optional).
    -   `telegram_bot_token`: Telegram bot token (optional).
    -   `channel_name`: Name of the Telegram channel to scrape.

### How to Configure the Telegram Client

To use the Telegram Scraper, you'll need to set up a Telegram client. Here's how:

1.  **Install the Telethon Package**: Make sure you have the Telethon package installed. You can install it via pip:


    `pip install telethon`

2.  **Define the Client Setup Function**: Create a function to set up the Telegram client. This will include passing necessary authentication details:

   
    ``` python
    from telethon import TelegramClient

    def get_scraping_client(job_id, logger, telegram_api_id, telegram_api_hashtelegram_phone_number=None, telegram_password=None, telegram_bot_token=None)-> TelegramClient:
        client = TelegramClient('session_name', telegram_api_id, telegram_api_hash)
        client.start(phone=telegram_phone_number, password=telegram_password)
        return client

    ```

### How to Retrieve Messages

Once the client is configured, you can retrieve messages from a specified channel using the `scrape` function.

#### Function: `scrape`

-   **Purpose**: Scrapes messages from a specified Telegram channel.
-   **Arguments**:
    -   `job_id`: Unique job identifier.
    -   `channel_name`: Telegram channel name to scrape.
    -   `tracer_id`: Job trace identifier.
    -   `scraped_data_repository`: Repository to store the scraped data.
    -   `telegram_client`: Configured `TelegramClient` instance.
    -   `openai_api_key`: OpenAI API key (if needed for processing).
    -   `log_level`: Logging level.


``` python
async def scrape(job_id, channel_name, tracer_id, scraped_data_repository, telegram_client, openai_api_key, log_level) -> JobOutput:
    with telegram_client:
        messages = await telegram_client.get_messages(channel_name, limit=100)
        for message in messages:
            # Process each message as needed
            print(message.text)
    return JobOutput(state='FINISHED', data=messages)

```

### Augmentation and Utility Functions

-   **Augmenting Data**: You can enhance the scraped messages by integrating data from other sources. For instance:

    ``` python
    def augment_telegram(client, message, filter):
        augmented_data = []
        # Example augmentation logic
        if filter in message.text:
            augmented_data.append(message.text)
        return augmented_data if augmented_data else None
    ```
    
-   **Utility Functions**: Includes logging, error handling, job state management, and cleanup tasks.

### Example: Running the Telegram Scraper Locally

Here is an example demonstrating how to use the Telegram scraper:
## Step1
#### Set Up Kernel-Planckster

First, clone the Kernel-Planckster repository onto your local machine:


`git clone https://github.com/dream-aim-deliver/kernel-planckster.git`

#### Install Dependencies

Set up a virtual environment for Kernel-Planckster's dependencies.
Run in the root directory of the kernel-planckster folder

``` python
python3 -m venv .venv
source .venv/bin/activate
```

Kernel-Planckster uses `poetry` for dependency management:



`pip install poetry
poetry install`

> **Note:** If `poetry` is unable to install `psycopg2`, use your system package manager to install it.

#### Running Kernel-Planckster

Run Kernel-Planckster from the command line:

`poetry run dev:storage`

#### Access Kernel-Planckster's API and Object Storage

-   **API:** Accessible at ``` http://localhost:8000/ ``` with an interactive interface at ``` http://localhost:8000/docs ``` for testing and debugging.
-   **Object Storage:** Accessible via the MinIO server at ``` http://localhost:9001/``` (or sometimes  ``` http://localhost:9091/ ```). The "SDA" bucket is created when a scraper is run.

## Step 2

``` python
import logging
from telethon import TelegramClient

# Set up logging
logger = logging.getLogger('telegram_scraper')
logging.basicConfig(level=logging.INFO)

# Define arguments
job_id = 123
tracer_id = "abc123"
telegram_api_id = "your_api_id"
telegram_api_hash = "your_api_hash"
telegram_phone_number = "your_phone_number"
telegram_password = "your_password"
channel_name = "your_channel_name"
log_level = logging.INFO

# Set up the Telegram client
client = get_scraping_client(job_id, logger, telegram_api_id, telegram_api_hash, telegram_phone_number, telegram_password)

# Run the scraper
output = scrape(job_id, channel_name, tracer_id, None, client, None, log_level)

# Output the result
print(output)
```



Twitter Scraper
---------------

The Twitter Scraper is designed to collect tweets based on search queries, allowing you to specify date ranges and filter results. This section provides a step-by-step guide to using it effectively.

### Setup and Configuration

#### Required Arguments

1.  **Kernel-Planckster-specific arguments**:

    -   `job_id`: Unique job identifier.
    -   `tracer_id`: Job trace identifier.
    -   `kp_host`: Kernel Planckster host address.
    -   `kp_port`: Kernel Planckster port number.
    -   `kp_auth_token`: Authentication token for Kernel Planckster.
    -   `kp_scheme`: Connection scheme (`http` or `https`).
    -   `log_level`: Logging level.
2.  **Twitter API Configuration**:

    -   `query`: The search query to use for retrieving tweets.
    -   `start_date`: The start date for the search in `YYYY-MM-DD` format.
    -   `end_date`: The end date for the search in `YYYY-MM-DD` format.
    -   `scraper_api_key`: API key for the scraper service.
    -   `openai_api_key`: API key for OpenAI services (if needed).

### How to Configure the Twitter Scraper

To get started with the Twitter Scraper, follow these steps:

1.  **Install Required Libraries**: Make sure you have the necessary libraries installed. Depending on your implementation, you might need libraries such as `tweepy` or other HTTP clients.


    `pip install tweepy`

2.  **Define the Setup Function**: Create a function to set up the scraper environment:

    ``` python
    import tweepy

    def setup(job_id, logger, kp_auth_token, kp_host, kp_port, kp_scheme):
        # Setup logic goes here
        client = tweepy.Client(bearer_token=kp_auth_token)
        return client
    ```

### How to Retrieve Tweets

The `scrape` function retrieves tweets based on the provided search query and date range.

#### Function: `scrape`

-   **Purpose**: Extracts tweets matching a specific query and date range.
-   **Arguments**:
    -   `job_id`: Unique job identifier.
    -   `tracer_id`: Job trace identifier.
    -   `query`: The search query for Twitter.
    -   `start_date`: Start date in `YYYY-MM-DD` format.
    -   `end_date`: End date in `YYYY-MM-DD` format.
    -   `scraped_data_repository`: Repository to store scraped data.
    -   `log_level`: Logging level.
    -   `scraper_api_key`: API key for the scraper.
    -   `openai_api_key`: API key for OpenAI (if needed).


``` python
def scrape(job_id, tracer_id, query, start_date, end_date, scraped_data_repository, log_level, scraper_api_key, openai_api_key):
    client = setup(job_id, logger, scraper_api_key, None, None, None)
    tweets = client.search_recent_tweets(query=query, start_time=start_date, end_time=end_date, max_results=100)
    for tweet in tweets.data:
        print(tweet.text)
    return JobOutput(state='FINISHED', data=tweets.data)
```

### Augmentation and Utility Functions

-   **Augmenting Data**: Enhance the retrieved tweets by incorporating additional data or insights:

    ``` python
    def augment_tweet(client, tweet, filter):
        if filter in tweet.text:
            return tweet
        return None
    ```


-   **Utility Functions**: These include logging, error handling, job state management, and saving/loading tweets.

### Example: Running the Twitter Scraper Locally

Here's an example to demonstrate how to use the Twitter scraper:

## Step1
#### Set Up Kernel-Planckster

First, clone the Kernel-Planckster repository onto your local machine:


`git clone https://github.com/dream-aim-deliver/kernel-planckster.git`

#### Install Dependencies

Set up a virtual environment for Kernel-Planckster's dependencies.
Run in the root directory of the kernel-planckster folder

``` python
python3 -m venv .venv
source .venv/bin/activate
```

Kernel-Planckster uses `poetry` for dependency management:



`pip install poetry
poetry install`

> **Note:** If `poetry` is unable to install `psycopg2`, use your system package manager to install it.

#### Running Kernel-Planckster

Run Kernel-Planckster from the command line:

`poetry run dev:storage`

#### Access Kernel-Planckster's API and Object Storage

-   **API:** Accessible at ``` http://localhost:8000/``` with an interactive interface at ```http://localhost:8000/docs``` for testing and debugging.
-   **Object Storage:** Accessible via the MinIO server at ``` http://localhost:9001/``` (or sometimes ``` http://localhost:9091/ ```). The "SDA" bucket is created when a scraper is run.


## Step2
``` python
import logging

# Set up logging
logger = logging.getLogger('twitter_scraper')
logging.basicConfig(level=logging.INFO)

# Define arguments
job_id = 123
tracer_id = "abc123"
query = "forest fire"
start_date = "2023-01-01"
end_date = "2023-01-31"
log_level = logging.INFO
scraper_api_key = "your_scraper_api_key"

# Run the scraper
output = scrape(job_id, tracer_id, query, start_date, end_date, None, log_level, scraper_api_key, None)

# Output the result
print(output)

```


Sentinel Scraper
----------------

The Sentinel Scraper is used to retrieve satellite images from the Sentinel Hub API. It provides options to specify geographic bounding boxes, date ranges, and resolution settings.

### Setup and Configuration

#### Required Arguments

1.  **Kernel-Planckster-specific arguments**:

    -   `job_id`: Unique job identifier.
    -   `tracer_id`: Job trace identifier.
    -   `kp_host`: Kernel Planckster host address.
    -   `kp_port`: Kernel Planckster port number.
    -   `kp_auth_token`: Authentication token for Kernel Planckster.
    -   `kp_scheme`: Connection scheme (`http` or `https`).
    -   `log_level`: Logging level.
2.  **Sentinel Hub Configuration**:

    -   `evalscript`: Sentinel Hub Evalscript to use.
    -   `bbox`: Bounding box of the area of interest.
    -   `resolution`: Spatial resolution in meters.
    -   `cloud_coverage`: Maximum allowable cloud coverage percentage.
    -   `start_date`: Start date for image acquisition in `YYYY-MM-DD` format.
    -   `end_date`: End date for image acquisition in `YYYY-MM-DD` format.
    -   `sh_client_id`: Sentinel Hub client ID.
    -   `sh_client_secret`: Sentinel Hub client secret.

### How to Configure the Sentinel Hub Client

To use the Sentinel Scraper, follow these steps:

1.  **Install Required Libraries**: Ensure that you have the necessary libraries installed, such as `sentinelhub` or `requests`.

    `pip install sentinelhub`

2.  **Define the Setup Function**: Create a function to set up the Sentinel Hub client:

    ``` python
    from sentinelhub import SHConfig, SentinelHubRequest

    def setup(job_id, logger, sh_client_id, sh_client_secret, evalscript, bbox, resolution, cloud_coverage):
        config = SHConfig()
        config.sh_client_id = sh_client_id
        config.sh_client_secret = sh_client_secret
        return config
    ```
### How to Retrieve Satellite Images

The `scrape` function retrieves satellite images from the Sentinel Hub based on the specified parameters.

#### Function: `scrape`

-   **Purpose**: Fetches satellite images from Sentinel Hub.
-   **Arguments**:
    -   `job_id`: Unique job identifier.
    -   `tracer_id`: Job trace identifier.
    -   `evalscript`: Sentinel Hub Evalscript.
    -   `bbox`: Bounding box for the area of interest.
    -   `resolution`: Spatial resolution in meters.
    -   `cloud_coverage`: Maximum cloud coverage percentage.
    -   `start_date`: Start date in `YYYY-MM-DD` format.
    -   `end_date`: End date in `YYYY-MM-DD` format.
    -   `scraped_data_repository`: Repository to store scraped data.
    -   `sh_client_id`: Sentinel Hub client ID.
    -   `sh_client_secret`: Sentinel Hub client secret.
    -   `openai_api_key`: API key for OpenAI (if needed).

``` python
def scrape(job_id, tracer_id, evalscript, bbox, resolution, cloud_coverage, start_date, end_date, scraped_data_repository, sh_client_id, sh_client_secret, openai_api_key):
    config = setup(job_id, logger, sh_client_id, sh_client_secret, evalscript, bbox, resolution, cloud_coverage)

    request = SentinelHubRequest(
        evalscript=evalscript,
        input_data=[SentinelHubRequest.input_data(data_source=DataSource.SENTINEL2_L1C, time_interval=(start_date, end_date))],
        responses=[SentinelHubRequest.output_response('default', MimeType.TIFF)],
        bbox=bbox,
        size=(resolution, resolution),
        config=config
    )

    images = request.get_data()
    # Process images as needed
    return JobOutput(state='FINISHED', data=images)
```

### Augmentation and Utility Functions

-   **Augmenting Data**: You can enhance satellite images by applying image processing techniques:

    ``` python
    def augment_image(image, filter):
        # Apply augmentation logic
        return processed_image
    ```

-   **Utility Functions**: Includes logging, error handling, job state management, and saving/loading images.

### Example: Running the Sentinel Scraper Locally

Here's an example to demonstrate how to use the Sentinel scraper:

## Step1
#### Set Up Kernel-Planckster

First, clone the Kernel-Planckster repository onto your local machine:


`git clone https://github.com/dream-aim-deliver/kernel-planckster.git`

#### Install Dependencies

Set up a virtual environment for Kernel-Planckster's dependencies.
Run in the root directory of the kernel-planckster folder

``` python
python3 -m venv .venv
source .venv/bin/activate
```

Kernel-Planckster uses `poetry` for dependency management:



`pip install poetry
poetry install`

> **Note:** If `poetry` is unable to install `psycopg2`, use your system package manager to install it.

#### Running Kernel-Planckster

Run Kernel-Planckster from the command line:

`poetry run dev:storage`

#### Access Kernel-Planckster's API and Object Storage

-   **API:** Accessible at ``` http://localhost:8000/ ``` with an interactive interface at ``` http://localhost:8000/docs ``` for testing and debugging.
-   **Object Storage:** Accessible via the MinIO server at ``` http://localhost:9001/ ``` (or sometimes ``` http://localhost:9091/ ```). The "SDA" bucket is created when a scraper is run.


## Step 2
``` python
import logging

# Set up logging
logger = logging.getLogger('sentinel_scraper')
logging.basicConfig(level=logging.INFO)

# Define arguments
job_id = 123
tracer_id = "abc123"
evalscript = "your_evalscript"
bbox = (13.822, 45.850, 13.928, 45.992)
resolution = 100
cloud_coverage = 20
start_date = "2023-01-01"
end_date = "2023-01-31"
log_level = logging.INFO
sh_client_id = "your_sh_client_id"
sh_client_secret = "your_sh_client_secret"

# Run the scraper
output = scrape(job_id, tracer_id, evalscript, bbox, resolution, cloud_coverage, start_date, end_date, None, sh_client_id, sh_client_secret, None)

# Output the result
print(output)
```

Conclusion
----------
This comprehensive guide provides detailed instructions on setting up and using Telegram, Twitter, and Sentinel scrapers. By following these steps, you can effectively gather data from these platforms for your specific needs. Ensure you have all necessary credentials and configurations set up before running each scraper.