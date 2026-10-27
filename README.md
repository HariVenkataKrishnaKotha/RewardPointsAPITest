# RewardPointsAPITest

> **Moved (October 2026).** These tests now live inside [RewardPointsAPI_StandAlone](https://github.com/HariVenkataKrishnaKotha/RewardPointsAPI_StandAlone) under `tests/RewardPointsAPITest/`. They target .NET 10 there and have grown from 3 to 18 tests. This repo is archived and kept only for history; the rest of this page describes the original 2022 version.

> **Archived.** These are the unit tests for [RewardPointsAPI_StandAlone](https://github.com/HariVenkataKrishnaKotha/RewardPointsAPI_StandAlone), a December 2022 take-home exercise. The repository is read-only and kept for reference. See the API repo for the full write-up.

## What it tests

The project contains three NUnit tests in `UnitTest1.cs` for `TransactionsController`. They use Moq to mock `ILogger` and `IConfiguration`.

The `[SetUp]` method builds five in-memory transactions and passes them to the controller through a mocked `IConfiguration`.

| Test | Call | Asserts |
|---|---|---|
| `Get_ReturnsListOfTransactions` | `Get()` | 5 transactions come back, and the customer, month and amount of two of them match |
| `GetRewardPoints_ReturnsListOfRewardPoints` | `GetRewardPoints("Hari Kotha", "January")` | 1 row: month 1, 120 points ($120 → 90, $80 → 30) |
| `GetTotalRewardPoints_ReturnsListOfTotalRewardPoints` | `GetTotalRewardPoints("Alex Jones")` | 1 row: 164 points ($90 → 40, $137 → 124) |

The tests do not cover threshold edge cases (exactly $50 or $100), fractional amounts, two customers in the same month, missing query parameters, or the health endpoint.

**Stack:** .NET 6 (`net6.0`, end-of-life), NUnit 3.13.3, NUnit3TestAdapter 4.3.1, Moq 4.18.3, coverlet.collector 3.2.0. FluentAssertions 6.8.0 is referenced but not used.

## How to run

The csproj references the API project at `..\RewardPointsAPI_StandAlone\RewardPointsAPI_StandAlone.csproj`, so clone both repositories into the same parent folder:

```bash
git clone https://github.com/HariVenkataKrishnaKotha/RewardPointsAPI_StandAlone.git
git clone https://github.com/HariVenkataKrishnaKotha/RewardPointsAPITest.git
cd RewardPointsAPITest
dotnet test
```

The project targets `net6.0`, which is out of support. If the .NET 6 runtime is not installed, the test host fails to start ("You must install or update .NET to run this application"). In that case, allow roll-forward to a newer installed runtime:

```bash
# bash
DOTNET_ROLL_FORWARD=Major dotnet test
```

```powershell
# PowerShell
$env:DOTNET_ROLL_FORWARD = "Major"
dotnet test
```

Verified on 2026-10-27 with .NET SDK 10.0.301 and roll-forward to the .NET 8 runtime: `Passed! - Failed: 0, Passed: 3, Skipped: 0, Total: 3`. Without roll-forward, the run aborted because the .NET 6 runtime was missing.

You can also open `RewardPointsAPI_StandAlone.sln` from the API repo. It already lists this project as `..\RewardPointsAPITest\RewardPointsAPITest.csproj`, so running `dotnet test RewardPointsAPI_StandAlone.sln` from the API folder runs the same three tests.

## Notes

- The project uses **NUnit**, so new tests take `[Test]` or `[TestCase]`. Ignore the `[Fact]` attribute mentioned in an earlier version of this README; that is xUnit.
- Only `RewardPointsAPITest.csproj`, `UnitTest1.cs` and `Usings.cs` are source. The `bin/`, `obj/` and `.vs/` folders are build output and IDE state that were committed in 2022 and are not needed to build.
- `LICENSE.txt` still contains the template placeholder `[year] [fullname]`.

## Status

Archived. The tests and API are kept as a snapshot of a 2022 exercise and are not maintained.
