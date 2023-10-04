# API Client Example

Below are some basic examples of how to get started writing an API client
for the [LightFi API](https://apiv2.lightfi.io/docs) using
[python](/API/client_example/#python-example-api-client)
or [typescript](/API/client_example/#typescript-example-api-client).

## Python Example API Client
### Install OpenAPI generator

See instructions from [OpenAPITools](https://github.com/OpenAPITools/openapi-generator/)

### Generate a client

An API client for python can be generated using the following example command:

```openapi-generator-cli generate -i https://apiv2.lightfi.io/openapi.json -g python --additional-properties=floatStrictType=false -o python_api_client```

After generating the API client, see the generated documentation for more info at `python_api_client/README.md`

### Client script

#### Basic starting place
A basic example script to make an API call using python3

```python
import openapi_client
from openapi_client.rest import ApiException
from pprint import pprint

configuration = openapi_client.Configuration(
    host = "https://apiv2.lightfi.io"
)

configuration.access_token = "YOUR_ACCESS_TOKEN"


# Enter a context with an instance of the API client
with openapi_client.ApiClient(configuration) as api_client:
    # Create an instance of the API class
    api_instance = openapi_client.DefaultApi(api_client)
    try:
        api_response = api_instance.read_base_location_properties_locations_get()
        print("The response of DefaultApi->read_base_location_properties_locations_get:\n")
        pprint(api_response)
    except ApiException as e:
        print("Exception when calling DefaultApi->read_base_location_properties_locations_get: %s\n" % e)

```

#### Access token expiry checking

You can use a function like the following to check whether your access token is expired and needs refreshing

```python
import jwt  # pyJWT
from time import time
def check_token_expired(token: str):
    unverified_claims = jwt.decode(token, options={"verify_signature": False})
    exp = unverified_claims.get('exp', 0)
    if exp > (time() + 10):  # 10 seconds margin
        return False  # Token is not expired
    return True  # Token expired or will expire in the next 10s
```


#### Using a refresh token

To handle obtaining a new `access_token` using your `refresh_token` (see [Obtaining an OAuth2 refresh token](/API/client_refresh_token/)) you can use code like the following:

```python
import httpx
import os

def refresh_oauth2_token(token_url: str, client_id: str, client_secret: str, refresh_token: str):
    data = {
        'grant_type': 'refresh_token',
        'client_id': client_id,
        'client_secret': client_secret,
        'refresh_token': refresh_token,
    }

    response = httpx.post(token_url, data=data)

    if response.status_code == 200:
        token_data = response.json()
        access_token = token_data['access_token']
        return access_token
    else:
        raise Exception(f"Failed to refresh OAuth2 token: {response.text}")

# Example usage, set the environment variables with your values:
token_url = "https://lightfiv2.auth.eu-west-2.amazoncognito.com/oauth2/token"
client_id = os.environ["CLIENT_ID"]
client_secret = os.environ["CLIENT_SECRET"]
refresh_token = os.environ["REFRESH_TOKEN"]

access_token = refresh_oauth2_token(token_url, client_id, client_secret, refresh_token)
```

#### Periodic API polling with automatic token refresh

Combining the token validation and refreshing mechanisms, we can design a wrapper that automatically manages token acquisition for a long-running client that periodically fetches data. In the following example, the client polls for a new Carbon Dioxide reading from a sensor every minute and logs any new readings to the console:

```python
import os
import time
from datetime import datetime
from typing import Callable, TypeVar

T = TypeVar("T")  # To allow linter to recognise return types from API wrapper

import httpx
import jwt  # pyJWT

import openapi_client
from openapi_client.models.var_name import VarName
from openapi_client.rest import ApiException


def check_access_token_expired(token: str | None) -> bool:
    if token is None:
        return True
    unverified_claims = jwt.decode(token, options={"verify_signature": False})
    exp = unverified_claims.get("exp", 0)
    if exp > (time.time() + 10):  # 10 seconds margin
        return False  # Token is not expired
    return True  # Token expired or will expire in the next 10s


def refresh_oauth2_token(token_url: str, client_id: str, client_secret: str, refresh_token: str):
    data = {
        "grant_type": "refresh_token",
        "client_id": client_id,
        "client_secret": client_secret,
        "refresh_token": refresh_token,
    }

    response = httpx.post(token_url, data=data)

    if response.status_code == 200:
        token_data = response.json()
        access_token = token_data["access_token"]
        return access_token
    else:
        raise Exception(f"Failed to refresh OAuth2 token: {response.text}")


class OAuthAPI:
    def __init__(
        self, host: str, token_url: str, client_id: str, client_secret: str, refresh_token: str
    ):
        self.configuration = openapi_client.Configuration(host=host)
        self.token_url = token_url
        self.client_id = client_id
        self.client_secret = client_secret
        self.refresh_token = refresh_token
        self.access_token = None

    def refresh_access_token(self):
        self.access_token = refresh_oauth2_token(
            self.token_url, self.client_id, self.client_secret, self.refresh_token
        )
        self.configuration.access_token = self.access_token

    def call(self, api_call: Callable[..., T], *args, **kwargs) -> T:
        if check_access_token_expired(self.access_token):
            self.refresh_access_token()

        try:
            with openapi_client.ApiClient(self.configuration) as api_client:
                return api_call(api_client, *args, **kwargs)
        except ApiException as e:
            if e.status == 401:  # Unauthorized, access token might have expired
                self.refresh_access_token()
                with openapi_client.ApiClient(self.configuration) as api_client:
                    return api_call(api_client, *args, **kwargs)
            else:
                raise e


# Initialize OAuthAPI instance
host = "https://apiv2.lightfi.io"
token_url = "https://lightfiv2.auth.eu-west-2.amazoncognito.com/oauth2/token"
client_id = os.environ["CLIENT_ID"]
client_secret = os.environ["CLIENT_SECRET"]
refresh_token = os.environ["REFRESH_TOKEN"]  # Remember to keep token up to date as required for your client settings
api = OAuthAPI(host, token_url, client_id, client_secret, refresh_token)


def get_sensor_latest_data(api_client, sensor_id):
    api_instance = openapi_client.DefaultApi(api_client)
    return api_instance.read_latest_sensor_information_sensors_sensor_id_get(
        sensor_id, project=["latest", "value"]
    )


sensor_id = "AQ2-FE753C0D7ADF"

timestamp, timestamp_last = None, None
value = None
while True:
    api_response = api.call(get_sensor_latest_data, sensor_id)
    if api_response.data is not None:
        for datum in api_response.data:
            if datum.var_name == VarName.CO2PPM:
                if datum.time is not None and datum.value is not None:
                    timestamp = datetime.fromtimestamp(datum.time).strftime("%Y-%m-%d %H:%M:%S")
                    value = datum.value
    if timestamp != timestamp_last:
        print(f"{timestamp} CO2 level: {value} ppm")
        timestamp_last = timestamp
    time.sleep(60)
```

## Typescript Example API Client

### Install OpenAPI generator

See instructions from [OpenAPITools](https://github.com/OpenAPITools/openapi-generator/)

### Generate a client

An API client for typescript can be generated using the following example command:

` openapi-generator-cli generate -i https://apiv2.lightfi.io/openapi.json --generator-name typescript-axios -o src/services/api`

### Client Script

#### Necessary variables

```typescript
API_URL= https://apiv2.lightfi.io
API_AUTH_TOKEN_URL= https://lightfiv2.auth.eu-west-2.amazoncognito.com/oauth2/token
API_TOKEN_CLIENT_ID= {{ To be accessed via administration }}
API_TOKEN_CLIENT_SECRET = {{{{ To be accessed via administration }}}}
API_REFRESH_TOKEN = {{ See documentation for how to obtain }}
```

#### Get Refresh Token

To obtain the refresh token see the documentation examples for Postman or python [here](/API/client_refresh_token).

#### Get Access Token

After generating refresh token. Access token can be generated using following function:

```typescript
const  getAccessToken = async () => {
	const  accessToken = await  fetch(process.env.API_AUTH_TOKEN_URL, {
		method:  "POST",
		headers: {
			"Content-Type":  "application/x-www-form-urlencoded",
		},
		body:  new  URLSearchParams({
			grant_type:  "refresh_token",
			client_id:  API_TOKEN_CLIENT_ID,
			client_secret:  API_TOKEN_CLIENT_SECRET,
			refresh_token:  AUTHORIZATION_REFRESH_TOKEN,
		}),
	})
		.then((response) =>  response.json())
		.then((data) =>  data.access_token);
	return  accessToken;
};
```

[ Note: For continuously running application, ```accessToken``` expiry is also available within the response json. It can be stored as variable and generate accessToken only upon expiration]

Generated Access token can be finally used to create api config and perform API calls

```typescript
const  apiConfig = async () =>
	new  Configuration({
		basePath:  API_URL,
		accessToken:  await  getAccessToken(),
	});

const  apiService = async () =>  new  DefaultApi(await  apiConfig());
```

#### Example API Call

For instance to receive location properties from api call:

```typescript
 const getLocationProperties = async(locationId:string): Promise<LocationOutput> => {
	const apiServiceConfig = await apiService();
	const {data} = await apiServiceConfig
		.readLocationProperties(locationId)
		.catch((res) => throw new Error(res);
	return data;
}

```
