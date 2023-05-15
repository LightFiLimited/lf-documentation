# API Client Typescript and generate Access Token

## Install OpenAPI generator

See instructions from [OpenAPITools](https://github.com/OpenAPITools/openapi-generator/)

## Generate a client

An API client for typescript can be generated using the following example command:

` openapi-generator-cli generate -i https://apiv2.lightfi.io/openapi.json --generator-name typescript-axios -o src/services/api`

## Starting place

### Necessary variables

```
API_URL= https://apiv2.lightfi.io
API_AUTH_TOKEN_URL= https://lightfiv2.auth.eu-west-2.amazoncognito.com/oauth2/token
API_TOKEN_CLIENT_ID= {{ To be accessed via administration }}
API_TOKEN_CLIENT_SECRET = {{{{ To be accessed via administration }}}}
API_REFRESH_TOKEN = {{ To be generated from postman }}
```

### Get Refresh Token

With mentioned variables from above, `Refresh Token` can be generated using the postman guide.
See instructions from https://docs.lightfi.io/API/postman_refresh_token/

### Get Access Token

After generating refresh token. Access token can be generated using following function:

```
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

```
const  apiConfig = async () =>
	new  Configuration({
		basePath:  API_URL,
		accessToken:  await  getAccessToken(),
	});

const  apiService = async () =>  new  DefaultApi(await  apiConfig());
```

### Example API Call

For instance to receive location properties from api call:

```
 const getLocationProperties = async(locationId:string): Promise<LocationOutput> => {
	const apiServiceConfig = await apiService();
	const {data} = await apiServiceConfig
		.readLocationProperties(locationId)
		.catch((res) => throw new Error(res);
	return data;
}

```
