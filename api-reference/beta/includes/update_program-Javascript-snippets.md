
```Javascript

const options = {
	authProvider,
};

const client = Client.init(options);

const program = {
    displayName: "testprogram3 new name"
};

let res = await client.api('/programs/7e59d237-2fb0-4e5d-b7bb-d4f9f9129213')
	.version('beta')
	.update({program : program});

```