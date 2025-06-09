# Synapse DEP-enabled workspace with Azure Function calls via Data flows

[![CC BY-NC-SA 4.0][cc-by-nc-sa-shield]][cc-by-nc-sa]
![GitHub commit activity](https://img.shields.io/github/commit-activity/m/erwinkramer/synapse-dep-data-flows-func)

## Reproduction steps

1. Create an Entra ID app registration. In this sample, the `Application ID URI` is `api://bank-nl`.
![alt text](.images/app.png)

1. Create a Function App. In this sample, the app has the domain `func-otel.azurewebsites.net`. For demo purposes, add the following identity configuration to your Function App. The `Allowed identities` value is the `Object ID` of the system assigned identity of your Synapse resource in your tenant:

   ![alt text](.images/functionapp.png)

1. Create a `REST` linked service, pointing to your function app (via the `Base URL`), and the Entra ID app registration (via the `Microsoft Entra ID resource`):
   ![alt text](.images/linked-service.png)

1. Create a managed private endpoint, pointing to the function app. The fqdn must match the Function App domain:
   ![alt text](.images/mpe.png)

1. Create `Data flow` with a [dataset](https://learn.microsoft.com/en-us/azure/data-factory/concepts-datasets-linked-services?tabs=data-factory) based of a `REST` store, and select the `REST` linked service you created earlier. 

   ![alt text](.images/dataflow.png)

1. Finally, test your connection, this will actually use your system assigned identity and will attempt to call via your linked service. 

   ![alt text](.images/test.png)

## Why does this work? 

There are 2 points to notice:

1. Even though we're not using a Function App dataset, simply because that doesn't exist, it still matches your `REST` configuration with the managed private endpoint connection of your Function App. DEP will allow all connections that have managed private endpoints.
1. Because authentication is in context of the linked service, DEP will allow that.

## License

This work is licensed under a
[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License][cc-by-nc-sa].

[![CC BY-NC-SA 4.0][cc-by-nc-sa-image]][cc-by-nc-sa]

[cc-by-nc-sa]: http://creativecommons.org/licenses/by-nc-sa/4.0/
[cc-by-nc-sa-image]: https://licensebuttons.net/l/by-nc-sa/4.0/88x31.png
[cc-by-nc-sa-shield]: https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg