---
sidebar_label: Data Scrapers
sidebar_position: 3
---

# Telegram Scraper
### Arguments & Inputs

#### Kernel-Planckster-specific arguments:
* `job_id`: Unique identifier for the job.
* `tracer_id`: Identifier for tracing the job.
* `kp_host`: Kernel Planckster host.
* `kp_port`: Kernel Planckster port.
* `kp_auth_token`: Kernel Planckster authentication token.
* `kp_scheme`: Kernel Planckster scheme (http/https).
* `log_level`: Logging level for the scraper.

#### Scraper-specific arguments:

##### Telegram API Configuration:
* `telegram_api_id`: Telegram API ID.
* `telegram_api_hash`: Telegram API Hash.
* `telegram_phone_number`: Phone number linked to the Telegram account (optional).
* `telegram_password`: Password for the Telegram account (optional).
* `telegram_bot_token`: Telegram bot token (optional).
* `channel_name`: Name of the Telegram channel to scrape.

### Telegram & Scraping

#### Configuration
* `get_scraping_client(job_id, logger, telegram_api_id, telegram_api_hash, telegram_phone_number=None, telegram_password=None, telegram_bot_token=None) -> TelegramClient`: 
  * This function sets up the Telegram client (using Telethon python package).
  * **Arguments:**
    * `job_id`: Unique identifier for the job.
    * `logger`: Logger instance.
    * `telegram_api_id`: Telegram API ID.
    * `telegram_api_hash`: Telegram API Hash.
    * `telegram_phone_number`: Phone number linked to the Telegram account (optional).
    * `telegram_password`: Password for the Telegram account (optional).
    * `telegram_bot_token`: Telegram bot token (optional).
  * **Returns:**
    * A configured `TelegramClient` instance.

#### Retrieving Messages
* `scrape(job_id, channel_name, tracer_id, scraped_data_repository, telegram_client, openai_api_key, log_level) -> JobOutput`:
  * This function takes as input the arguments required to scrape the desired data using the Telegram Scraper API.
  * **Arguments:**
    * `job_id`: Unique identifier for the job.
    * `channel_name`: Name of the Telegram channel.
    * `tracer_id`: Identifier for tracing the job.
    * `scraped_data_repository`: Repository to store scraped data.
    * `telegram_client`: Configured `TelegramClient` instance.
    * `openai_api_key`: API key for OpenAI services.
    * `log_level`: Logging level.
  * **Returns:**
    * A `JobOutput` object with the job state and list of source data.

## Augmentation

* `augment_telegram(client, message, filter)`:
  * Once the text messages are extracted, this function will handle the augmentation of this data with other data sources for example data scraped through Sentinel API.
  * **Arguments:**
    * `client`: Configured Instructor client.
    * `message`: Telegram message to be processed.
    * `filter`: Filter string for message relevance.
  * **Returns:**
    * A list of augmented data if relevant, otherwise `None`.

* `get_lat_long(location_name)`:
  * Retrieves latitude and longitude for a given location name using geolocation.
  * **Arguments:**
    * `location_name`: Name of the location to retrieve coordinates for.
  * **Returns:**
    * Latitude and longitude coordinates.    

## Utility Functions

1. **Extensive Logging and Error Handling**:
   * The scraper includes in-built error handling mechanisms (`try-except` blocks) and logs various stages of the job execution.

2. **Job State Management**:
   * The job state is tracked and updated through various stages (`BaseJobState.CREATED`, `BaseJobState.RUNNING`, `BaseJobState.FINISHED`, `BaseJobState.FAILED`).

3. **Cleanup**:
   * After processing, temporary directories where media files are saved are cleaned up to free up storage.


## How to Run Locally

To run the Telegram scraper locally, follow the instructions in the [Local Setup Guide](#).

## Example

```python
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
telegram_bot_token = "your_bot_token"
channel_name = "your_channel_name"
openai_api_key = "your_openai_api_key"
log_level = logging.INFO

# Set up the Telegram client
client = get_scraping_client(job_id, logger, telegram_api_id, telegram_api_hash, telegram_phone_number, telegram_password, telegram_bot_token)

# Create a ScrapedDataRepository instance (implement as needed)
scraped_data_repository = ScrapedDataRepository()

# Run the scraper
output = await scrape(job_id, channel_name, tracer_id, scraped_data_repository, client, openai_api_key, log_level)

# Output the result
print(output)
```

# Twitter Scraper
### Arguments & Inputs

#### Kernel-Planckster-specific arguments:
* `job_id`: Unique identifier for the job.
* `tracer_id`: Identifier for tracing the job.
* `kp_host`: Kernel Planckster host.
* `kp_port`: Kernel Planckster port.
* `kp_auth_token`: Kernel Planckster authentication token.
* `kp_scheme`: Kernel Planckster scheme (http/https).
* `log_level`: Logging level for the scraper.

#### Scraper-specific arguments:

##### Twitter API Configuration:
* `query`: Search query for Twitter.
* `start_date`: Start date for the search (YYYY-MM-DD).
* `end_date`: End date for the search (YYYY-MM-DD).
* `scraper_api_key`: API key for the scraper service.
* `openai_api_key`: API key for OpenAI services.

### Twitter & Scraping

#### Configuration
* `setup(job_id, logger, kp_auth_token, kp_host, kp_port, kp_scheme) -> Tuple[KernelPlancksterGateway, ProtocolEnum, FileRepository]`: 
  * This function sets up the Kernel Planckster Gateway, the storage protocol, and the file repository.
  * **Arguments:**
    * `job_id`: Unique identifier for the job.
    * `logger`: Logger instance.
    * `kp_auth_token`: Kernel Planckster authentication token.
    * `kp_host`: Kernel Planckster host.
    * `kp_port`: Kernel Planckster port.
    * `kp_scheme`: Kernel Planckster scheme (http/https).
  * **Returns:**
    * A tuple containing the Kernel Planckster Gateway, storage protocol, and file repository.

#### Retrieving Tweets
* `scrape(job_id, tracer_id, query, start_date, end_date, scraped_data_repository, work_dir, log_level, scraper_api_key, openai_api_key) -> JobOutput`:
  * This function takes as input the arguments required to scrape the desired data using the Twitter Scraper API.
  * **Arguments:**
    * `job_id`: Unique identifier for the job.
    * `tracer_id`: Identifier for tracing the job.
    * `query`: Search query for Twitter.
    * `start_date`: Start date for the search (YYYY-MM-DD).
    * `end_date`: End date for the search (YYYY-MM-DD).
    * `scraped_data_repository`: Repository to store scraped data.
    * `work_dir`: Directory for temporary work files.
    * `log_level`: Logging level.
    * `scraper_api_key`: API key for the scraper service.
    * `openai_api_key`: API key for OpenAI services.
  * **Returns:**
    * A `JobOutput` object with the job state and list of source data.

## Augmentation

* `augment_tweet(client, tweet, filter)`:
  * Once the tweets are extracted, this function will handle the augmentation of this data with other data sources for example data scraped through Sentinel API.
  * **Arguments:**
    * `client`: Configured Instructor client.
    * `tweet`: Tweet data to be processed.
    * `filter`: Filter string for tweet relevance.
  * **Returns:**
    * A list of augmented data if relevant, otherwise `None`.

  * `get_lat_long(location_name)`:
  * Retrieves latitude and longitude for a given location name using geolocation.
  * **Arguments:**
    * `location_name`: Name of the location to retrieve coordinates for.
  * **Returns:**
    * Latitude and longitude coordinates.  

## Utility Functions

1. **Extensive Logging and Error Handling**:
   * The scraper includes in-built error handling mechanisms (`try-except` blocks) and logs various stages of the job execution.

2. **Job State Management**:
   * The job state is tracked and updated through various stages (`BaseJobState.CREATED`, `BaseJobState.RUNNING`, `BaseJobState.FINISHED`, `BaseJobState.FAILED`).

3. **Cleanup**:
   * After processing, temporary directories where media files are saved are cleaned up to free up storage.

* `save_tweets(tweets, file_path)`:
  * Saves tweets to the specified file path in JSON format.
  * **Arguments:**
    * `tweets`: List of tweets to be saved.
    * `file_path`: Path to save the tweets.
    
* `load_tweets(file_path)`:
  * Loads tweets from the specified file path.
  * **Arguments:**
    * `file_path`: Path to load the tweets from.
  * **Returns:**
    * Loaded tweet data.


## How to Run Locally

To run the Twitter scraper locally, follow the instructions in the [Local Setup Guide](#).

## Example

```python
import logging
from app.sdk.scraped_data_repository import ScrapedDataRepository

# Set up logging
logger = logging.getLogger('twitter_scraper')
logging.basicConfig(level=logging.INFO)

# Define arguments
job_id = 123
tracer_id = "abc123"
query = "forest fire"
start_date = "2023-01-01"
end_date = "2023-01-31"
work_dir = "/path/to/work/dir"
log_level = logging.INFO
scraper_api_key = "your_scraper_api_key"
openai_api_key = "your_openai_api_key"

# Create a ScrapedDataRepository instance (implement as needed)
scraped_data_repository = ScrapedDataRepository()

# Run the scraper
output = scrape(job_id, tracer_id, query, start_date, end_date, scraped_data_repository, work_dir, log_level, scraper_api_key, openai_api_key)

# Output the result
print(output)

```

# Sentinel Scraper Documentation

## Scraper

### Arguments & Inputs

#### Kernel-Planckster-specific arguments:
* `job_id`: Unique identifier for the job.
* `tracer_id`: Identifier for tracing the job.
* `kp_host`: Kernel Planckster host.
* `kp_port`: Kernel Planckster port.
* `kp_auth_token`: Kernel Planckster authentication token.
* `kp_scheme`: Kernel Planckster scheme (http/https).
* `log_level`: Logging level for the scraper.

#### Scraper-specific arguments:

##### Define the WGS84 coordinate bounding box; float
* `long_left`: Leftmost longitude, e.g., -113.2
* `lat_down`: Bottommost latitude, e.g., 57.2
* `long_right`: Rightmost longitude, e.g., -108.5
* `lat_up`: Topmost latitude, e.g., 59.3

##### Selecting the date range: "YYYY-MM-DD" format
* `start_date`: Day to begin scanning, e.g., "2023-06-01"
* `end_date`: Day to finish scanning, e.g., "2023-06-20"
*Note:* Depending on the specific satellite (e.g., Landsat, Sentinel, etc.), the temporal resolution varies. It’s unlikely to get a new image every day in a region, so using large timeframes and multiple satellites is the best way to receive coverage.

##### Spatial Resolution: integer value (meters)
* `resolution`: Meters above the surface, e.g., 120
*Note:* Resolution must be greater than 10 meters and the final image cannot exceed 2500 x 2500 pixels. Thus, one can either have high resolution (fewer meters above the surface) and a small bounding box or low resolution (more meters above the surface) and a large bounding box.


### Sentinelhub & Scraping

#### Configuration
* `get_scraping_config(job_id, logger, sentinel_client_id, sentinel_client_secret) -> SHConfig`: 
  * This function sets up the Sentinel client configuration (using SentinelHub API).
  * **Arguments:**
    * `job_id`: Unique identifier for the job.
    * `logger`: Logger instance.
    * `sentinel_client_id`: SentinelHub API Client ID.
    * `sentinel_client_secret`: SentinelHub API Client Secret.
  * **Returns:**
    * A configured `SHConfig` instance.

#### Retrieving Images
* `get_images(logger, job_id, tracer_id, scraped_data_repository, output_data_list, protocol, coords_wgs84, evalscript_true_color, config, start_date, end_date, resolution, image_dir) -> list[KernelPlancksterSourceData]`:
  * This function takes as input the arguments required to scrape the desired data using the Sentinel API.
  * **Arguments:**
    * `logger`: Logger instance.
    * `job_id`: Unique identifier for the job.
    * `tracer_id`: Identifier for tracing the job.
    * `scraped_data_repository`: Repository to store scraped data.
    * `output_data_list`: List to store output data.
    * `protocol`: Storage protocol.
    * `coords_wgs84`: Tuple of coordinates (long_left, lat_down, long_right, lat_up).
    * `evalscript_true_color`: Evalscript for Sentinel Hub request.
    * `config`: Sentinel Hub configuration object.
    * `start_date`: Start date for image retrieval.
    * `end_date`: End date for image retrieval.
    * `resolution`: Spatial resolution in meters.
    * `image_dir`: Directory to save images.
  * **Returns:**
    * A list of `KernelPlancksterSourceData` objects.

## Augmentation

* `augment_images(logger, job_id, tracer_id, scraped_data_repository, output_data_list, protocol, coords_wgs84, image_dir) -> list[KernelPlancksterSourceData]`:
  * This function processes the images to detect certain features, such as forest fires.
  * **Arguments:**
    * `logger`: Logger instance.
    * `job_id`: Unique identifier for the job.
    * `tracer_id`: Identifier for tracing the job.
    * `scraped_data_repository`: Repository to store scraped data.
    * `output_data_list`: List to store output data.
    * `protocol`: Storage protocol.
    * `coords_wgs84`: Tuple of coordinates (long_left, lat_down, long_right, lat_up).
    * `image_dir`: Directory where images are saved.
  * **Returns:**
    * A list of `KernelPlancksterSourceData` objects.

## Utility Functions

1. **Extensive Logging and Error Handling**:
   * The scraper includes in-built error handling mechanisms (`try-except` blocks) and logs various stages of the job execution.

2. **Job State Management**:
   * The job state is tracked and updated through various stages (`BaseJobState.CREATED`, `BaseJobState.RUNNING`, `BaseJobState.FINISHED`, `BaseJobState.FAILED`).

3. **Cleanup**:
   * After processing, temporary directories where images are saved are cleaned up to free up storage.

## How to Run Locally

To run the Sentinel scraper locally, follow the instructions in the [Local Setup Guide](#).

## Example

```python
import logging
from sentinelhub import SHConfig
from app.sdk.scraped_data_repository import ScrapedDataRepository

# Set up logging
logger = logging.getLogger('sentinel_scraper')
logging.basicConfig(level=logging.INFO)

# Define arguments
job_id = 123
tracer_id = "abc123"
long_left = -113.2
lat_down = 57.2
long_right = -108.5
lat_up = 59.3
start_date = "2023-06-01"
end_date = "2023-06-20"
resolution = 120
image_dir = "/path/to/image_dir"
log_level = logging.INFO
sentinel_client_id = "your_sentinel_client_id"
sentinel_client_secret = "your_sentinel_client_secret"

# Set up the Sentinel client
config = get_scraping_config(job_id, logger, sentinel_client_id, sentinel_client_secret)

# Create a ScrapedDataRepository instance (implement as needed)
scraped_data_repository = ScrapedDataRepository()

# Run the scraper
output = scrape(job_id, tracer_id, scraped_data_repository, log_level, long_left, lat_down, long_right, lat_up, config, start_date, end_date, image_dir, resolution)

# Output the result
print(output)
```