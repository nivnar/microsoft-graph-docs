
```Cs

GraphServiceClient graphClient = new GraphServiceClient( authProvider );

var thumbnails = await graphClient.Me.Drive.Items["{item-id}"].Thumbnails
	.Request()
	.GetAsync();

```