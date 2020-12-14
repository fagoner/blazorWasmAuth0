## Blazor WASM + Auth0

## Requirements

* [x] Dotnet core 3.1
* [x] Auth0 Account

## Package(s)

``` 
dotnet add package Microsoft.AspNetCore.Components.WebAssembly.Authentication
```

## Files to modify

#### wwwroot/appsettings.json

``` 
{
  "Auth0": {
    "Authority": "https://<<authority>>",
    "ClientId": "<<clientId>>"
  }
}
```

#### Program.cs
```
...
builder.Services.AddOidcAuthentication(options =>
{
    // Configure your authentication provider options here.
    // For more information, see https://aka.ms/blazor-standalone-auth
    builder.Configuration.Bind("Auth0", options.ProviderOptions);
    options.ProviderOptions.ResponseType = "code";
});
...
```

#### Pages/Authentication.razor

``` 
@* Client/Pages/Authentication.razor *@

@page "/authentication/{action}"

@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@using Microsoft.Extensions.Configuration

@inject IConfiguration Configuration
@inject NavigationManager Navigation

<RemoteAuthenticatorView Action="@Action">
    <LogOut>
        @{
            var authority = (string)Configuration["Auth0:Authority"];
            var clientId = (string)Configuration["Auth0:ClientId"];

            Navigation.NavigateTo($"{authority}/v2/logout?client_id={clientId}");
        }
    </LogOut>
</RemoteAuthenticatorView>

@code{
    [Parameter] public string Action { get; set; }
}
```

## Credits

* [securing-blazor-webassembly-apps](https://auth0.com/blog/securing-blazor-webassembly-apps/)
