
```Cs

GraphServiceClient graphClient = new GraphServiceClient( authProvider );

await graphClient.Me.Calendar
	.Request()
	.DeleteAsync();

```