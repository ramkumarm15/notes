[# notes](https://learn.microsoft.com/en-us/dotnet/api/overview/azure/security.keyvault.keys-readme?view=azure-dotnet)


[Function(nameof(ConfigurationUpdateTrigger))]
public async Task<IActionResult> RunAsync([EventGridTrigger] EventGridEvent @event)
{
    _logger.LogInformation("Event type: {type}, Event subject: {subject}", @event.EventType, @event.Subject);
    _logger.LogInformation(JsonConvert.SerializeObject(@event));
    try
    {
        if (@event.EventType == "Microsoft.EventGrid.SubscriptionValidationEvent")
        {
            return new OkObjectResult(new
            {
                ValidationResponse = @event.Data.ToObjectFromJson<SubscriptionValidationEventData>().ValidationCode
            });
        }
        else if (@event.EventType == "Microsoft.AppConfiguration.KeyValueModified" || @event.EventType == "Microsoft.AppConfiguration.KeyValueDeleted")
        {
            var http = new HttpClient();
            var apiUrl = Environment.GetEnvironmentVariable("CentralizedWrapperAPIURL");
            _logger.LogInformation(apiUrl);
            var response = await http.GetAsync(apiUrl);
            _logger.LogInformation(response.StatusCode.ToString());
        }
    }
    catch (Exception ex)
    {
        _logger.LogInformation(ex.Message.ToString());
    }

    return new OkObjectResult("");
}

{
    "Data": {},
    "Id": "85902535-fcf2-48da-8fb2-08cdfd3a5144",
    "Topic": "/subscriptions/f2e037c1-c057-4d76-b609-66cab14a70a8/resourceGroups/poc/providers/microsoft.appconfiguration/configurationstores/pocappconfig",
    "Subject": "https://pocappconfig.azconfig.io/kv/Testing:Settings:Refresh?label=%00&api-version=2023-10-01",
    "EventType": "Microsoft.AppConfiguration.KeyValueModified",
    "EventTime": "2024-10-20T12:16:17+00:00",
    "DataVersion": "2"
}
