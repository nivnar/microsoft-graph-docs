
```Cs

GraphServiceClient graphClient = new GraphServiceClient( authProvider );

var report = await graphClient.Reports.GetOffice365GroupsActivityStorage('D7')
	.Request()
	.GetAsync();

```