# sales.data
`Sales.Data` contains the data access layer for the Sales application. It includes the Entity Framework Core `DbContext`, entity configurations, and migration artifacts used to store and load domain data from the database.

## Purpose

- Host the EF Core model and mappings
- Provide repository/CRUD operations used by higher layers (services, API)
- Contain EF Core migrations and database initialization scripts

## Prerequisites

- .NET 10 SDK
- A supported relational database (SQL Server is used by default in this solution)
- `dotnet-ef` CLI tool for managing migrations (optional if migrations are applied from another project):
  - Install: `dotnet tool install --global dotnet-ef`

## Dependencies

- `Sales.Domain` — domain entities used by the EF model.
- `Microsoft.EntityFrameworkCore` and the provider package (for example `Microsoft.EntityFrameworkCore.SqlServer`) — configured in the project file.
- A startup project that configures `DbContext` at design-time (typically `Sales.Api`).

## Configuration

- The runtime connection string is typically provided by the application that references this project (for example, `Sales.Api`). Update the connection string in that project's `appsettings.json` or `App.config` as appropriate.
- If you need to run migrations from this project directly, add a design-time factory or ensure a startup project provides the configuration for creating the `DbContext`.

## Common tasks

- Add a migration:
  - `dotnet ef migrations add MyMigrationName --project Sales.Data --startup-project Sales.Api`
- Apply migrations to the database:
  - `dotnet ef database update --project Sales.Data --startup-project Sales.Api`

Adjust `--startup-project` to the project that provides the configuration (usually `Sales.Api`). If migrations are managed centrally (for example, by the API project), prefer running EF commands from that startup project.

## Tests

If there are unit/integration tests that depend on the data layer, they will typically be in a separate test project. Use an in-memory or test database when running automated tests.

## Contributing

- Follow existing patterns for entity configuration and naming conventions.
- Add migrations when altering the model and include descriptive migration names.
- Keep database access concerns contained in this project; business logic should remain in `Sales.Services` or `Sales.Domain`.

## License

Refer to the repository root for license information.
