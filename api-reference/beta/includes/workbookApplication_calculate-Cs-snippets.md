
```Cs

GraphServiceClient graphClient = new GraphServiceClient( authProvider );

var calculationType = "calculationType-value";

await graphClient.Me.Drive.Items["{id}"].Workbook.Application
	.Calculate(calculationType)
	.Request()
	.PostAsync()

```