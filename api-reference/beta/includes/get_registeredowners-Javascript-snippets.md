
```Javascript

const options = {
	authProvider,
};

const client = Client.init(options);

let res = await client.api('/devices/{id}/registeredOwners')
	.version('beta')
	.get();

```