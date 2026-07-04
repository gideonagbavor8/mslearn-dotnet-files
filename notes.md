# CSE 325 - Assignment Notes

## Part 1: ContosoPizza Web API Evidence

GET /pizza/ response showing existing records and added Hawaiian pizza:

```json
[
  { "id": 1, "name": "Classic Italian", "isGlutenFree": false },
  { "id": 2, "name": "Veggie", "isGlutenFree": true },
  { "id": 3, "name": "Hawaiian", "isGlutenFree": false }
]
```

## Part 2: Sales Summary Function

```csharp
void GenerateSalesReport(IEnumerable<string> salesFiles, double salesTotal, string outputDir)
{
    var report = new StringBuilder();

    report.AppendLine("Sales Summary");
    report.AppendLine("----------------------------");
    report.AppendLine($" Total Sales: {salesTotal:C}");
    report.AppendLine();
    report.AppendLine(" Details:");

    foreach (var file in salesFiles)
    {
        string salesJson = File.ReadAllText(file);
        SalesData? data = JsonConvert.DeserializeObject<SalesData?>(salesJson);
        double fileTotal = data?.Total ?? 0;

        var fileName = Path.GetFileNameWithoutExtension(file);
        report.AppendLine($"  {fileName}: {fileTotal:C}");
    }

    File.WriteAllText(Path.Combine(outputDir, "salesReport.txt"), report.ToString());
    Console.WriteLine($"Sales report generated at: {Path.Combine(outputDir, "salesReport.txt")}");
}
```