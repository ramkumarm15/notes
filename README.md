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

[
    {
        "id": "fdb059d0-4baf-484b-8780-d678d91f4921",
        "topic": "/subscriptions/f2e037c1-c057-4d76-b609-66cab14a70a8/resourceGroups/poc/providers/microsoft.appconfiguration/configurationstores/pocappconfig",
        "subject": "https://pocappconfig.azconfig.io/kv/Testing:Settings:Refresh?label=%00\u0026api-version=2023-10-01",
        "data": {
            "key": "Testing:Settings:Refresh",
            "label": null,
            "etag": "8fo_R9JgfIEs125FJR9BB7FPgqXLH3Zm-5lZO82C1-M",
            "syncToken": "zAJw6V16=NDoxOSM3NDg0NTc5Mg==;sn=74845792"
        },
        "eventType": "Microsoft.AppConfiguration.KeyValueModified",
        "eventTime": "2024-11-12T11:34:58.0000000Z",
        "dataVersion": "2"
    }
]
