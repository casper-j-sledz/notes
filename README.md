> # [Angular](angular.md)

> # Azure 
> ## App Certificates & Secrets
>> * -> https://portal.azure.com/
>> * -> App registrations
>> * -> Select or create app
>> * -> Manage -> Certificates & secrets
> ## Azure - Pipe Config
>> * -> Home 
>> * -> App Services 
>> * -> [Select service] 
>> * -> Settings --> Configuration
>> * -> FUNCTION_EXTENSION_VERSION [3 - for .NET Core 3 and higher]
>> * -> FUNCTION_WORKER_RUNTIME [dotnet-isolated - for .NET 5 and higher]
> #### [DevOps Server (local / internal environments)](https://learn.microsoft.com/en-us/azure/devops/server/download/azuredevopsserver?view=azure-devops)
> #### [Az-400 - Create account](https://microsoftlearning.github.io/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/Instructions/Labs/AZ400_M00_Validate_lab_environment.html)
> #### [Az-400 - Create account](https://microsoftlearning.github.io/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/)
> #### CSP - Azure Subscription in Poland
> ## DevOps
>> ### Pipeline auto run after merge
>>> Edit Pipeline -> Triggers -> Enable continuous integration -> select proper branch
>> ### Pipe Date Versioning
>>> {pipelinePrefix}$(date:yy).$(date:MM).$(date:dd)$(rev:.r)

> # BitLocker
>> `powershell`
>> ```powershell
>> Set-Location $env:UserProfile
>> manage-bde -protectors -get c:
>> 
>> # Volume C: [System]
>> # All Key Protectors
>> #     Numerical Password:
>> #       ID: {$ID_Numerical_Password}
>> #       Password:
>> #         {Password}
>> #     TPM And PIN:
>> #       ID: {ID_TPM_And_PIN}
>> #       PCR Validation Profile:
>> #         7, 11
>> #         (Uses Secure Boot for integrity validation)
>> 
>> manage-bde -protectors -adbackup c: -id $ID_Numerical_Password

> # C#, .Net
> ## Code
>> ### Add Custom Headers to Http Response
>>> ```csharp
>>> private void AddHeaders(IHeaderDictionary headers, HttpResponseHeaders headersToAdd)
>>> {
>>>   var autoAddHeaders = new HashSet<string> { "Content-Type", "Date", "Server", "Transfer-Encoding" };
>>>
>>>   foreach (var h in headersToAdd)
>>>   {
>>>     if(!autoAddHeaders.Contains(h.Key))
>>>       headers.Append(h.Key, string.Join(",", h.Value!));
>>>   }
>>> }
>> ### Assert Log
>>> ```csharp
>>> // Assert
>>> this.loggerMock
>>>     .Verify(l => l.Log(
>>>         LogLevel.Error,
>>>         It.IsAny<EventId>(),
>>>         It.Is<It.IsAnyType>((@object, _) => @object.ToString()!.Contains("{...}")),
>>>         It.IsAny<Exception>(),
>>>         It.IsAny<Func<It.IsAnyType, Exception, string>>>()!));
>>> loggerMock.VerifyNoOtherCalls();
>> ### Attribute - Property
>>> ```csharp
>>> [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
>>> public class ExampleAttribute : Attribute { public string ExampleProperty { get; set; } }
>>> 
>>> public class ExampleClass
>>> {
>>>   public string PropertyOne { get; private set; }
>>> 
>>>   [ExampleAttribute]
>>>   public string PropertyTwo { get; private set; }
>>> 
>>>   [ExampleAttribute(ExampleProperty = "Test")]
>>>   public string PropertyThree { get; private set; }
>>> }
>> ### Attribute - Repeat Test
>>> ```csharp
>>> using Microsoft.VisualStudio.TestTools.UnitTesting;
>>> using System;
>>> using System.Collections.Generic;
>>> using System.Linq;
>>> using System.Reflection;
>>> 
>>> namespace NET_47.MsTest
>>> {
>>>     public sealed class RepeatAttribute : Attribute, ITestDataSource
>>>     {
>>>         private const int DEFAULT_REPEAT = 1;
>>>         private readonly int _count;
>>> 
>>>         public RepeatAttribute(int count = DEFAULT_REPEAT)
>>>             => _count = count > 0 ? count : DEFAULT_REPEAT;
>>> 
>>>         // Called before test
>>>         /// <inheritdoc/>
>>>         public IEnumerable<object[]> GetData(MethodInfo methodInfo)
>>>             => Enumerable.Range(1, _count).Select(x => new object[0]);
>>> 
>>>         // Called after test
>>>         /// <inheritdoc/>
>>>         public string GetDisplayName(MethodInfo methodInfo, object[] data)
>>>             => data != null
>>>                 ? $"{methodInfo.Name} x{_count}"
>>>                 : null;
>>>     }
>>> }
>> ### Disable / Open CORS
>>> ```csharp
>>> builder.Services.AddCors(options =>
>>> {
>>>   // Some applications like DataFlow-UI for local development requires enabling CORS:
>>>   options.AddDefaultPolicy(policy =>
>>>   {
>>>       policy.AllowAnyOrigin()
>>>             .AllowAnyMethod()
>>>             .AllowAnyHeader();
>>>   });
>>> });
>> ### Documenting Code
>>> #### Inheritdoc
>>>> ```csharp
>>>> ///<inheritdoc/>
>>>> ///<inheritdoc src="YourClass"/>
>>> ### Generic types
>>>> ```csharp
>>>> /// <returns>
>>>> ///    A <see cref="ActionResult{T}"/> containing a <see cref="List{T}"/> of <see cref="ExampleResponse"/>.
>>>> /// </returns>
>>>> public async Task<ActionResult<List<ExampleResponse>>> Get([FromQuery] ExampleQueryModel exampleQuery)
>>> #### Other
>>>> ```csharp
>>>> /// I<see cref="EgGeneric{T}"/>
>>>> /// <typeparam name="T">Type of delete service.</typeparam>
>>>> 
>>>> /// <summary>
>>>> /// Initializes a new instance of the <see cref="EgConstructor"/> class.
>>>> /// </summary>
>>>> 
>>>> /// <summary>
>>>> /// e.g. summary.
>>>> /// </summary>
>>>> /// <param name="egParameter"> e.g. parameter.</param>
>>>> /// <returns>e.g. result.</returns>
>> ### Enum to Array (Names)
>>> ```csharp
>>> Enum.GetNames<EnumType>();
>> ### ToDictionary()
>>> `(ICollection, IEnumerable).Cast<string>().ToDictionary(k => k, k => Session[k]);`
>> ### Generic + Interface
>>> ```csharp
>>> public class GenericClass<T> : IInterface where T : IGenericInterface
>> ### Getting request data - headers, cookies, etc.:
>>> `IHttpContextAccessor`
>> ### Handling partial success of parallel processing
>>> ```csharp
>>> var results = new ConcurrentBag<ExampleResults>();
>>> var errors  = new ConcurrentDictionary<string, Error>();
>>>
>>> await Parallel.ForEachAsync(ExampleList, cancellationToken, async (el, ct) => { [...] }
>>> ////////////////////////////////////////////////////////////////////////////////////////////////////
>>> protected ActionResult<T> PartialSuccessResult<T>(Result<T> result)
>>> {
>>>      if (!result.IsSuccess)
>>>          return StatusCode(result.Error.StatusCode, result.Error.Message);
>>>
>>>      if (result.Error != null)
>>>      {
>>>          Response.Headers.Append("Error-Code", result.Error.StatusCodes);
>>>          Response.Headers.Append("Error-Message", result.Error.Message);
>>>          return StatusCode(StatusCodes.Status207MultiStatus, result.Value);
>>>      }
>>>
>>>      return Ok(result.Value);
>>> }
>> ### Is xUnit Test
>>> ```csharp
>>> private static bool Is_xUnitTest()
>>> {
>>>     string testAssemblyName = "xunit";
>>>     return AppDomain.CurrentDomain.GetAssemblies().Any(a => a.FullName.StartsWith(testAssemblyName));
>>> }
>> ### HostEnvironmentExtensions
>>> ```csharp
>>> public static class HostEnvironmentExtensions
>>> {
>>>     private static string DB_POSTGRES_TYPE { get => "PostgreSQL"; }
>>> 
>>>     public static bool IsPostgres(this IHostEnvironment environment)
>>>     {
>>>         if (environment == null)
>>>             throw new ArgumentNullException(nameof(environment));
>>> 
>>>         return environment.EnvironmentName.Contains(DB_POSTGRES_TYPE, StringComparison.InvariantCultureIgnoreCase);
>>>     }
>>> }
>> ### ILike query (EF Core)
>>> ```csharp
>>> public async Task<List<ExampleEntity>>> GetAsync(ExampleQueryModel exampleQuery)
>>> {
>>>   var query = dbContext.ExampleEntity.AsQueryable();
>>>
>>>   // Exact search
>>>   if (!string.IsNullOrEmpty(exampleQuery.SearchDefaultValue))
>>>     query = query.Where(e => EF.Functions.ILike(e.FirstValue, $"{exampleQuery.SearchDefaultValue}"));
>>>
>>>   // Like search
>>>   if (!string.IsNullOrEmpty(exampleQuery.SearchValue))
>>>     query = query.Where(e => EF.Functions.ILike(e.SecondValue, $"%{exampleQuery.SearchValue}%"));
>>>
>>>   return await query.ToListAsync();
>>> }
>> ### Linq function
>>> ```csharp
>>> var r = healthReport.Entries.Aggregate(new int[3], (acc, keyValue) => acc);
>>> await httpContext.Response.WriteAsync($"_");
>>> ////////////////////////////////////////////////////////////////////////////////////////////////
>>> private static readonly Func<int[], KeyValuePair<string, HealthReportEntry>, int[]> AggregateStatuses = (accumulator, entry) =>
>>> {
>>>     accumulator[(int)entry.Value.Status]++;
>>>     return accumulator;
>>> };
>> ### Metrics - Counter & Histogram
>>> ```csharp
>>> var serviceName = "TestService";
>>> var serviceNamespace = "TestServiceNamespace";
>>> 
>>> var telemetryAttributes = new Dictionary<string, object>
>>> {
>>>     { "service.name", serviceName },
>>>     { "service.namespace", serviceNamespace },
>>>     { "service.instance.id", "X" },
>>>     { "service.version", "1.0.0.0" },
>>> };
>>> 
>>> var meterName = "TestMeter";
>>> var resourceBuilder = ResourceBuilder.CreateDefault().AddAttributes(telemetryAttributes);
>>> _meter = new Meter(meterName);
>>> _meterProvider = Sdk.CreateMeterProviderBuilder()
>>>     .SetResourceBuilder(resourceBuilder)
>>>     .AddMeter(meterName)
>>>     .AddAzureMonitorMetricExporter(o => o.ConnectionString = applicationInsightsConnection, null)
>>>     .Build();
>>> 
>>> ConcurrentDictionary<string, Counter<long>>  counters = new ();
>>> ConcurrentDictionary<string, Histogram<double>>  histograms = new ();
>>> 
>>> //IncrementCounter
>>> counters.GetOrAdd(counterName, _ => _meter.CreateCounter<long>(counterName))
>>>     .Add(1);
>>> 
>>> //RecordHistogram
>>> histograms.GetOrAdd(histogramName, _ => _meter.CreateHistogram<double>(histogramName))
>>>     .Record(value);
>> ### Named arguments
>>> ```csharp
>>> PrintOrderDetails(orderNum: 31, productName: "Red Mug", sellerName: "Gift Shop");
>>> PrintOrderDetails(productName: "Red Mug", sellerName: "Gift Shop", orderNum: 31);
>> ### Registry Edit
>>> * shell\open\command
>>> * Computer\HKEY_CLASSES_ROOT\.txt\ShellNew
>>> * ItemName
>>> * shell\open\command
>>>
>>> * `Microsoft.Win32.Registry.GetValue(String, String, Object)`
>>> * (Microsoft.Win32.Registry.dll) `Install-Package Microsoft.Win32.Registry -Version 5.0.0`
>> ### String Interpolation Formatting
>>> ```csharp
>>> $"{posBatch:D10}{posStan:D6}" //Digits formatting
>> ### String to bites (serialize)
>>> ```csharp
>>> Encoding.UTF8.GetBytes(@string)
>> ### Swagger default value
>>> ```csharp
>>> [ExcludeFromCodeCoverage]
>>> public class CustomersQueryModel
>>> {
>>>   /// <summary>
>>>   /// Search Default Value
>>>   /// </summary>
>>>   [DefaultValue("Example-Default-Value")]
>>>   public string? SearchDefaultValue { get; set; }
>>>
>>>   /// <summary>
>>>   /// Search Value
>>>   /// </summary>
>>>   public string? SearchValue { get; set; }
>>> }
>> ### Is Type (Type pattern matching)
>>> ```csharp
>>> if (@object is ExpectedType expectedTypeInstance) { ... }
>> ### Arrange, Act, Assert
>>> ```csharp
>>> // Arrange
>>> // Act
>>> // Assert
> ## Remove bin & obj catalogs
>> `git clean -xdf --dry-run`
> ## PreBuildEvent
>> ```xml
>> <PropertyGroup>
>>     <PreBuildEvent>powershell.exe {powershell commands}</PreBuildEvent>
>> </PropertyGroup>
> ## [NuGet building](https://github.com/dotnet/sourcelink) (Assembly, .csproj)
>> ```xml
>> <PropertyGroup>
>>     [...]
>>     <OutputType>Library</OutputType>
>>     <IsPackable>true</IsPackable>
>>     <Authors>C. J. Sledz, {...}</Authors>
>>     <Company>{...}</Company>
>>     <ContinuousIntegrationBuild>true</ContinuousIntegrationBuild>
>>     <DebugType>embedded</DebugType>
>>     <Description>{...}</Description>
>>     <Deterministic>true</Deterministic>
>>     <EmbedUntrackedSources>true</EmbedUntrackedSources>
>>     <PackageLicenseExpression>UNLICENSED</PackageLicenseExpression>
>>     <PackageReadmeFile>README.md</PackageReadmeFile>
>>     <PublishRepositoryUrl>true</PublishRepositoryUrl>
>>     <Version>$(version)</Version>
>>   </PropertyGroup>
>> 
>>   <ItemGroup>
>>    {Repository type: https://github.com/dotnet/sourcelink}
>>   </ItemGroup>
>> ```
>> * Might be required to run `dotnet pack` in .csproj location:
>> * `dotnet add package Microsoft.SourceLink.AzureRepos.Git --version 1.1.1`
> ## Errors
>> ### 'IServiceCollection' does not contain a definition for 'AddHealthChecks' 
>>> * **ERROR:**'IServiceCollection' does not contain a definition for 'AddHealthChecks' and no accessible extension method 'AddHealthChecks' accepting a first argument of type 'IServiceCollection' could be found (are you missing a using directive or an assembly reference?).
>>> * **Solution:** Install NuGet or add
>>>   * `<PackageReference Include="Microsoft.Extensions.Diagnostics.HealthChecks" Version="X.X.X" />`
>> ### Unable to start program 'func' (Azure Functions)
>>> * **ERROR:** Unable to start program 'func'
>>> * **Solution:** 
>>>   * a. Add Azure Functions(func.exe) to Environment Variables 
>>>     *  e.g. path: `%AppData%\..\Local\AzureFunctionsTools\Releases\3.37.0\cli_x64`
>>>   * b. Set path in project properties
>>>     * -> VS 
>>>     * -> Properties (Project)
>>>     * -> Debug 
>>>     * -> General 
>>>     * -> Open debag launch profiles UI 
>>>     * -> Executable 
>>>     * -> Set path to func.exe e.g. path `%AppData%\..\Local\AzureFunctionsTools\Releases\3.37.0\cli_x64`
> ## [EditorConfig](https://editorconfig.org)
>> ```powershell
>> dotnet format
>> dotnet format --verify-no-changes -v diag
>> dotnet format whitespace --verify-no-changes -v diag
>> ```
>> ```ini
>> # Indentation and spacing
>> indent_size = 4
>> indent_style = space
>> tab_width = 4
>> trim_trailing_whitespace = true
>> 
>> # New line preferences
>> insert_final_newline = true
>> 
>> # Organize usings
>> dotnet_sort_system_directives_first = true
>> file_header_template = © Copyright {year} {company} All rights reserved.
>> 
>> # New line preferences
>> dotnet_style_allow_multiple_blank_lines_experimental = false
>> 
>> # Naming rules
>> dotnet_naming_rule.interface_should_be_begins_with_i.severity = error
>> dotnet_naming_rule.types_should_be_pascal_case.severity = error
>> dotnet_naming_rule.non_field_members_should_be_pascal_case.severity = warning
>> 
>> # Spelling rules
>> spelling_languages = en-gb
>> spelling_exclusion_path = exclusion.dic
>> spelling_checkable_types = strings,identifiers,comments
>> spelling_use_default_exclusion_dictionary = false
>> 
>> ######################## Code analyzer rules configuration ########################
>> # SA1101: Prefix local calls with this
>> dotnet_diagnostic.SA1101.severity = none
>> 
>> # SA1200: Using directives should be placed correctly
>> dotnet_diagnostic.SA1200.severity = none
>> 
>> # SA1309: Field names should not begin with underscore
>> dotnet_diagnostic.SA1309.severity = none
>> 
>> # SA1600: Elements should be documented
>> dotnet_diagnostic.SA1600.severity = none
> ## [ReSharper CLI](https://www.jetbrains.com/help/resharper/ReSharper_Command_Line_Tools.html)
>> ### Installation
>>> ```powershell
>>> dotnet tool install -g JetBrains.ReSharper.GlobalTools
>> ### Running
>>> ```powershell
>>> $outputPath = "$($env:UserProfile)\Downloads\code-inspection.txt" && 
>>> jb InspectCode --build --format=Text -o="$($env:UserProfile)\Downloads\-code-inspection.txt" >> --severity=WARNING --verbosity=WARN --profile=".\Solution.sln.DotSettings" --no-swea Solution.sln 
>>> && Write-Output "$($outputPath):`n`t$(Type $outputPath)"
> ## secret.json
>> * Secrets Directory: `%APPDATA%\Microsoft\UserSecrets\`
>> * .csproj
>> ````xml
>> <PropertyGroup>
>>   <UserSecretsId>$(MSBuildProjectName)</UserSecretsId>
>>   <GenerateAssemblyInfo>true</GenerateAssemblyInfo> 
>>   <!-- If GenerateAssemblyInfo == false add `[assembly: UserSecretsId("your_user_secrets_id")]` in `AssemblyInfo.cs` -->
>> </PropertyGroup>
> ## [.NET - Support Policy](https://dotnet.microsoft.com/en-us/download/visual-studio-sdks?cid=getdotnetsdk)
> ## [.NET Framework - Support Policy](https://learn.microsoft.com/en-us/lifecycle/products/microsoft-net-framework)
> ## [.NET vs C# language versioning](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/configure-language-version)

> # Cloud
>> * [Cloud Compare](https://comparecloud.in/#.WykVEQU53Og.linkedin)
>> * **Claud Cost Calculators**
>>   * [Azure](https://azure.microsoft.com/pricing/calculator)
>>   * [AWS](https://calculator.aws)
>>   * [Google Cloud](https://cloud.google.com/products/calculator)

> # Copilot
> * `Generate the XML documentation in this file, use <see cref=""> tags to properly describe parameters and return objects.`

> # Cucumber
>> ### [Cucumber Regex](https://github.com/cucumber/cucumber-js/blob/main/docs/support_files/world.md)
> ## [Cucumber Setup Project](https://marketplace.visualstudio.com/items?itemName=AbhinabaGhosh.cucumberquick)
>> ### Install Cucumber
>>> `npm install cucumber --save-dev`
>> ### Extensions
>>> * Cucumber (Gherkin) Full Support
>>> * Cucumber Quick
>> ### Add file:
>>> .vscode\settings.json
>>> ```js
>>>	{
>>>	  "cucumber-quick": {
>>>			"tool": "protractor",
>>>			"script": "npx protractor relative-path/to/protractor.conf.js"
>>>		}
>>>	}
> ### PowerShell:	
>>> * `npm install -g protractor`
>>> * `npm protractor --version`


> # Developer Community, Change requests
> ### C#
>> * [Extension properties](https://developercommunity.visualstudio.com/t/add-c-extension-properties/972639)
>> * [Anonymous Interface Implementation](https://stackoverflow.com/questions/51722348/c-sharp-anonymous-interface-implementation)
> ### Azure DevOps
>> * [Ability to remove branches from "Mine" view](https://developercommunity.visualstudio.com/t/ability-to-remove-branches-from-mine-view/931378)
> ## Jira / Confluence
>> * [Display JIRA issue summaries in edit mode](https://jira.atlassian.com/browse/CONFSERVER-24448)

> # Docker & Podman
> ## Docker
>> ### Start container
>>> `docker compose up -d`
>> ### Stop containers
>>> `docker compose stop`
>> ### Stop and Remove containers
>>> `docker compose down`
> ## Disable browser lunch
>> ### Edit docker-compose.dcproj:
>>> ```xml
>>> <Project>
>>> <PropertyGroup>
>>>     [...]
>>> <DockerLaunchAction>None</DockerLaunchAction>
>>>     [...]
>>> </PropertyGroup>
>>> </Project>
> ## Create local network
>> ### 1. Create local network: .ps1
>>> `docker network create local_network`
>> ### 2. Edit source & destination: docker-compose.yml
>>> ```yml
>>> services:
>>>   service.name:
>>>     container_name: service.name
>>>     [...]
>>>     networks:
>>>       - local_network
>>>
>>> networks:
>>>   local_network:
>>>     external: true
>> ### 3. Edit destination config with url for source
>>>`https://service.name/[...]`
> ## Podman
> ### Initial configuration
>> 1. Default installation
>> 1. Default Podman Machine
>> 1. Resources install Compose
>> 1. Settings preferences Docker Compatibility -> Enabled
> ### Commands
>> 1. `podman build -t=example-api .\`
>> 1. `podman tag example-app-tag europe-west-docker.pkg.env/example-env/example-repository/example-app`
>> 1. `podman run -p localhost:5001/tcp europe-west-docker.pkg.env/example-env/example-repository/example-app`
>> 1. `gcloud auth configure-docker europe-west-docker.pkg.env`
>> 1. `podman push europe-west-docker.pkg.env/example-env/example-repository/example-app`
> ### ERROR: Cannot connect to Podman. Please verify your connection to the Linux
>>> * **ERROR:** Cannot connect to Podman. Please verify your connection to the Linux system using `podman system connection list`, or try `podman machine init` and `podman machine start` to manage a new Linux VM
Error: unable to connect to Podman. failed to create sshClient: connection to bastion host (ssh://user@localhost:50754/run/user/1000/podman/podman.sock) failed: dial tcp [::1]:50754: connectex: No connection could be made because the target machine actively refused it.
>>> * **Solution:** 
>>> ```powershell
>>> podman machine stop
>>> podman machine rm -f
>>> podman machine init --now
> ### ERROR: Error response from daemon: i/o timeout
>>> * **ERROR:** Image chainGuard/minio:latest Interrupt 123.1s Error response from daemon: i/o timeout.
>>> * **Solution:** 
>>> ``` VPN connection might block connection / check internet

> # Edge
> ## Config
>> ## Import Browser Data
>>> * -> Settings
>>> * -> Profiles
>>> * -> Import browser data
>> ## 
>>> * -> Privacy, search, and services
>>>   * -> Services
>>>     * -> Save time and money with Shopping in Microsoft Edge -> OFF
>>>     * -> Get notifications of related things you can explore with Discover -> OFF
>>>     * -> Address bar and search
>>>       * -> Search engine used in the address bar -> Select or add one
>>>       * -> Search on new tabs uses search box or address bar -> Address bar
>> ## 
>>> * -> Appearance
>>>   * -> Customize toolbar
>>>     * -> Show tab actions menu -> OFF
>>>     * -> Hide title bar while in vertical tabs -> OFF
>>>     * -> Show tab preview on hover -> OFF
>>>     * -> Show favorites bar -> ALWAYS
>>>     * -> Favorites button -> OFF
>>>     * -> Collections button -> OFF
>> ## 
>>> * -> Sidebar
>>>   * -> Customize sidebar
>>>     * -> Always show sidebar -> OFF
>>>   * -> App and notification settings
>>>     * -> Discover -> OFF
>> ## 
>>> * -> Start, home, and new tabs
>>>   * -> When Edge starts -> Open tabs from the previous session
>>>   * -> Home button -> https://www.google.pl/
>>>   * -> New tab page 
>>>     * -> Layout  -> Focused / Custom
>>>     * -> Language -> UK (English)
>>>     * -> Quick links -> OFF
>>>     * -> Background -> Edit -> OFF
>>>     * -> Show greeting -> OFF
>>>     * -> Content -> OFF 
>> ## Import Favorites (Bookmarks)
>>> * -> Favorites Bar
>>>   * -> Import favorites
>>>     * -> Right click -> delete (not always working, reset browser computer)
> ## Keep logs on redirect / refresh
>> * -> Dev Tools (`F12`)
>>   * -> Network
>>   * -> Check `Preserve log`
> ## Open code file
>> * Dev Tools (`F12`)
>>   * -> Sources
>>   * -> `Ctrl` + `p`


> # EdgeExcel / VBA
> ## Disable cloud save pop up
>> * Save changes to this file?
>> * Avoid losing changes to this file in the future by saving it to a folder backed up in the cloud instead.
>> * To disable popup:
>> * File -> Options -> Save -> Save workbooks (section) -> Don't show the Backstage when opening or saving files with keyboard shortcuts
Excel Function - Distinct Values
> ## Excel links
>> * [Absolute Structured References in Excel Tables](https://www.excelcampus.com/tips-shortcuts/absolute-formula-references-excel-structured-table)
>> * [ADDRESS function](https://support.office.com/en-us/article/address-function-d0c26c0d-3991-446b-8de4-ab46431d4f89)
>> * [ADDRESS function](https://exceljet.net/excel-functions/excel-address-function)
>> * [Change the case of text](https://support.office.com/en-gb/article/change-the-case-of-text-01481046-0fa7-4f3b-a693-496795a7a44d)
>> * [Excel reference to another sheet or workbook (external reference)](https://www.ablebits.com/office-addins-blog/2015/12/08/excel-reference-another-sheet-workbook/)
>> * [Excel Table Doesn't Expand For New Data](https://contexturesblog.com/archives/2015/04/30/excel-table-doesnt-expand-for-new-data/)
>> * [File format Excel](https://docs.microsoft.com/en-us/deployoffice/compat/office-file-format-reference)
>> * [Function returning column letter](https://superuser.com/questions/1259506/formula-to-return-just-the-column-letter-in-excel/1259507)
>> * [How to Create Dynamic Chart Titles in Excel](https://trumpexcel.com/dynamic-chart-titles-in-excel/)
>> * [How to Format Excel Pivot Table](https://www.contextures.com/excel-pivot-table-format.html)
>> * [Hiding zero data labels in chart](https://www.extendoffice.com/documents/excel/2031-excel-hide-zero-data-labels.html)
> ## Functions
>> ### Distinct Values
>>> ```vb
>>> 'Marker:
>>> =IF(COUNTIFS($A$1:A1,A1)>1,0,1)
>>> 'Values:
>>> =IF(COUNTIFS($A$1:A1,A1)>1,"",A1)
>> ### If
>>> ```vb
>>> =IF([@[Column]] = "{{ Value }}", 1, "")
>> ### Row Number (No.)
>>> ```vb
>>> =ROW(A2)-1
>> ### VLookup
>>> ```vb
>>> =IFERROR(VLOOKUP([@[Distinct_Source_Table]], tTable[[Column_Start]:[Column_End]], {{ Columns Shift }}, FALSE), "")
>> ### To clean up
>>> ```vb
>>> =PROPER(A1)
>>> =PODSTAW(A1; "/"; ".")
>>> =SUBSTITUTE(A1; "/"; ".")
>>> =LEWY(B1; ZNAJDŹ(","; B1)-1)
>>> =FRAGMENT.TEKSTU(A1;4;3) & LEWY(A1; 2) & FRAGMENT.TEKSTU(A1; 6; 9)
>>> =INDIRECT("'Inc Statistic'!" & ADDRESS((MATCH(E2;Table6[ID für Ticket];0)+1); 5))
>>> WorkingSheet.Cells(CurrentRow, MyColumn).Value = 
>>> (ClientName & " " & "(" & ClientLocation & ")" & " " & ExtraDefinition)
>>> =INDIRECT("'Inc Statistic'!" & ADDRESS((MATCH(LEFT(D2;FIND("]";D2;9));Table6[[ Ticket Code]];0)+1); 5))
>>> =A1&","&B1&","&+C1&","""&D1&""","""&E1&""","&F1&","&G1&","&H1&","&I1&","&J1&","&K1
>>> WorkingSheet.Range("B3").Value = 
>>> (ClientName & " " & "(" & ClientLocation & ")" & " " & ExtraDefinition)
>>> 
>>> =ADDRESS(1,COLUMN())
>>> =FIND("$", C39) + 1
>>> =FIND("$", C39, C40)
>>> =MID(C39,C40, C41-C40)
>>> =MIN(SUM(INDIRECT($C$42&$B$39&":"&C42&$B$39)), 1)
> ## VBA
>> ### VBA_s1
>>> ```vb
>>> Imports System
>>> 
>>> Module Program
>>> table auto expansion
>>> 
>>>     Private Function TextToCol(text As String, separator as String, stringMark as String) As String()
>>>         Dim splittedText = text.Split(separator)
>>>         Dim listOfValues As New List(Of String)
>>>         Dim charVar As Char
>>>         charVar = stringMark
>>>         Dim stringBuilder As New Text.StringBuilder
>>>         For i = 0 To splittedText.Length-1
>>>             If splittedText(i).Length > 0 Then
>>>                 If splittedText(i)(0).Equals(charVar) Then
>>>                     text = splittedText(i)
>>>                     stringBuilder.Append(text)
>>>                     Do While Not text(text.Length-1).Equals(charVar)
>>>                         i += 1
>>>                         text = separator & splittedText(i)
>>>                         stringBuilder.Append(text)
>>>                     Loop
>>>                     listOfValues.Add(stringBuilder.ToString().Replace(stringMark, ""))
>>>                     stringBuilder.Clear()
>>>                 Else 
>>>                     listOfValues.Add(splittedText(i))
>>>                 End If
>>>             Else 
>>>                 listOfValues.Add(splittedText(i))
>>>             End If
>>>         Next
>>>         return listOfValues.ToArray()
>>>     End Function
>>>     
>>>     Private Sub DateCorrection(values As String(), toRemove as String, fromIndexes As integer())
>>>         For Each item As integer In fromIndexes
>>>             values(item) = values(item).Replace(toRemove, "")
>>>         Next
>>>     End Sub
>>>     
>>>     Sub Main(args As String())
>>>         Const text = "0047642283,Closed,3,[10][P][BeBu] -Neue E4,""Doe, John"",""03.04.2019, 07:51:52"",,""13.11.2019, 06:18:44"",""05.06.2019, 11:08:34"",4627524,224.0"
>>>         'todo covert text to right cell
>>>         Const separator = "," 
>>>         Const stringMark = """"
>>>         Const dateCorrectionToRemove = ","
>>>         Dim dates = {5, 7, 8}
>>>         
>>>         Dim values = TextToCol(text, separator, stringMark)
>>>         DateCorrection(values, dateCorrectionToRemove, dates)
>>>         
>>>         'todo values types conversion
>>>         'todo saving to appropriate cells
>>>         'todo separated sheets for request and incidents
>>>         'todo pivot auto refresh
>>>         'todo months table
>>>         'todo auto loaded months chart
>>>         
>>>         Console.WriteLine(text)
>>>         For Each item In values
>>>             Console.Write(item & "|")
>>>         Next
>>>         Console.WriteLine()
>>>         
>>>     End Sub
>>> End Module
>> ### VBA_s2
>>> ```vb
>>> 'todo select all records witch status is' Sleep' or 'WIP' data marriage
>>> 'todo remove duplicates
>>> 'todo pivot auto refresh
>>> 'todo auto set appropriate month in pivot and chart title
>>> 'todo remove greater i_* in appropriate sheet
>>> 'todo last two charts
>>> 
>>> 'Set sheet_req = Sheets("Req Report")
>>> 'MsgBox VarType(Sheets("Inc Report"))
>>> 'Dim cols_base() As Variant
>>> 'cols_dest = Array(1, 2, 3, 6, 7, 10, 11, 12, 15, 16, 17)
>>> 
>>> Sub ExtractData()
>>>     Dim i As Integer
>>>     i = 1
>>>     Do While Not IsEmpty(Cells(i, "A").Value)
>>>         test (i)
>>>         i = i + 1
>>>     Loop
>>>     MsgBox "A" & i
>>>     DataFormatting (i)
>>> End Sub
>>> 
>>> Sub DataFormatting(ByVal records As Integer)
>>>     IntegerFormatting 4
>>>     IntegerFormatting 11
>>>     DateFormatting 7
>>>     DateFormatting 9
>>>     DateFormatting 10
>>>     FloatFormatting 12, "0.0"
>>> End Sub
>>> 
>>> Sub FloatFormatting(ByVal col As Integer, formatting As String)
>>>     lastRowIndex = Cells(Rows.Count, col).End(xlUp).row
>>>     Set c = Range(Cells(1, col), Cells(lastRowIndex, col))
>>>     c.NumberFormat = formatting
>>>     c.FormulaLocal = c.Value
>>> End Sub
>>> 
>>> Sub DateFormatting(ByVal col As Integer)
>>>     lastRowIndex = Cells(Rows.Count, col).End(xlUp).row
>>>     Set c = Range(Cells(1, col), Cells(lastRowIndex, col))
>>>     c.NumberFormat = "dd.mm.yyyy hh:mm"
>>>     c.FormulaLocal = c.Value
>>> End Sub
>>> 
>>> Sub IntegerFormatting(ByVal col As Integer)
>>>     lastRowIndex = Cells(Rows.Count, col).End(xlUp).row
>>>     Set c = Range(Cells(1, col), Cells(lastRowIndex, col))
>>>     c.NumberFormat = ""
>>>     c.FormulaLocal = c.Value
>>> End Sub
>>> 
>>> Sub test(ByVal row As Integer, Optional separator As String = ",", Optional stringMark As String = """")
>>>     Dim splittedText() As String
>>>     splittedText = Split(Cells(row, "A").Value, separator)
>>>     Dim cellText As String
>>>     Dim column As Integer
>>>     column = 2
>>>     Dim i As Integer
>>>     For i = 0 To ArrayLen(splittedText) - 1
>>>         If Len(splittedText(i)) > 0 And Left(splittedText(i), 1) = stringMark Then
>>>             cellText = Right(splittedText(i), Len(splittedText(i)) - 1)
>>>             Do While Not Right(splittedText(i), 1) = stringMark
>>>                 i = i + 1
>>>                 cellText = cellText & Left(splittedText(i), Len(splittedText(i)) - 1)
>>>             Loop
>>>         Else
>>>             cellText = splittedText(i)
>>>         End If
>>>         Cells(row, column).Value = cellText
>>>         column = column + 1
>>>     Next i
>>> End Sub
>>> 
>>> Public Function ArrayLen(arr As Variant) As Integer
>>>     ArrayLen = UBound(arr) - LBound(arr) + 1
>>> End Function
>>> 
>>> 
>>> 'TotalTicketsPerYearAppNameServeArea
>>> Sub pivotTest()
>>> 
>>>     Dim pivotTableName As String
>>>     Dim fieldName As String
>>>     pivotTableName = "TotalTicketsPerYearAppNameServeArea"
>>>     fieldName = "Total Tickets Per Year"
>>>     
>>>     RefreshPivotTable pivotTableName
>>>     ClearFilterPivotTable pivotTableName, fieldName
>>>     
>>>     'MsgBox VarType(pt)
>>>     'FilterPivotTable pivotTableName, fieldName
>>> End Sub
>>> 
>>> Sub ClearFilterPivotTable(ByVal pivotTableName As String, fieldName As String)
>>>     PivotTables(pivotTableName).PivotFields(fieldName).ClearAllFilters
>>> End Sub
>>> 
>>> Sub RefreshPivotTable(ByVal pivotTableName)
>>>         PivotTables(pivotTableName).PivotFields(fieldName).RefreshTable
>>> End Sub
>>> 
>>> Sub FilterPivotTable(ByVal pivotTableName As String, fieldName As String)
>>>     
>>>     PivotTables(pivotTableName).PivotFields(fieldName).CurrentPage = _
>>>       "0"
>>>     'ActiveSheet.PivotTables("PivotTable2").ManualUpdate = False
>>>     'Application.ScreenUpdating = True
>>> End Sub
> ## VBA links
>> * [Arrays And Ranges In VBA](http://www.cpearson.com/excel/ArraysAndRanges.aspx)
>> * [Disable "Compile error" dialogs](https://stackoverflow.com/questions/11560934/when-editing-microsoft-office-vba-how-can-i-disable-the-popup-compile-error-m)
>> * [MsgBox / Message Box](https://trumpexcel.com/vba-msgbox/)
>> * [Checking if a cell is empty with VBA](https://stackoverflow.com/questions/13360651/excel-how-to-check-if-a-cell-is-empty-with-vba/13360702)
>> * [Debugging in VBA](https://www.youtube.com/watch?v=U9tqvXYL9zw)
>> * [ListObject (Tables)](https://www.thespreadsheetguru.com/blog/2014/6/20/the-vba-guide-to-listobject-excel-tables)
>> * [Passing Variables Between Macros](https://www.thesmallman.com/pass-variables-between-macros)
>> * [Referencing Pivot Table Ranges](https://peltiertech.com/referencing-pivot-table-ranges-in-vba/)
>> * [Refresh Pivot Tables Automatically When Source Data Changes](https://www.excelcampus.com/vba/refresh-pivot-tables-automatically/)
>> * [The Complete Guide to Using Arrays in Excel VBA](https://excelmacromastery.com/excel-vba-array/)
>> * [Collections Ultimate Guide](https://excelmacromastery.com/excel-vba-collections/)
>> * [Excel Filters Ultimate Guide](https://www.excelcampus.com/vba/macros-filters-autofilter-method/)
>> * [VarType function (Visual Basic for Applications)](https://docs.microsoft.com/en-us/office/vba/language/reference/user-interface-help/vartype-function)
>> * [Char type not defined](https://stackoverflow.com/questions/28623732/char-type-not-defined)
>> * [Selecting Multiple Items in a Pivot Table](https://www.mrexcel.com/board/threads/vba-selecting-multiple-items-in-a-pivot-table.1019489/)
>> * [VBA Code To Convert Column Number to Letter Or Letter To Number](https://www.thespreadsheetguru.com/the-code-vault/vba-code-to-convert-column-number-to-letter-or-letter-to-number)
>> * [Converting Data Types](https://software-solutions-online.com/converting-data-types/#Jump2.3)
>> * [PasteSpecial Values, Formats, Formulas](https://wellsr.com/vba/2018/excel/vba-pastespecial-values-formats-formulas-and-more/)
>> * [VBA Strings & Characters - Built-in Constants](https://bettersolutions.com/vba/strings-characters/builtin-constants.htm)
>> * [VBA to Create a PIVOT TABLE in Excel - READY to use MACRO Code](https://excelchamps.com/blog/vba-to-create-pivot-table/)

> # Git & GitHub
> ## Git SSH Key
>> * -> Git GUI -> Help Show SSH key -> Generate Key -> Copy to clipboard
>> * -> GitLab -> User -> Preferences -> SSH Keys -> Add new key 
>>   * Usage type: Authentication & Signing
>>   * Expiration date: null
> ## Pull changes with temp stash
>> `git stash save "TEMP_$(Get-Date -Format "yyyy-MM-dd_HH:mm")"; git pull; git stash apply`
> ## Add Submodule
>> `git submodule add [repositoryURL]`
> ## Checkout / crate branch
>> `git checkout -b us/0000000000`
> ## Crate commit
>> `git commit -m "Commit Name"`
> ## Push to new branch
>> `git push --set-upstream origin (git branch --show-current);`
> ## (GitHub) Go to create new PR (Pull Request) page
>> `start "$((git remote get-url origin).Replace('.git', ''))/pull/new/$(git branch --show-current)"`
> ## (GitHub) Go to PR (Pull Request) page
>> `start "$((git remote get-url origin).Replace('.git', ''))/pulls"`
> ## Branches naming
>> * `bug/AB#[ID] [Title]`
>> * `merge-release-[V.er.si.on]-to-develop`
>> * `spike/AB#[ID] [Title]`
>> * `story/AB#[ID] [Title]`
>> * `task/AB#[ID] [Title]`
>> * `technical/AB#[ID] [Title]`
> ## Add all new files and tracked changes
>> * `git add .`
> ## Change branch
>> * `git checkout [branchName]`
> ## Change commit date
>> ### CommitDate / AuthorDate / GIT_COMMITTER_DATE
>>> * `git commit --amend --date "Mon Mar 1 01:00:00 2020 +0000" --no-edit` 
>> ### Check commit history:
>>> * `git log --pretty=fuller`
> ## Change git editor
>> * `git config --global core.editor {editorName/editorPath}`
>> * `git config --global core.editor     "'C:\Program Files\Microsoft VS Code\Code.exe' -n -w"`
>> * `git config --global sequence.editor "'C:\Program Files\Microsoft VS Code\Code.exe' -n -w"`
>> * `-c "core.editor=code --wait --reuse-window" -c "sequence.editor=code --wait --reuse-window"`
> ## Check commits history
>> * `git log`
> ## Check git version
>> * `git --version`
> ## Remove untracked files
>> * `git clean -xdf --dry-run`
>> * `-x` -> Remove files that are ignored by .gitignore.
>> * `-d` -> Remove untracked directories.
>> * `-f` -> Force removal.
>> * `--dry-run` -> Lists files to delete.
> ## Clear not existing remote references
>> * `git fetch --prune` \ `git fetch -p`
> ## Clone repository
>> * `git clone [httpsAdders / sshAdders]`
> ## Config List
>> * `git config -l`
>> * `git config --global -l`
> ## Config User Name & eMail 
>> * `git config user.name  "Casper J. Sledz"`
>> * `git config user.email "casper.j.sledz@gmail.com"`
>> * `config --global` for global config
> ## Connect Origin Repository
>> * `git remote add origin [urlOfNewRepository]`
> ## Connect Origin Branch
>> * `git branch --set-upstream-to=origin/[branchName]`
> ## Create local branch and switch to it
>> * `git checkout -b [newLocalBranchName]`
> ## Create new commit
>> * `git commit -m [commitName]`
> ## Commands list
>> * `git --help`
> ## Check if repository exist (current folder)
>> * `git status`
> ## Create new local repository
>> * `git init`
> ## Delete branch
>> * `git branch --delete [brachName]` 
>> * `-d` => `--delete`
>> * `-D` => `--delete --force`
> ## Delete origin branch
>> * `git push origin --delete origin [originBrachName]` 
> ## Disable SSL verify (globally) ERR: SSL certificate problem: self signed certificate
>> * `git config --global http.sslVerify [false / true]` 
> ## Discard everything permanently
>> * `git reset --hard`
> ## Discard file to HEAD
>> * `git reset HEAD [fileName]` 
> ## Discard last commit - return to edition
>> * `git reset --soft HEAD~1`
> ## Discard last tracked (added / staged) changes
>> * `git reset HEAD` 
> ## Disconnect Origin
>> * `git remote rm origin` 
> ## Files size (git bash)
>> * `git ls-tree -r --long HEAD | sort -k 4 -n -r` 
> ## Get origin url
>> * `git remote get-url origin`
> ## Ignore file / untrack
>> * `git update-index --assume-unchanged [FileName]`
> ## Ignore file / untrack (revert)
>> * `git update-index --no-assume-unchanged [FileName]`
> ## LFS (Large File Storage)
>> 1. Download and install [LFS](https://git-lfs.com/)
>> 2. `git lfs install`
>> 3. `git lfs track "example\path\to\e.g.\large.dll"`
> ## Moving repository
>> 1. 
>>> 1. `git fetch -p`
>>> 2. `git fetch --tags`
>>> 3. `git remote rm origin`
>>> 4. `git remote add origin [urlOfNewRepository]`
>>> 5. `git push --set-upstream origin [originBranchName] --force` -> (for each branch to migrate )
>>> 6. `git push --tags` 
>> 2. 
>>> 1. -> Create temp directory
>>> 2. -> Open terminal in created director
>>> 3. -> Clone repository to move
>>> 	`git clone {urlOfOldRepository}`
>>> 4. -> May require go catalogue up to cloned repo
>>> 5. -> Check out to all beaches which you would like to move 
>>> 	`git checkout {branch-name}`
>>> 6. -> Fetch tags 
>>> 	`git fetch --tags`
>>> 7. -> you can verify beaches and tags
>>> 	`git tag`
>>> 	`git branch -a`
>>> 8. -> Disconnect old repo
>>> 	`git remote rm origin`
>>> 9. -> Connect with new repo
>>> 	`git remote add origin {urlOfNewRepository}`
>>> 10. -> Push main branch
>>> 11. -> Push rest of branches
>>> 	`git push origin --all`
> ## Open online method documentation
>> * `git help [commandName]`
> ## Override local changes
>> * `git reset --hard origin/development`
> ## Push to new origin branch
>> * `git push --set-upstream origin [newOnlineBranchName]`
> ## Push to repository (existing one)
>> * `git push -u origin [branchName]`
> ## Rebase commits
>> * `git rebase -i HEAD~[numOfLastCommits]`
> ## Remove files from the working tree
>> * `git rm -r [config/example-configuration]`
c-r` -> Allow recursive removal when a leading directory name is given
> ## Remove files (Large)
>> 1. -> [Install JDK](https://adoptium.net/)
>> 2. -> [Download bfg.jar](https://rtyley.github.io/bfg-repo-cleaner/)
>> 3. -> Remove file bigger than:
>>    1. `java -jar "$env:UserProfile\Downloads\bfg-1.14.0.jar" --strip-blobs-bigger-than 100M`
>>    1. `git reflog expire --expire=now --all`
>>    1. `git gc --prune=now --aggressive`
>> 4.  Change brach and repeat (CURRENT BRACH CANNOT BE EDITED)
>> 3. -> Remove specifics file:
>>    1. `java -jar bfg-1.14.0.jar --delete-files oraociei12.dll`
>>    1. `git reflog expire --expire=now --all`
>>    1. `git gc --prune=now --aggressive`
> ## Remove git repository
>> * `Remove-Item -Force -Recurse –path ./.git`
> ## Rename commit
>> * `git commit --amend -m [newCommitName]`
> ## Rename merge
>> * `git commit --amend` -> Renames merge if it's last commit
> ## Reverting single file
>> * `git checkout [commitID] --[filePath]`
>> * `git checkout 933cd760 --"\App\src\API\Example.cs"`
> ## Save changes as temp (stash)
>> * `git stash`
> ## Unstage and remove from index
>> * `git rm --cached [example.cs]`
>> * `rm --cached` 
>>   * Use this option to unstage and remove paths only from the index. 
>>   * Working tree files, whether modified or not, will be left alone.
> ## GitHub Actions
>> ### YML Environment Values References
>>> #### Environment secrets
>>>> `secrets.{{ name }}`
>>> #### Environment variables
>>>> `vars.{{ name }}`
>>> #### Print values
>>>> ```yaml
>>>> - name: "Echo"`
>>>>   run: echo "{{ value }}"`
> ## SSH Config
>> ```powershell
>> ssh-keygen -t ed25519 -C "[userEmail]"
>> ssh-keygen -t ECDSA   -C "[userEmail]"
>>  
>> ssh -i [userDirectory]/.ssh/id_ecdsa [userEmail]
>> ssh [userEmail] mkdir -p .ssh
>>
>> # ssh-rsa
>> #	-go to-> GitLab
>> #	-go to-> User Settings
>> #	-go to-> SSH Keys
>> # 	-copy key-> [userDirectory]/.ssh/id_rsa.pub
>> #	-> pase and add key
>> 
>> git config --global credential.helper WinCred
> ## GitHub Branching Rules
>> ```json
>> {
>>   "id": 1,
>>   "name": "Secure release branches against deletions and updates",
>>   "target": "branch",
>>   "source_type": "Repository",
>>   "source": "github",
>>   "enforcement": "active",
>>   "conditions": {
>>     "ref_name": {
>>       "exclude": [],
>>       "include": [
>>         "refs/heads/Release_Branches/1.0.0.0"
>>       ]
>>     }
>>   },
>>   "rules": [
>>     {
>>       "type": "deletion"
>>     },
>>     {
>>       "type": "non_fast_forward"
>>     },
>>     {
>>       "type": "update"
>>     }
>>   ],
>>   "bypass_actors": []
>> }
>> /////////////////////////////////////////////////////////////////////////////////////////////////
>> {
>>   "id": 2,
>>   "name": "Secure core branches against edition expect TeamId=101",
>>   "target": "branch",
>>   "source_type": "Repository",
>>   "source": "github",
>>   "enforcement": "active",
>>   "conditions": {
>>     "ref_name": {
>>       "exclude": [],
>>       "include": [
>>         "~ALL"
>>       ]
>>     }
>>   },
>>   "rules": [
>>     {
>>       "type": "update"
>>     },
>>     {
>>       "type": "creation"
>>     }
>>   ],
>>   "bypass_actors": [
>>     {
>>       "actor_id": 101,
>>       "actor_type": "Team",
>>       "bypass_mode": "always"
>>     }
>>   ]
>> }
>> /////////////////////////////////////////////////////////////////////////////////////////////////
>> {
>>   "id": 3,
>>   "name": "Secure core branches against deletions",
>>   "target": "branch",
>>   "source_type": "Repository",
>>   "source": "github",
>>   "enforcement": "active",
>>   "conditions": {
>>     "ref_name": {
>>       "exclude": [],
>>       "include": [
>>         "~DEFAULT_BRANCH",
>>         "refs/heads/test"
>>       ]
>>     }
>>   },
>>   "rules": [
>>     {
>>       "type": "deletion"
>>     },
>>     {
>>       "type": "non_fast_forward"
>>     }
>>   ],
>>   "bypass_actors": []
>> }
>> /////////////////////////////////////////////////////////////////////////////////////////////////
>> {
>>   "id": 4,
>>   "name": "Pull request requirements - Test branch",
>>   "target": "branch",
>>   "source_type": "Repository",
>>   "source": "github",
>>   "enforcement": "active",
>>   "conditions": {
>>     "ref_name": {
>>       "exclude": [],
>>       "include": [
>>         "refs/heads/test"
>>       ]
>>     }
>>   },
>>   "rules": [
>>     {
>>       "type": "pull_request",
>>       "parameters": {
>>         "required_approving_review_count": 0,
>>         "dismiss_stale_reviews_on_push": false,
>>         "require_code_owner_review": false,
>>         "require_last_push_approval": false,
>>         "required_review_thread_resolution": false
>>       }
>>     },
>>     {
>>       "type": "required_deployments",
>>       "parameters": {
>>         "required_deployment_environments": [
>>           "development"
>>         ]
>>       }
>>     }
>>   ],
>>   "bypass_actors": []
>> }
>> /////////////////////////////////////////////////////////////////////////////////////////////////
>> {
>>   "id": 5,
>>   "name": "Pull request requirements - Development branch",
>>   "target": "branch",
>>   "source_type": "Repository",
>>   "source": "github",
>>   "enforcement": "active",
>>   "conditions": {
>>     "ref_name": {
>>       "exclude": [],
>>       "include": [
>>         "~DEFAULT_BRANCH"
>>       ]
>>     }
>>   },
>>   "rules": [
>>     {
>>       "type": "pull_request",
>>       "parameters": {
>>         "required_approving_review_count": 1,
>>         "dismiss_stale_reviews_on_push": true,
>>         "require_code_owner_review": false,
>>         "require_last_push_approval": true,
>>         "required_review_thread_resolution": true
>>       }
>>     },
>>     {
>>       "type": "required_linear_history"
>>     }
>>   ],
>>   "bypass_actors": []
>> }
>> /////////////////////////////////////////////////////////////////////////////////////////////////
>> // TODO
>> //  Working Branches Naming Rule
>> //    Working_Branches[/]TeamPrefix-[0-9]{5}_?.*
>> //      Working_Branches/TeamPrefix-18742
>> 
>> // Release Branches Naming Rule
>> //    ^((?!(Release_Branches\/TeamPrefix_[0-9]{1,2}([.][0-9]{1,2}){3})|(Working_Branches\/TeamPrefix-[0-9]{5}_?.*))).*$
>> //      Release_Branches/TeamPrefix_2.4.1.0
>> //      Release_Branches/TeamPrefix_2.3.21.0
>> //      Release_Branches/TeamPrefix_2.3.20.0
>> //      Release_Branches/TeamPrefix_2.4.1.0
>> //      Release_Branches/TeamPrefix_2.3.16.0
>> //      Release_Branches/TeamPrefix_2.3.18.0
>> 
>> //      Working_Branches/TeamPrefix-19903_sample_pipeline
>> //      Working_Branches/TeamPrefix-20300_only_ods
>> //      Working_Branches/TeamPrefix-20302_update_date
>> //      Working_Branches/TeamPrefix-00000
>> 
>> //      Working_Branches/TeamPrefix_20300
>> //      Working_Branches/update_date
>> 
>> //      Release_Branches/TeamPrefix_2.4.1
>> 
>> //      Test_Branches/Test

> # GCP
> ## Authenticate to Artifact Registry
>> `gcloud auth configure-docker europe-west1-docker.pkg.dev`
> ## Get clusters
>> `gcloud container clusters list`
> ## Get credentials
>> * `gcloud container clusters get-credentials [clusterName] --region [regionName]`
>> * `gcloud container clusters get-credentials example-infrastructure-cluster --region europe-west`
> ## Login to GCP
>> * `gcloud auth login`
> ## Set project
>> * `gcloud config set project [projectName]`
>> * `gcloud config set project example-project-name`

> # Jira
> ## Filters
>> * All bugs assigned to component:
>>   * `project = {Project} AND component = {Component} AND type = Bug ORDER BY created DESC`
>> * Find Particular Label:
>>   * `project = {Project} AND component = {Component} AND labels = {Label} ORDER BY updated DESC`
>> * Others:
>>   * `project = {Project} AND component = {Component} AND type != Sub-task AND subject ~ "Release of TeamPrefix-" ORDER BY updated DESC`
>>   * `project = {Project} AND component = {Component} AND type != Sub-task AND resolved > 2023-04-18 ORDER BY updated DESC`
>>   * `project = {Project} AND component = {Component} AND type != Sub-task AND resolved > 2023-04-18 AND fixVersion = EMPTY ORDER BY updated DESC`
>>   * `project = {Project} AND component = {Component} AND type != Sub-task AND resolved > 2023-04-18 AND fixVersion = EMPTY AND assignee in ({UserId_1}, {UserId_2}, {...}) ORDER BY updated DESC`
> ## Sprint Managing
>> * Adding new sprint
>>   * -> Backlog -> Backlog (section) -> Create sprint
>>   * -> move issues which will be pending in new sprint
>> * Closing the sprint
>>   * -> All issues/sub-tasks have to be closed
>>   * -> All releasable issues must have assigned "Fix Version/s"
>>   * -> Active sprints -> Switch sprint -> Select proper sprint -> Complete sprint
> ## Automation
>> ### TODO
>>> *  Generating confluence review page / reports
>>> *  Auto mail from Jira

> # Kubernetes
> ## Add Kubernetes file
>> * `kubectl --namespace [namespace] apply -f [fullFileName]`
>> * `kubectl --namespace example-namespace apply -f nginx.yml`
>> * `kubectl --namespace example-namespace apply -f deployment.yml`
>> * `kubectl --namespace example-namespace apply -f svc.yml`
> ## Check version
>> `kubectl version`
> ## Cluster info
>> `kubectl cluster-info`
> ## Get namespace info
>> * `kubectl --namespace [namespace]`
>> * `kubectl --namespace example-namespace`
> ## Get namespace list
>> * `kubectl get ns`
> ## Get pod list
>> * `kubectl --namespace [namespace] get pod`
>> * `kubectl --namespace example-namespace get pod`
> ## Ingress adders
>> `35.244.178.180`
> ## Kubernetes installation (kubectl)
>> * [Download kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows)
>> * Add "kubectl.exe" to "Environments Variables: System -> Paths
> ## Role binding
>> * `kubectl --namespace [namespace] get rolebinding/[roleName] -o yaml`
>> * `kubectl --namespace frontend-conquerors get rolebinding/frontend-conquerors-team-members -o yaml`


> # Links
>> * [Mark Down Basic Syntax](https://www.markdownguide.org/basic-syntax/)
>> * [Microsoft VisualStudio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/)
>> * [Text generator](https://www.lipsum.com/)
>> * [AzureDevOps Pull Request Merge Conflicts Extension](https://praveenkumarsreeram.com/2022/05/05/azure-devops-tips-and-tricks-6-resolve-merge-conflicts-using-pull-request-merge-conflicts-azure-devops-extension/)
>> * [Debugging application's state changes: Redux DevTools](https://chromewebstore.google.com/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?pli=1)
>> * [Typing learn tool](https://www.keybr.com/)
>> * [Time required to brake a password](https://www.hivesystems.io/blog/are-your-passwords-in-the-green)
>> * [Rethink mandatory password changes](https://www.ftc.gov/policy/advocacy-research/tech-at-ftc/2016/03/time-rethink-mandatory-password-changes)
>> * [Most popular technologies 2022](https://survey.stackoverflow.co/2022/#most-popular-technologies-language-prof)
>> * [Interesting Lecture IT (Project Phoenix)](https://lubimyczytac.pl/ksiazka/308849/projekt-feniks-powiesc-o-it-modelu-devops-i-o-tym-jak-pomoc-firmie-w-odniesieniu-sukcesu)

> # Notepad++
>> #### [.md (Mark Down) - Basic Syntax](https://www.markdownguide.org/basic-syntax)
>> #### Word wrap:
>>> * → View → Word wrap
>> #### Configure Columns
>>> * → View → Configure Columns
>>>   * → Password:    UNCHECK
>>>   * → Attachments: UNCHECK
>> #### Replace Tab with spaces
>>> * → Settings → Preferences...
>>> * → Language → Tab Settings
>>>   * → Tab size → 2
>>>   * → Replace by space → TRUE
>> #### Install plugins
>>> * → Plugins → Plugins Admin
>>>   * → Compare
>>>   * → DSpellChecker

> # Outlook
> # Classic View
>> * -> View -> Current View -> Change View -> Compact
>> * -> View  -> Layout group -> Reading Pane -> Right
> # Layout
>> * -> View -> Layout -> Reading Pane -> Right
> # Auto Responder
>> * -> File -> Automatic Replies
>> ```txt
>> Dear Sender,
>> 
>> #v1:
>> I am currently Out of Office till {dddd, YYYY MMMM dd}.
>> If you need immediate assistance please contact:
>> {Team}: {TeamContact}
>> {Superior}: {SuperiorContact}
>> #v2:
>> I'm currently on leave till {dddd, YYYY MMMM dd}.
>> For things related to {Team} please forward mail to our DL: {TeamDL}
>> 
>> {Mail Signature}
> # Auto Responder
>> ```txt
>> Best regards
>> {FullName}
>> {Icon} {Role} | {Team}
>> {Icon} {Main Department} | {Sub-department} | {etc.}
>> {Icon} E-Mail: {CompanyMail}
>> {Company} Poland | {City} | {Address}
>> {CompanyHomePage}
> ## Check Grade / Level
>> * -> Home -> Address Book -> Find Person -> Properties -> Member of -> Grade
> ## Company Visual Identity - Wallpaper

> # Password safety
>> * [Time required to brake a password](https://www.hivesystems.io/blog/are-your-passwords-in-the-green)
>> * Password braking test hardware: 10 x RTX5090 (2025)
>> * Password considered as save when braking time > 10 billion years (10^10)
>> * Letters
>>   * \>= 18+
>> * Uppercase & Lowercase Letters
>>   * \>= 15
>> * Numbers & Uppercase & Lowercase Letters
>>   * \>= 15
>> * Special Characters & Numbers & Uppercase & Lowercase Letters
>>   * \>= 14

> # Postman
>> Request data -> Code  (Icon: </>)
>> Variable -> {{VariableName}}
>> Disable redirect -> Request -> Settings -> Automatically fallow redirects -> OFF
>> Authorization: 
>> ```json
>> {
>>     "apiKey": {
>>         "Key": "Authorization",
>>         "Value": "TokenName Token",
>>         "Add to": "Header"
>>     }
>> }

> # PostgreSQL
> ## Add Mock User
>> * `psql -h localhost -U postgres -p 5432 postgres`
>> * login with main password (passed during installation)
>> * CREATE ROLE mock_user WITH LOGIN CREATEDB ENCRYPTED PASSWORD
>> * `'SCRAM-SHA-256$4096:0CymgkTIKyl9DtgU0qMfvg==$fDjCCSLRyLXS9YnzULn5JH86q8k0jmOHsemk914xc+s=:LvC1p4O53uPny/9OTk/k6ZWfNVCPT7suwbu92Wqf60Q=';`
> ## Add PostgreSQL to Environment Variables
>> * `Windows` + `R` -> sysdm.cpl
>> * System Properties 
>>   *    Advanced -> Environment Variables
>> * Environment Variables
>>   * System variables -> Path -> Edit.. -> New -> `C:\Program Files\PostgreSQL\14\bin`
> ## Start PostgreSQL Server
>> * `pg_ctl -D "C:\Program Files\PostgreSQL\14\data" start`
>> * `pg_ctl -D "C:\Program Files\PostgreSQL\14\data" stop`
>> * `pg_ctl -D "C:\Program Files\PostgreSQL\14\data" restart`

> # Powers of Two
>> | Powers        | Value
>> | ------------- | ---------------------------
>> | 2^ 0          |                          1
>> | 2^ 1          |                          2
>> | 2^ 2          |                          4
>> | 2^ 3          |                          8
>> | 2^ 4          |                         16
>> | 2^ 5          |                         32
>> | 2^ 6          |                         64
>> | 2^ 7 (sbyte)  |                        128
>> | 2^ 8 (byte)   |                        256
>> | 2^ 9          |                        512
>> | 2^10          |                      1_024
>> | 2^11          |                      2_048
>> | 2^12          |                      4_096
>> | 2^13          |                      8_192
>> | 2^14          |                     16_384
>> | 2^15 (short)  |                     32_768
>> | 2^16 (ushort) |                     65_536
>> | 2^17          |                    131_072
>> | 2^18          |                    262_144
>> | 2^19          |                    524_288
>> | 2^20          |                  1_048_576
>> | 2^21          |                  2_097_152
>> | 2^22          |                  4_194_304
>> | 2^23          |                  8_388_608
>> | 2^24          |                 16_777_216
>> | 2^25          |                 33_554_432
>> | 2^26          |                 67_108_864
>> | 2^27          |                134_217_728
>> | 2^28          |                268_435_456
>> | 2^29          |                536_870_912
>> | 2^30          |              1_073_741_824
>> | 2^31 (int)    |              2_147_483_648
>> | 2^32 (uint)   |              4_294_967_296
>> | 2^61 (long)   |  9_223_372_036_854_775_808
>> | 2^62 (ulong)  | 18_446,744_073_709_551_615

> # Regex
>> | Function                  | Regex                | Replace   
>> | ------------------------- | -------------------- | -----------------------------------------------
>> | Trim line                 | `\s+$` (TODO: Exclude line brakes)
>> | Remove log time stamp     | `\[[^\s]*\]`         
>> | Remove crossed md lines   | `>> [*] [0-9]{2}:[0-9]{2} ~~[^~]*~~\|> ### ~~[^~]*~~`
>> | Readonly to property      | `private readonly string (\w+) = "(\w+)";` | `private string $1 { get => "$2"; }`
>> | Not Whitespace            | `[^\s]`
>> | Not Line                  | `^((?!<phrase>).)*$`
>> | Match anything            | `.*`
>> | Letters                   | `[A-Za-z]{Length}`
>> | Digits                    | `\d{Length}`
>> | Match exact 10 digits     | `^\d{10}$`
>> | .md correct link brackets | `[(]([^)]*)[)][[]([^\]]*)[\]]`             | `[$1]($2)`
>> | Start of the string       | `^`
>> | End of the string         | `$`
>> | Oracle date to PostgreSQL | `to_date\('(..)-(...)-(..)','DD-MON-RR'\)` | `'20$3-$2-$1'`
>> | Array format to enum      | ^
>> | -> Remove spaces          | `'([0-9]{3})': '(.*)',`                    | `$2 = $1,`
>> | -> To enum format         | ` : '([A-Za-z]*)[ -](.*)',`                | ` : '$1$2',`
>> ```csharp
>>  var friendlyHttpStatus = {
>>     '200': 'OK',
>>     '201': 'Created',
>>     '202': 'Accepted',
>>     '203': 'Non-Authoritative Information',
>>     '204': 'No Content',
>>     '205': 'Reset Content',
>>     '206': 'Partial Content',
>>     '300': 'Multiple Choices',
>>     '301': 'Moved Permanently',
>>     '302': 'Found',
>>     '303': 'See Other',
>>     '304': 'Not Modified',
>>     '305': 'Use Proxy',
>>     '306': 'Unused',
>>     '307': 'Temporary Redirect',
>>     '400': 'Bad Request',
>>     '401': 'Unauthorized',
>>     '402': 'Payment Required',
>>     '403': 'Forbidden',
>>     '404': 'Not Found',
>>     '405': 'Method Not Allowed',
>>     '406': 'Not Acceptable',
>>     '407': 'Proxy Authentication Required',
>>     '408': 'Request Timeout',
>>     '409': 'Conflict',
>>     '410': 'Gone',
>>     '411': 'Length Required',
>>     '412': 'Precondition Required',
>>     '413': 'Request Entry Too Large',
>>     '414': 'Request-URI Too Long',
>>     '415': 'Unsupported Media Type',
>>     '416': 'Requested Range Not Satisfiable',
>>     '417': 'Expectation Failed',
>>     '418': 'I\'m a teapot',
>>     '429': 'Too Many Requests',
>>     '500': 'Internal Server Error',
>>     '501': 'Not Implemented',
>>     '502': 'Bad Gateway',
>>     '503': 'Service Unavailable',
>>     '504': 'Gateway Timeout',
>>     '505': 'HTTP Version Not Supported',
>> };

> # Software
>> | Purpose                   | Software             | Link   
>> | ------------------------- | -------------------- | -----------------------------------------------
>> | Angular / TypeScript builder   | [Node.js](https://nodejs.org/en/download/current)
>> | Azure local emulation          | Azurite + [Azure Storage Explorer](https://azure.microsoft.com/en-us/products/storage/storage-explorer)
>> | API client (service) generator | NSwag
>> | Common Notepad                 |   One Note / SharePoint + Notepad++
>> | Creating `.setup` files        | [Inno Setup](https://jrsoftware.org/isdl.php#stable)
>> | Designing HTTP requests        | [SwaggerHub](https://swagger.io/tools/swaggerhub)
>> | Documenting HTTP requests      | Swagger
>> | PDF Editing                    | [PDF-XChange Viewer](https://www.tracker-software.com/product/pdf-xchange-viewer)
>> | SVG Editor                     | [BOXY SVG (online)](https://boxy-svg.com/app)
>> | Git                            | [Git](https://git-scm.com/download/win)
>> | Git Manager                    | [Git Extensions](http://gitextensions.github.io)
>> | HTTP requests                  | [Postman](https://www.postman.com/downloads)
>> | IT Communicator                | Slack / Microsoft Teams
>> | Learning platforms             | Udemy / Pluralsight
>> | Multiple Node Versions         | NVM
>> | Desktop recording (Windows 10) | [ActivePresenter](https://swagger.io/tools/swaggerhub)
>> | Desktop recording (Windows 11) | (Windows Snip & Sketch) [Snipping Tool](https://www.microsoft.com/en-us/p/snip-sketch/9mz95kl8mr0l?activetab=pivot:overviewtab)
>> | Notepad                        | [Notepad++](https://notepad-plus-plus.org/downloads)
>> | NuGet manager                  | [NuGet Package Explorer](https://apps.microsoft.com/store/detail/nuget-package-explorer/9WZDNCRDMDM3?hl=pl-pl&gl=pl)
>> | Password manager (offline)     | [KeePass](https://keepass.info)
>> | Password manager (online)      | [Bitwarden](https://bitwarden.com/pricing)
>> | Picking system colors          | [PowerToys - Color Picker](https://github.com/microsoft/PowerToys/releases/tag/v0.82.0)
>> | Folders Compare                | [WinMerge](https://winmerge.org)
>> | Screenshots editor             | [Screenpresso](https://www.screenpresso.com/download)
>> | Screenshots tool               | (Windows Snip & Sketch) [Snipping Tool](https://www.microsoft.com/en-us/p/snip-sketch/9mz95kl8mr0l?activetab=pivot:overviewtab)
>> | Code Sharing (online)          | [CodeShare](https://codeshare.io)
>> | Speech to Text                 | (Phone) Samsung Recorder
>> | Fixing Windows 11 taskbar      | [StartAllBack](https://www.startallback.com)
>> | Virtual desktops               | [Citrix Receiver](https://www.citrix.com/pl-pl/downloads/citrix-receiver)
>> | .Net Editor (IDE)              | (Visual Studio) Diagnostic Tools
>> | .Net code inspection           | [ReSharper CLI](https://www.jetbrains.com/help/resharper/ReSharper_Command_Line_Tools.html)
>> | .Net analyzer                  | [StyleCop Analyzers](https://www.nuget.org/packages/stylecop.analyzers)
>> | .NET Assembly Browsing         | [dotPeek](https://www.jetbrains.com/decompiler)
>> | Code Editor                    | [Visual Studio Code](https://code.visualstudio.com/#alt-downloads)

> # SQL
> ## CASE - Switch - WHEN ... THEN
>> ```sql
>> CASE
>>   WHEN Condition_1 THEN 1
>>   WHEN Condition_2 THEN 0
>>   ELSE -1
>> END AS [ColumnName]
> ## CAST - Type
>> ```sql
>> CAST('2025-01-07T14:11:28.4200000' AS DATETIME2)
>> CAST(0 AS BIT) - compare to bool false: [TableA].[Boolean] = CAST(0 AS BIT)
> ## CONVERT (Type)
>> ```sql
>> CONVERT(DATE, [DateTime]) AS [Date]
> ## COUNT (Counters Table)
>> ```sql
>> DECLARE @Counter_A INT;
>> DECLARE @Counter_B INT;
>> 
>> SELECT @Counter_A = COUNT (*) FROM [Table_A];
>> SELECT @Counter_B = COUNT (*) FROM [Table_B];
>> 
>> CREATE TABLE [#CountersTable]
>> (
>> 	[A] INT,
>> 	[B] INT,
>> );
>> 
>> INSERT [#CountersTable] VALUES (@Counter_A, @Counter_B);
>> 
>> SELECT * FROM [#CountersTable];
> ## DELETE
>> ```sql
>> DELETE 
>>   FROM users
>>   WHERE user_name NOT IN ('domain\egid1', 'domain\egid2', 'domain\egid3')
>> ;
> ## SELECT INTO, CREATE TABLE (Temp)
>> ```sql
>> DROP TABLE IF EXISTS [#TempTable]
>> 
>> -- a. SELECT INTO
>> SELECT *
>>   INTO [#TempTable]
>>   FROM [Knowledge].[dbo].[Skills]
>> ;
>> 
>> -- b. CREATE TABLE
>> CREATE TABLE [#TempTable]
>> (
>> 	[Id] INT NOT NULL,
>> 	[Boolean] BIT NOT NULL,
>> 	[Date] DATE NOT NULL,
>> );
>> 
>> SELECT *
>> 	FROM [#TempTable]
>> ;
> ## DECLARE, SET, PRINT  (Variables)
>> ```sql
>> DECLARE @DefaultDate DATE = '2000-01-01'
>> 
>> DECLARE @Name NVARCHAR(255), @Id INT
>> SET @Name= 'Test-Name'
>> 
>> DECLARE @NewRecordIndex INT;
>> SELECT @NewRecordIndex = MAX([Id]) 
>>   FROM [TableA]
>> ;
>> 
>> PRINT @NewRecordIndex
> ## DECLARE CURSOR (Update Cursor)
>> ```sql
>> DECLARE @NewRecordData INT;
>> DECLARE UpdateCursor CURSOR FOR
>>     SELECT *
>>     FROM [TableA]
>>   OPEN UpdateCursor
>>     FETCH NEXT FROM UpdateCursor INTO @NewRecordData
>>     WHILE @@FETCH_STATUS = 0 BEGIN
>>     
>>       INSERT INTO [TableA] ([RecordData])
>>       VALUES (@NewRecordData);
>>       
>>       FETCH NEXT FROM UpdateCursor INTO @NewRecordData
>>     END
>>   CLOSE UpdateCursor
>>   DEALLOCATE UpdateCursor
>> ;
> ## EXEC (Execute Procedure)
>> ```sql
>> DECLARE 
>>     @Arg_1 INT           = 1,
>>     @Arg_2 BIGINT        = 900000000000,
>>     @Arg_3 NVARCHAR(255) = 'Test-Arg',
>>     @Arg_4 DATE          = '2000-01-01';
>> 
>> EXEC PREFIX_PROCEDURE_NAME @Argument_1 = @Arg_1, @Argument_2 = @Arg_2,
>>                            @Argument_3 = @Arg_3,  @Argument_4 = @Arg_4
>>;
> ## GROUP BY
>> ```sql
>> SELECT COUNT([Id]) AS [Count], MIN([Time]) AS [MinTime]
>>   FROM [TableA]
>>   GROUP BY [Column_1]
>>   HAVING [Column_2] = 1
>> ;
> ## IF
>> ```sql
>> IF @Boolean = 1 BEGIN
>>   {SQL Code}
>> END ELSE BEGIN
>>   {SQL Code}
>> END
>> 
>> IF @Boolean = 1
>>   {SQL Code}
>> ELSE
>>   {SQL Code}
> ## IF (In Line If)
>> ```sql
>> IIF(Condition, 1, 0) AS [ColumnName]
> ## INSERT
>> ```sql
>> DECLARE @Name NVARCHAR(50) = 'ExampleName'
>> 
>> SET IDENTITY_INSERT [TableA] ON
>> 
>> INSERT [TableA] ([Id], [Name]) 
>>   VALUES (1, @Name)
>> ;
>> 
>> SET IDENTITY_INSERT [TableA] OFF
> ## JOIN
>> ```sql
>> SELECT *
>>   FROM [TableA] AS a
>> -- a. INNER JOIN
>>      INNER JOIN [TableB] AS b ON a.[Id] = b.[AId]
>> -- b. LEFT  JOIN
>>      LEFT  JOIN [TableB] AS b ON a.[Id] = b.[AId]
>> -- c. RIGHT  JOIN
>>      RIGHT JOIN [TableB] AS b ON a.[Id] = b.[AId]
>> ;
>>
>> -- d. QUERY JOIN
>> SELECT *
>>   FROM [TableA] AS a
>>   LEFT JOIN (
>>     SELECT *
>>     FROM [TableC] AS c
>>     INNER JOIN [TableD] AS d ON c.[Id] = d.[CId]
>>   ) AS b ON a.[Id] = b.[AId]
>> ;
> ## ORDER BY
>> ```sql
>> ORDER BY [Id] ASC <=> ORDER BY [Id]
>> ORDER BY [Id] DESC
>> 
>> ORDER BY IIF([NullableColumn] IS NULL, 1, 0), [NullableColumn] - Nulls at the end
> ## RAISERROR
>> ```sql
>> RAISERROR (40409, -1, -1,  'Message');
> ## SELECT TOP (Limit data to 1000 rows)
>> ```sql
>> SELECT TOP (1000) *
>>   FROM [TableA]
>> ;
> ## TRUNCATE TABLE (Remove all rows)
>> ```sql
>> TRUNCATE TABLE [TableA]
> ## OTHER QUERIES
>> ```sql
>> SELECT 
>>     ORDER_ID, 
>>     TO_CHAR(ORDER_CREATION_DATE, 'YYYY-MM-DD') AS CREATION_DATE, 
>>     TO_CHAR(ORDER_REQUESTED_DISPATCH_DATE, 'YYYY-MM-DD') AS REQUESTED_DISPATCH_DATE, 
>>     ORDER_SOURCE, ORDER_CURRENT_STATUS, ROUND((ORDER_REQUESTED_DISPATCH_DATE - ORDER_CREATION_DATE)/365, 2) AS DATE_DIF_IN_DAYS
>> FROM ORDERS_VW
>> WHERE ORDER_REQUESTED_DISPATCH_DATE IS NOT NULL
>>       AND ORDER_CURRENT_STATUS IS < 90
>> ORDER BY DATE_DIF_IN_DAYS DESC
>> ;
> ## Export all data from table to an insertable sql format
>> 1. Right click database
>> 1. Point to tasks In SSMS 2017 you need to ignore step 2 - the generate scripts options is at the top level of the context menu.
>> 1. Select generate scripts
>> 1. Click next
>> 1. Choose tables
>> 1. Click next
>> 1. Click advanced
>> 1. Scroll to Types of data to script - Called types of data to script in SMSS 2014
>> 1. Select data only
>> 1. Click on 'Ok' to close the advanced script options window
>> 1. Click next and generate your script
> ## SQL server start error
>> "The request failed or the service did not respond in a timely fashion.<br>
>> Consult the event log or other applicable error logs for details."
>> * Open SQL Server Configuration manager
>>   * Click on the SQL Server Services (on the left)
>>   * Double-click on the SQL Server Instance that I wanted to start
>>   * Select the Built-in account radio button in the Log On tab and choose Local system from the drop-down menu
>>   * Click apply at the bottom, then right click the instance and select Start

> # UiPath
> ## API key UiPath
>> * https://cloud.uipath.com/capgeminipolska/portal_/licensing
>>   * -> Admin
>>   * -> Licenses
>>   * -> Other
>>   * -> Document Understanding
>> * Endpoints:
>>   * https://docs.uipath.com/automation-cloud/docs/about-licensing#document-understanding-endpoints
> ## Academy
>> * https://academy.uipath.com/channel
>> * https://academy.uipath.com/learning-plans
>> * Credentials: Microsoft Account
>> * Developer Path, REFramework (Robotic Enterprise)
>> #### [UiPath (Studio Community Edition)](https://cloud.uipath.com/capgeminipolska/portal_/resourceCenter)
>> #### [UiPath (Pro Community)](https://download.uipath.com/beta/UiPathStudioSetup.exe)
>> ## UiPath Robot Config
>> * UiRobot: %LocalAppdata%\UiPath\UiPath.Agent.exe
>>   * -> Orchestrator Settings
>> * User (RobotName): corp\ksledz
>> * Add Robot to Environments 
>> * Management -> Robots -> Environments
>> * Machine: [MachineId]
>> * OrchestratorUrl: [URL]
>> * MachineKey: [Password]

> # UML
> ## Access Modifiers
>> | Modifier | Scope
>> | -------- | -----------
>> | `+`      | public
>> | `~`      | package
>> | `#`      | protected
>> | `/`      | derived
>> | `-`      | private

> # Visual Studio
> ## .NET add nuget source
>> ```powershell
>> dotnet nuget add source "$NUGET_SOURCE" \g
>>     --name "$NUGET_NAME" \
>>     --username "$NUGET_USERNAME" \
>>     --password "$NUGET_PASSWORD" \
>>     --store-password-in-clear-text
> ## Snippets path (VS 019)
>> `C:\Program Files (x86)\Microsoft Visual Studio\2019\Professional\VC#\Snippets\1033\Visual C#\`
> ## Code metrics
>> * -> Project name
>>   * -> Analyze and Code Cleanup
>>   * -> Calculate Code Metrics
> ## Default projects directory
>> `%UserProfile%\source\repos`
> ## Free licenses
>> * Apache 2.0, 
>> * MIT 
>> * BSD 3
> ## Line Indicator 80 / 120 characters line
>> * [Editor Guidelines](https://marketplace.visualstudio.com/items?itemName=PaulHarrington.EditorGuidelines)
> ## [MSDN](https://my.visualstudio.com/Downloads/Featured)
> ## Open File 
>> `ctrl` + `t`
> ## Wrap Lines
>> `ctrl` + `e`, `ctrl` + `w`
> ## The file is locked by another process
>> * Warning MSB3026: Could not copy to Beginning retry 1 in 1000ms. The process cannot access the file because it is being used by another process. The file is locked by "{PID}"
>> * `Get-Process  -Id {PID}`
>> * `Stop-Process -Id {PID}`
> ## SpellChecker (VS2022) (NOT CHECKS CODE)
>> * [VisualStudio.SpellChecker.VS2022AndLater](https://marketplace.visualstudio.com/items?itemName=EWoodruff.VisualStudioSpellCheckerVS2022andLater)
> ## Nuget - Package sources
>> * -> Tools -> Options...
>> * -> NuGet Package Manager -> Package sources
>>   * -> [nuget.org](https://api.nuget.org/v3/index.json)
>>   * -> [Microsoft Visual Studio Offline Packages](file:///C:/Program%20Files%20(x86)/Microsoft%20SDKs/NuGetPackages)

> # Visual Studio Code
> ## Keyboard Shortcuts
>> | Shortcuts              | Key combination
>> | ---------------------- | ------------------------------
>> | Multi point editing    | `alt`  + `click`
>> | Move line              | `alt`  + `↑` / `↓`
>> | Duplicate line         | `alt`  + `shift` + `↑` / `↓`
>> | Select line            | `crtl` + `l`
>> | Open terminal          | `crtl` + `` ` ``
>> | Open new terminal      | `crtl` + `shift` + `` ` ``
>> | Move by word           | `crtl` + `←` / `→`
>> | Clear terminal         | `crtl` + `k `
>> | Next same word         | `crtl` + `d` (+ `click`)
>> | Command palette        | `crtl` + `p`
>> | Search command         | `>` (command palette)
>> | Search paragraph       | `@` (command palette)
>> | Reopen last closed tab | `ctrl` + `shift` + `t`
>> | Run and debug          | `ctrl` + `shift` + `d`
>> | Open commands list     | `ctrl` + `shift` + `p`
>> | Open Markdown Preview to the Side   | `ctrl` + `K`, `V`
>> | Open Markdown Preview to the Window | `ctrl` + `shift` + `v`
> ## Add VS Code as default git editor
>> `git config --global core.editor "code --wait"`


> # Windows
> ## Windows Explorer Environment Variables
>> | Name                 | Variable                                     | Value
>> | -------------------- | -------------------------------------------- |  -----------------------------
>> | AppData (Roaming)    | `%AppData%`                                  | `C:\Users\{User}\AppData\Roaming`
>> | AppData (Local)      | `%LocalAppData%`                             | `C:\Users\{User}\AppData\Local`
>> | User Profile         | `%UserProfile%`                              | `C:\Users\{User}`
>> | Local Hosts file     | `%SystemRoot%\System32\drivers\etc\hosts`    | `C:\Windows\System32\drivers\etc\hosts`
>> | Start Menu (System)  | `%ProgramData%\Microsoft\Windows\Start Menu` | `C:\ProgramData\Microsoft\Windows\Start Menu`
>> | Start Menu (User)    | `%AppData%\Microsoft\Windows\Start Menu`     | `%AppData%\Microsoft\Windows\Start Menu`
>> | Startup (User)       | `%AppData%\Microsoft\Windows\Start Menu\Programs\Startup` | `%AppData%\Microsoft\Windows\Start Menu\Programs\Startup`
>> | All User's Profile   | `%AllUsersProfile%`              | `C:\ProgramData`
>> | Program Files Common | `%CommonProgramFiles%`           | `C:\Program Files\Common Files`
>> | Program Files Common  x86 | `%CommonProgramFiles(x86)%` | `C:\Program Files (x86)\Common Files`
>> | Home Drive           | `%HomeDrive%`                    | `C:\`
>> | Program Data         | `%ProgramData%`                  | `C:\ProgramData`
>> | Program Files        | `%ProgramFiles%`                 | `C:\Program Files or C:\Program Files`
>> | Program Files x86    | `%ProgramFiles(x86)%`            | `C:\Program Files (x86)`
>> | Public Users         | `%Public%`                       | `C:\Users\Public`
>> | System Drive         | `%SystemDrive%`                  | `C:\`
>> | Windows (System Root) | `%SystemRoot%`                  | `C:\Windows`
>> | AppData Temp         | `%Temp%`                         | `C:\Users\{User}\AppData\Local\Temp`
>> | CMD History Folder   | `%AppData%\Microsoft\Windows\PowerShell\PSReadLine`
> ## Windows Shortcuts
>> | Shortcuts                | Key combination
>> | ------------------------ | ------------------------------
>> | Permanently delete files | `shift` + `delete`
>> | Run commands             | `win`   + `r`
>> | Clipboard history        | `win`   + `v`
> ## Change installations default directory
>> * -> `win` + `r`
>> * -> regedit
>> * -> HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion
>>   * -> Permissions...
>>   * -> ALL APPLICATION PACKAGES
>>   * -> Advanced
>>   * -> Owner: Administrators -Change-> Add AD USER
>>     * ProgramFilesDir (x86)
>>     * ProgramFilesDir
> ## Change Outlook download default folder
>> * -> `win` + `r` 
>> * -> regedit
>> * -> HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office\[16.0]\Outlook\Options
>>   * -Add   -> 	String Value 
>>   * -Name  -> 	DefaultPath 
>>   * -Modify-> 	`%UserProfile%\Downloads`
> ## Change screenshots default directory
>> * -> `%UserProfile%\Pictures\Screenshots`
>>   * -> Properties
>>   * -> Localization
>>   * -> `%UserProfile%\Downloads\0.Screenshots`
> ## Compatibility Appraiser
>> * -> Start -> Task Scheduler
>>   * -> `Task Scheduler Library\Microsoft\Windows\Application Experience`
>>   * -> Microsoft Compatibility Appraiser -> Disable
> ## Environment Variables
>> * a. Windows 
>>   * -> Edit the system environment variables
>>   * -> Environment Variables
>>     * -> System variables -> Path -> Edit.. -> New -> [PasteYourPath]
>> * b. `win` + `r`-> sysdm.cpl
>> * System Properties 
>>   * -> Advanced -> Environment Variables
>>     * -> System variables -> Path -> Edit.. -> New -> [PasteYourPath]
>> * **REQUIRES RESTART OF TERMINAL!!!**
> ## Not found edgegdi.dll
>> * -> copy: 
>>   * `%SystemRoot%\SysWOW64\`    
>>   * `EdgeManager.dll`
>> * -> paste to:
>>   * `%SystemRoot%\System32\`
>>   * -> rename pasted EdgeManager.dll to: Edgegdi.dll
> ## Init config
>> ### Classic File Explorer context menu
>>> * -> `win` + `r` -> regedit 
>>>   * -> HKEY_CURRENT_USER\SOFTWARE\CLASSES\CLSID
>>>   * -> New -> Key -> {86ca1aa0-34aa-4e8b-a509-50c905bae2a2}
>>>   * -> New -> Key -> InprocServer32 (left default value blank)
>> ### Enable clipboard history
>>> `win` + `v`
>>> * -> Windows -> Settings
>>> * -> System -> Clipboard
>>> * -> Clipboard history: ON
>> ### File Explorer
>>> * -> File Explorer -> Menu (...) -> Options 
>>>   * -> Open File Explorer to: -> "{User}"
>>>   * -> Privacy
>>>     * -> Show recently used files   -> UNCHECK
>>>     * -> Show frequently used files -> UNCHECK
>>>     * -> Show recommended section   -> UNCHECK
>>>   * -> View 
>>>     * -> Advanced settings:
>>>       * -> Hidden files and folders -> Show hidden files, folders and drivers -> CHECK
>>>       * -> Hide extensions for known file types -> UNCHECK
>> ### Windows Languages
>>> * -> Windows right click -> Setting
>>>   * -> Time & language
>>>   * -> Language & region 
>>>   * -> Options
>>>   * -> Keyboards -> Add a keyboard
>>>     * -> Polish (Programmers)
>>>     * -> Remove others
>>>   * -> Language features -> Download all
>> ### (Windows 11) Restore old menu start
>>> * -> Win + R
>>>   * -> regedit
>>>     * -> Computer\HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced
>>>     * -> new -> DEWORD (32-bit) value
>>>       * -> Value data: 1
>> ### Other / TODO
>>> * -> File Explorer -> File -> Change folder and search options
>>> * -> Open File Explorer to: -> This PC / Quick Access
>>> * Set Default Folder When Opening Explorer in Windows 10
>>> * HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced
>>> * -> New Shortcut -> Downloads -> %UserProfile%/Downloads

> ## KeyTool certificates
>> * `keytool -import -noprompt -trustcacerts -alias "[aliasName]" -file [fullPath] -cacerts`
> ## PenDrive boot
>> * -> CMD
>>   * ->  DiskPart
>>     * -> list disk
>>     * -> select disk {X}
>>     * -> detail disk
>>     * -> clean
>>     * -> create partition primary
>>     * -> active
>>     * -> format fs=ntfs quick
>>     * -> assign
> ## Run commands
>> `win` + `r`
>> ### Control Panel
>>> | Command           | Tool Name
>>> | ----------------- | ------------------------------------
>>> | access.cpl        | Accessibility Options
>>> | hdwwiz.cpl        | Add New Hardware Wizard
>>> | appwiz.cpl        | Add/Remove Programs
>>> | timedate.cpl      | Date and Time Properties
>>> | desk.cpl          | Display Properties
>>> | inetcpl.cpl       | Internet Properties
>>> | joy.cpl           | Joystick Properties
>>> | main.cpl keyboard | Keyboard Properties
>>> | main.cpl          | Mouse Properties
>>> | ncpa.cpl          | Network Connections
>>> | ncpl.cpl          | Network Properties
>>> | telephon.cpl      | Phone and Modem options
>>> | powercfg.cpl      | Power Management
>>> | intl.cpl          | Regional settings
>>> | mmsys.cpl sounds  | Sound Properties
>>> | mmsys.cpl         | Sounds and Audio Device Properties
>>> | sysdm.cpl         | System Properties
>>> | nusrmgr.cpl       | User settings
>>> | firewall.cpl      | Firewall Settings (sp2)
>>> | wscui.cpl         | Security Center (sp2)
>>> | wupdmgr           | Takes you to Microsoft Windows
>> ### Management Consoles
>>> | Command      | Tool Name
>>> | ------------ | ------------------------------------
>>> | certmgr.msc  | Certificate Manager
>>> | ciadv.msc    | Indexing Service
>>> | compmgmt.msc | Computer management
>>> | devmgmt.msc  | Device Manager
>>> | dfrg.msc     | Defragment
>>> | diskmgmt.msc | Disk Management
>>> | fsmgmt.msc   | Folder Sharing Management
>>> | eventvwr.msc | Event Viewer
>>> | gpedit.msc   | Group Policy (< XP Pro)
>>> | iis.msc      | Internet Information Services
>>> | lusrmgr.msc  | Local Users and Groups
>>> | mscorcfg.msc | Net configurations
>>> | ntmsmgr.msc  | Removable Storage
>>> | perfmon.msc  | Performance Manager
>>> | secpol.msc   | Local Security Policy
>>> | services.msc | System Services
>>> | wmimgmt.msc  | Windows Management
>> ### Microsoft Office
>>> | Command  | Tool Name
>>> | -------- | ------------------------------------
>>> | winword  | Microsoft Word
>>> | excel    | Microsoft Excel
>>> | powerpnt | Microsoft PowerPoint
>>> | msaccess | Microsoft Access
>>> | outlook  | Microsoft Outlook
>>> | ois      | Microsoft Picture Manager
>>> | winproj  | Microsoft Project
>> ### System
>>> | Command    | Tool Name
>>> | ---------- | ------------------------------------
>>> | calc       | Calculator
>>> | cfgwiz32   | ISDN Configuration Wizard
>>> | charmap    | Character Map
>>> | chkdisk    | Repair damaged files
>>> | cleanmgr   | Cleans up hard drives
>>> | clipbrd    | Windows Clipboard viewer
>>> | cmd        | Opens a new Command Window (cmd.exe)
>>> | control    | Displays Control Panel
>>> | dcomcnfg   | DCOM user security
>>> | debug      | Assembly language programming tool
>>> | defrag     | Defragmentation tool
>>> | drwatson   | Records programs crash & snapshots
>>> | dxdiag     | DirectX Diagnostic Utility
>>> | explorer   | Windows Explorer
>>> | fontview   | Graphical font viewer
>>> | ftp        | ftp.exe program
>>> | hostname   | Returns Computer's name
>>> | ipconfig   | Displays IP configuration for all network adapters
>>> | jview      | Microsoft Command line Loader for Java classes
>>> | mmc        | Microsoft Management Console
>>> | msconfig   | Configuration to edit startup files
>>> | msinfo32   | Microsoft System Information Utility
>>> | nbtstat    | Displays stats and current connections using NetBios over TCP/IP
>>> | netstat    | Displays all active network connections
>>> | nslookup   | Returns your local DNS server
>>> | odbcad32   | ODBC Data Source Administrator
>>> | ping       | Sends data to a specified host/IP
>>> | regedit    | registry Editor
>>> | regsvr32   | register/deregister DLL/OCX/ActiveX
>>> | regwiz     | Registration wizard
>>> | scannow    | System File Checker1
>>> | sfc        | System File Checker
>>> | sndrec32   | Sound Recorder
>>> | sndvol32   | Volume control for sound card
>>> | sysedit    | Edit system startup files (config.sys, autoexec.bat, win.ini, etc.)
>>> | systeminfo | display various system information in text console
>>> | taskmgr    | Task manager
>>> | telnet     | Telnet program
>>> | taskkill   | kill processes using command line interface
>>> | tskill     | reduced version of Task kill from Windows XP Home
>>> | tracert    | Traces and displays all paths required to reach an internet host
>>> | winchat    | simple chat program for Windows networks
>>> | winipcfg   | Displays IP configuration
> ## Snip & Sketch 
>> * Cache folder
>>   * `%LocalAppData%/Packages/Microsoft.ScreenSketch_8wekyb3d8bbwe/TempState`
>> * [Clipboard autosave](#enable-clipboard-history)


> # TODO BlossomCookbook 
>> * Settings polish, english languages
>> * Easy recipe searching
>> * Responsive application
>> * Usage of free icons and images
>> * Automated creation of shopping list
>> * Recalculating of meal portions

> # Linguistic
> ## Acronyms
>> * ADK (Assessment and Deployment Kit)
>> * ADO (Azure DevOps)
>> * ATP (Available To Promise)
>> * BA (Business Analyst)
>> * C2C (Consumer to Consumer)
>> * CAS (Code Access Security)
>> * CFF (Customer Fulfillment Flow)
>> * CLR (Common Language Runtime)
>> * COCO (Company Owned Company Operated)
>> * CODO (Company Owned Dealer Operated)
>> * CR (Check and Repair)
>> * CRM (Customer Relationship Management)
>> * CRT (Commercial Road Transport)
>> * CTE (Common Test Environment)
>> * CWIS (Central Warehousing Information Service)
>> * DOD (Definition Of Done)
>> * DODO (Dealer Owned Dealer Operated)
>> * DoD (Definition of Done)
>> * DoR (Definition of Ready)
>> * EID (Enterprise IDentifier)
>> * EM (Engagement Manager)
>> * ERP (Enterprise Resource Planning)
>> * FEP (Front End Processor)
>> * FOE (Full Operational Environment)
>> * FYI (For Your Information)
>> * GDPR (General Data Protection Regulation - European equivalent of RODO)
>> * GOES (Global Order Enrichment Service)
>> * GPO (Group Policy Object)
>> * HKL (Hardware Lab Kit)
>> * HOS (Hospital Shop Visit)
>> * I&D (Inclusively & Diversity)
>> * IKP (Internal Knowledge Portal)
>> * INC (INCident)
>> * CF (Customer Fulfillment)
>> * LCM (Live Cycle Management)
>> * ISC (Interbase Server Connection)
>> * IoC (Inversion of Control)
>> * IoT (Internet of Things)
>> * JIT (Just In Time)
>> * JWT (JSON Web Token)
>> * LCM (Life Cycle Management)
>> * LTSA (Long-Term Service Agreement)
>> * LTSC (Long-Term Servicing Channel)
>> * MHS (Material Handling System)
>> * MR&O (Maintenance, Repair and Overhaul)
>> * MSAL (MicroSoft Authentication Library)
>> * NFR (Non Functional Requirements)
>> * NSC (NearShore Center)
>> * OOP (Object Oriented Programming)
>> * OP (Operator Preference)
>> * PBI (ProBlem Investigation)
>> * PBI (Product Backlog Item)
>> * PCI (Payment Cart Industry)
>> * PDL (Personal Development Lead)
>> * PM (Project Manager)
>> * PMO (Project Management Office)
>> * POC (Proof of Concept)
>> * PPE (Pre-Production Environment)
>> * PRB (PRoBlem)
>> * PTE (Project Test Environment)
>> * RCA (Root Cause Analysis)
>> * REFramework (Robotic Enterprise Framework)
>> * REST (Representational State Transfer)
>> * RF (ReFurbishment)
>> * RPI (Robotic Process Automation)
>> * SDK (Software Development Kit)
>> * SDL (Security Development Lifecycle)
>> * SDL (Software Delivery Lied)
>> * SLF (Service Level Fulfillment)
>> * SLM (Sales Location Management)
>> * SW (Scenario Workbench)
>> * TAP (Temporary Access Password)
>> * TID (Tenant IDentifier)
>> * TPI (Third (3rd) Party Issuers)
>> * TPN (Third (3rd) Party Networks)
>> * TRT (Turn Round Time)
>> * TSA Catalogue (Technical Services/Store Assets Catalogue)
>> * UAT (User Acceptance Test)
>> * VAI (Virtual Access Interface)
>> * WDK (Windows Driver Kit)
> ## Monospace Fonts
>>> * [Monospace Fonts](https://en.wikipedia.org/wiki/List_of_monospaced_typefaces)
>>> * Windows Fonts `%SystemRoot%/Fonts`
>>>   * Anonymous Pro
>>>   * Bitstream Vera Sans Mono
>>>   * Cascadia Code
>>>   * Century Schoolbook Monospace
>>>   * Comic Mono
>>>   * Computer Modern Mono/Typewriter
>>>   * Consolas
>>>   * Courier
>>>   * Cousine
>>>   * DejaVu Sans Mono
>>>   * Droid Sans Mono
>>>   * Envy Code R
>>>   * Everson Mono
>>>   * Fantasque Sans
>>>   * Fira Code
>>>   * Fira Mono
>>>   * Fixed
>>>   * Fixedsys
>>>   * FreeMono
>>>   * Go Mono
>>>   * Hack
>>>   * HyperFont
>>>   * IBM MDA
>>>   * IBM Plex Mono
>>>   * Inconsolata
>>>   * Input
>>>   * Iosevka
>>>   * JetBrains Mono
>>>   * JuliaMono
>>>   * Letter Gothic
>>>   * Liberation Mono
>>>   * Lucida Console
>>>   * Menlo
>>>   * Monaco
>>>   * Monofur
>>>   * Monospace (Unicode)
>>>   * Nimbus Mono L
>>>   * NK57 Monospace
>>>   * Noto Mono
>>>   * OCR-A
>>>   * OCR-B
>>>   * Operator Mono
>>>   * Overpass Mono
>>>   * Oxygen Mono
>>>   * PragmataPro
>>>   * Prestige Elite
>>>   * ProFont
>>>   * PT Mono
>>>   * Recursive Mono
>>>   * Roboto Mono
>>>   * SF Mono
>>>   * Source Code Pro
>>>   * Spleen
>>>   * Terminus
>>>   * Tex Gyre Cursor
>>>   * Ubuntu Mono
>>>   * Victor Mono
>>>   * Wumpus Mono
> ## Names Metonyms
>> | Norway               | John Smith, Fred Bloggs, Joe Bloggs, Joe Public, Jane Public, Tommy Atkins
>> | Switzerland, Austria | Average Joe, Ordinary Joe, John Doe, Joe Sixpack, Jane Doe, Plain Jane
>> | Czech Republic       | Wasylij Pupkin, Iwan Pietrowicz
>> | Italy                | Kalle Svensson
>> | Finland              | Max Mustermann, Erika Mustermann, Otto Normalverbraucher
>> | France               | Jan Modaal, Jan MetDePet
>> | Ireland              | Ola Nordmann
>> | Israel               | Hans Meier
>> | Spain                | Jan Novák
>> | Japan                | Mario Rossi
>> | Serbia               | Matti Meikäläinen, Maija Meikäläinen
>> | Lithuania            | Jean Dupont, Monsieur Durand
>> | Dominican Republic   | Seán Citizen
> ## NATO Phonetic Alphabet
>> | Char | Phonetic Name
>> | ---- | ---------------
>> | `A`  | Alpha
>> | `B`  | Bravo
>> | `C`  | Charlie
>> | `D`  | Delta
>> | `E`  | Echo
>> | `F`  | Foxtrot
>> | `G`  | Golf
>> | `H`  | Hotel
>> | `I`  | India
>> | `J`  | Juliet
>> | `K`  | Kilo
>> | `L`  | Lima
>> | `M`  | Mike
>> | `N`  | November
>> | `O`  | Oscar
>> | `P`  | Papa
>> | `Q`  | Quebec
>> | `R`  | Romeo
>> | `S`  | Sierra
>> | `T`  | Tango
>> | `U`  | Uniform
>> | `V`  | Victor
>> | `W`  | Whiskey
>> | `X`  | X-ray
>> | `Y`  | Yankee
>> | `Z`  | Zulu
> ## Special Characters
>> | Char | Name
>> | ---- | ---------------------------------------------------------------------------------------
>> | `~`  | Tilde
>> | `'`  | Acute, back quote, grave, grave accent, left quote, open quote, a push
>> | `!`  | Exclamation mark
>> | `@`  | At, At symbol, Ampersat, Arobase
>> | `#`  | Hash, Sharp, Octothorpe, Number, Pound
>> | `$`  | Dollar sign, Generic currency
>> | `%`  | Percent
>> | `^`  | Caret, Circumflex
>> | `&`  | Ampersand, And symbol
>> | `*`  | Asterisk, Multiplication symbol, Star symbol
>> | `(`  | Parenthesis (left), Open
>> | `)`  | Parenthesis (right), Close
>> | `-`  | Dash, Minus, Hyphen
>> | `–`  | Long dash
>> | `_`  | Underscore
>> | `+`  | Plus
>> | `=`  | Equal
>> | `{`  | Curly bracket, Open brace, Squiggly brackets
>> | `}`  | Curly bracket, Close brace, Squiggly brackets
>> | `[`  | Square brackets, Open bracket
>> | `]`  | Square brackets, Closed bracket
>> | `\|` | Pipe, Vertical bar
>> | `\`  | Backslash, Reverse solidus
>> | `/`  | Forward slash, Solidus, Virgule, Whack, Division symbol
>> | `:`  | Colon
>> | `;`  | Semicolon
>> | `\`  | Quote, Quotation mark, Inverted commas
>> | `'`  | Apostrophe, Single quote
>> | `<`  | Less than, Angle brackets
>> | `>`  | Greater than, Angle brackets
>> | `,`  | Comma
>> | `.`  | Dot, Full stop, Period
>> | `?`  | Question mark
>> | `	` | Tab
> ## Vocabulary (EN -> PL)
>> | EN                  | PL
>> | ------------------- | --
>> | ingenuity
>> | pitching
>> | pitch
>> | nurture
>> | conduct
>> | retaliation
>> | entrepreneurs
>> | prudent
>> | counsel
>> | bid
>> | pears
>> | unconscious bias
>> | hell of a mess    | niezły bałagan / burdel

----------------------------------------------------------------------------------------------------
## To Clean up
Win: otwieranie domyślnie "File Explorer'a" w "Downloads"
Win: zapamiętywanie rozmiaru okien (half screen'y)
Obliczyć powierzchnię monitorów
    15.6 + 15.6
    15.6 + 24
    15.6 + 24 +32
    15.6 + 27 +32
Write function wich create file with each ascii sign 
    https://www.asciitable.com/
----------------------------------------------

AlgoSec
SendGrid
1password

Azure AD TID (Tenant IDentifier)

DATA SOURCE=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=example.host.com)(PORT=1521))(CONNECT_DATA=(SERVICE_NAME=service.name.com)));USER ID=db_user;PASSWORD=secret_password;Pooling=true; Min Pool Size=5; Max Pool Size=10; Incr Pool Size=5; Decr Pool Size=2; Statement Cache Size=20; Connection Timeout=800; Connection Lifetime=800

set NODE_OPTIONS=--openssl-legacy-provider;  ng run AppName:build:development

Kubernetes
  PostgreSQL port-forward
    kubectl -n frontend-conquerors port-forward svc/pg-sqlproxy-gcloud-sqlproxy 5433:5432
    Auth error:
      gcloud auth application-default login
    
230s AppService limitation
  https://learn.microsoft.com/en-us/troubleshoot/azure/app-service/web-apps-performance-faqs#why-does-my-request-time-out-after-230-seconds

this.notificationService.showSuccess(
  'Upload of the Excel template was successful.<br/>Please check the table below for format errors'
  'Upload successful'
);

Add PickingGroups API

var r = OracleParametersToString(storeProcedureParameters);
public static string OracleParametersToString(OracleParameter oracleParameters)
{
  var stringParameters = oracleParameters.Select(oracleParameter 
  {
    if (!string.IsNullOrWhiteSpace(oracleParameter?.Value?.ToString()))
    {
      return $"\"{oracleParameter.ParameterName}\": \"{oracleParameter.Value}\"";
    }
    return null;
  }).Where(text text != null);
  var result = string.Join("\n" stringParameters);
  return result;
}

{
  const response: any = ;
  spyOn(component as any 'updateUnitLoadIds').and.returnValue(of(response))
  component.ngOnInit();
  fixture.detectChanges();
  await fixture.whenStable();
}

bind my name użwać lepiej bo jak zabraknie leci kolejnością i można ławo przestwić i błąd gotowy
cast oracle connection potrzebne do bind by name

supplier_with_available_qty_v1(bu_code_sup_in  IN     wis_gui_obj_types.t_bu_code
                              item_no_in      IN     wis_gui_obj_types.t_item_no
                              lu_in           IN     wis_gui_obj_types.t_chr_value
                              xml_out         OUT     wis_gui_obj_types.t_xmltype)

using System.ComponentModel.DataAnnotations.Schema;
using System.Xml.Serialization;

{
    [Table("TABLE_VW" Schema = "SCHEMA_NAME")]
    public class Item
    {
        public string ItemNumber { get; set; }
        public string ItemType { get; set; }
        public string ItemName { get; set; }
    }
}


<fa-icon class="dropdown-icon" ="faCaretDown"></fa-icon>

<button _ngcontent-eyx-c233="" class="primary-button save-button ng-star-inserted" disabled=""> Proceed with order lines </button>

<button _ngcontent-lwv-c175="" class="primary-button save-button ng-star-inserted" disabled=""> Proceed with order lines </button>

<button _ngcontent-lwv-c174="" class="primary-button add-order-line-button"><fa-icon _ngcontent-lwv-c174="" class="ng-fa-icon"><svg role="img" aria-hidden="true" focusable="false" data-prefix="fas" data-icon="plus" class="svg-inline--fa fa-plus fa-w-14" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512"><path fill="currentColor" d="M416 208H272V64c0-17.67-14.33-32-32-32h-32c-17.67 0-32 14.33-32 32v144H32c-17.67 0-32 14.33-32 32v32c0 17.67 14.33 32 32 32h144v144c0 17.67 14.33 32 32 32h32c17.67 0 32-14.33 32-32V304h144c17.67 0 32-14.33 32-32v-32c0-17.67-14.33-32-32-32z"></path></svg></fa-icon> Add order line
</button>

this.notificationService.showError(
        'There was something wrong with the used template which is why it was not possible to upload the data. Please download the correct template in the pop-up you will see after choosing the upload as method for order creation.'
        'Problem with Excel template'
      );


if (!await userPrivilegeService.DoesUserHaveCreateShowroomOrderPrivilege(cancellationToken compartment userEmail))               
            {
                return OperationResult<byte>.Failure($"User '{userEmail}' does not have permissions to generate order report");
            }
      
if (!await userPrivilegeService.DoesUserHaveCreateReturnOrderPrivilege(cancellationToken compartment userEmail))               
            {
                return OperationResult<byte>.Failure($"User '{userEmail}' does not have permissions to generate order report");
            }
      
      DoesUserHaveCreateReturnOrderPrivilege(cancellationToken
                                        compartmentParameter
                                        userEmail);

public async Task<IActionResult> GetOrderReport([HttpTrigger(AuthorizationLevel.Anonymous "get" Route = "OrderReport/{compartment}/{orderType}/{referenceId}")]HttpRequest request

Request 1password:
  •  Go to https://cybersec.ingka.com
    -> Products -> 1password -> Personal Vault
    -> accept Terms -> Order
  •  On your email box you will get 1password incitation "Join on 1Password"
    -> Join now -> save your "Secret Key" (it will be required for longing from different devices)
    -> Set your master password. It should be strong cause it provides access to whole your and team vault. It should have at least 16 characters, capital letter, small letter, digit, special character
  •  Login in to https://ikea.1password.com
  •  Ask Scrum Master/Tech Lead to add you in to group "Group Name"
    
Adding/Removing members:
  -> Login in to https://ikea.1password.com -> Groups (right panel) 
  -> Select proper group e.g. "Group Name" -> Mange
  -> search user to add / uncheck user to remove
Turn on two-factor authentication for your 1Password account
https://support.1password.com/two-factor-authentication

----------------------------------------------------------------------------------------------------
## Cucumber: Setup Project (NOT WORKING)
https://cucumber.io/docs/guides/10-minute-tutorial/

PowerShell -> mvn archetype:generate (>> = shift + enter)
>> "-DarchetypeGroupId=io.cucumber" 
>> "-DarchetypeArtifactId=cucumber-archetype"
>> "-DarchetypeVersion=7.0.0"
>> "-DgroupId=hellocucumber"
>> "-DartifactId=hellocucumber"
>> "-Dpackage=hellocucumber"
>> "-Dversion=1.0.0-SNAPSHOT"
>> "-DinteractiveMode=false"

PowerShell -> 
    mvn archetype:generate  "-DarchetypeGroupId=io.cucumber" "-DarchetypeArtifactId=cucumber-archetype" "-DarchetypeVersion=7.0.0" "-DgroupId=hellocucumber" "-DartifactId=hellocucumber" "-Dpackage=hellocucumber" "-Dversion=1.0.0-SNAPSHOT" "-DinteractiveMode=false"

Install Maven
	go to -> https://maven.apache.org/download.cgi -> download
	add path to "Environment Variables" (MAY REQUIRE SYSTEM RESTART)
	verify Maven installation -> PowerShell -> mvn -v

Install Gradle
    go to -> https://gradle.org/releases/ -> download binary-only version
        add path to "Environment Variables" (MAY REQUIRE SYSTEM RESTART)
    verify Gradle installation -> PowerShell -> gradle -v

PowerShell -> cd hellocucumber -> 
  gradle init
    Found a Maven build. Generate a Gradle build from this? -> yes
        Select build script DSL:
            1: Groovy
            2: Kotlin
        Enter selection -> 1
        Generate build using new APIs and behavior -> no


build.gradle -> open file -> add sections ->
	configurations {
		cucumberRuntime {
			extendsFrom testImplementation
		}
	}
	
	task cucumber() {
		dependsOn assemble, testClasses
		doLast {
			javaexec {
				main = "io.cucumber.core.cli.Main"
				classpath = configurations.cucumberRuntime + sourceSets.main.output + sourceSets.test.output
				args = ['--plugin', 'pretty', '--glue', 'hellocucumber', 'src/test/resources']
			}
		}
	}

PowerShell ->
    mvn test
    gradle cucumber
