#knowledgesession 

Intro

"Hi everyone, today I’ll be walking you through a custom NuGet package I built that centralizes how we handle application configuration and secrets. It leverages Azure App Configuration and Key Vault in .NET Core."

Slide 1

"Each microservice or app had its own way of managing configuration, leading to inconsistencies."

"Secrets were sometimes stored in appsettings.json or passed via environment variables, which isn’t secure or scalable."

"Any change in config required application restarts or redeployments."


Slide 2
"To address the challenges, I built a reusable NuGet package that wraps the logic for fetching configuration values from Azure App Configuration and secrets from Key Vault."

Slide 3
"This slide shows how everything connects.
Our application uses the NuGet package as a bridge—it abstracts away all the configuration and secret-fetching logic.
The package connects to Azure App Configuration, which stores feature flags and app settings.
App Config can also reference secrets from Azure Key Vault"


Feature Management 

"Feature Management is a major benefit of Azure App Configuration.
We’ve integrated support in our package so that teams can utilize the feature.
it allows you to turn application features on or off without deploying new code.
So let's see what is the usage."
Show confluence page

