# WPF .NET Core REST API Client

A **WPF** desktop application built with **.NET Core** that consumes a RESTful API (e.g., GoRest public API). Implements **MVVM** pattern with async HTTP calls for CRUD operations on user data, showcasing:

- **MVVM Architecture** with `ViewModelLocator`
- **HTTP Abstraction** via `IWebRequestLib` and `WebRequestLib`
- **Configuration** using `Microsoft.Extensions.Configuration`
- **JSON Serialization** with `Newtonsoft.Json`
- **Data Binding** and Commands for UI interactions
- **Unit Testing** for models and HTTP library

---

## Table of Contents
1. [Features](#features)
2. [Prerequisites](#prerequisites)
3. [Getting Started](#getting-started)
4. [Configuration](#configuration)
5. [Architecture](#architecture)
6. [Project Structure](#project-structure)
7. [Models](#models)
8. [ViewModels](#viewmodels)
9. [Helpers](#helpers)
10. [Views](#views)
11. [Testing](#testing)
12. [Running the Application](#running-the-application)
13. [Contributing](#contributing)
14. [License](#license)

---

## Features

- **CRUD Operations**: Retrieve pages of users, search, export, add, update, and delete via REST API.
- **Async HTTP**: Non-blocking calls with `HttpClient` wrapped in `WebRequestLib`.
- **Token Authentication**: Bearer token support for secured endpoints.
- **Dynamic DataGrid**: Auto-refreshing list of users with paging.
- **Dialog Windows**: Separate dialogs for Add, Edit, and Delete functions.
- **Dependency Injection**: Constructor injection of services and configuration.
- **Unit Tests**: Coverage for API consumer logic and HTTP wrapper.

## Prerequisites

- [.NET 6.0 SDK](https://dotnet.microsoft.com/download)
- Visual Studio 2022 or later
- Internet access for external API (e.g., https://gorest.co.in)

## Getting Started

1. **Clone the repository**:
   ```bash
git clone https://github.com/nicktretyakov/WPF_REST_API_NetCore.git
cd WPF_REST_API_NetCore/WpfAppApiClientNetCore
```
2. **Restore NuGet packages**:
   ```bash
dotnet restore
```
3. **Configure API settings** (see below).
4. **Build and run** the WPF project.

## Configuration

Edit `appsettings.json` in the project root:

```json
{
  "MySettings": {
    "BaseURI": "https://gorest.co.in/public-api/",
    "BearerToken": "<YOUR_TOKEN_HERE>"
  }
}
```

- `BaseURI`: Base URL of the REST API.
- `BearerToken`: Your API token (if required).

## Architecture

```text
+--------------------+     +------------------------+     +--------------------+
|      Views         | <-> |     ViewModels         | <-> |      Models         |
| (XAML + code-behind)|     | (INotifyPropertyChanged)|     | (ApiConsumerModel)  |
+--------------------+     +------------------------+     +--------------------+
         ^                                                    |
         |                                                    v
     RelayCommand                                    WebRequestLib (IWebRequestLib)
         |                                                    |
         v                                                    v
      Helpers                                         HTTP calls via HttpClient
```

- **Views**: XAML windows (`MainWindow`, `AddData`, `EditData`, `DeleteData`).
- **ViewModels**: Expose properties, commands, and orchestrate model interactions.
- **Models**: `ApiConsumerModel` implements `IApiConsumerModel` to perform API calls.
- **Helpers**: `WebRequestLib` implements `IWebRequestLib` for HTTP verbs.

## Project Structure

```text
WpfAppApiClientNetCore/
├── appsettings.json       # API configuration
├── Helpers/               # RelayCommand, ModelHelper
├── Models/                # ApiConsumerModel, interfaces, WebRequest models
├── ViewModels/            # ApiConsumerViewModel and interface
├── Views/                 # XAML windows and code-behind
├── ViewModelLocator.cs    # Service locator for DI
├── WpfAppApiClientNetCore.csproj
└── WpfAppApiClientNetCore.sln

WpfAppApiClientNetCoreTests/
├── Models/                # Tests for ApiConsumerModel
├── ViewModels/            # Tests for ApiConsumerViewModel
└── WpfAppApiClientNetCoreTests.csproj
```

## Models

- **`ApiConsumerModel`** (`IApiConsumerModel`): Contains methods:
  - `GetPageAsync(int page)`
  - `SearchUserAsync(Person user)`
  - `SearchedUserGetPageAsync(Person user, int page)`
  - `ExportUserAsync(Person user, int totalPages)`
  - `GetUserAsync(int id)`
  - `PostDataAsync(Person person)`
  - `PatchDataAsync(Person person)`
  - `DeleteDataAsync(Person person)`
- **`WebRequestLib`** (`IWebRequestLib`): Low-level HTTP helper using `HttpClient`.
- **Configuration** via `IConfiguration` for base URI and token.

## ViewModels

- **`ApiConsumerViewModel`** (`IApiConsumerViewModel`):
  - Properties: `Users`, `SelectedUser`, `Page`, `SearchCriteria`, etc.
  - Commands: `LoadPageCommand`, `SearchCommand`, `AddCommand`, `EditCommand`, `DeleteCommand`, `ExportCommand`.
  - Interacts with `IApiConsumerModel` to fetch and mutate data.

## Helpers

- **`RelayCommand`**: Implements `ICommand` for binding actions.
- **`ModelHelper`**: Utility methods for data conversion and validation.

## Views

- **`MainWindow.xaml`**: DataGrid display, paging controls, and action buttons.
- **`AddData.xaml`**, **`EditData.xaml`**, **`DeleteData.xaml`**: Dialog windows for CRUD operations.

## Testing

1. **Navigate** to `WpfAppApiClientNetCoreTests` folder.
2. **Run tests** with:
   ```bash
dotnet test
```
- **Model Tests**: `ApiConsumerModelTests`, `WebRequestLibTests`
- **ViewModel Tests**: `ApiConsumerViewModelTests`

## Running the Application

- **Development**: In Visual Studio, set `WpfAppApiClientNetCore` as startup and press F5.
- **Standalone**: `dotnet run --project WpfAppApiClientNetCore/WpfAppApiClientNetCore.csproj`

Ensure `appsettings.json` is present alongside the executable.

## Contributing

1. Fork the repository.
2. Create a new branch (`feature/XYZ`).
3. Implement your changes and add tests.
4. Submit a pull request.

Please adhere to .NET Core coding conventions and include unit tests for new functionality.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

