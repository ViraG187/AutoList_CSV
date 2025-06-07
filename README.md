# AutoList_CSV
PowerShell program to make file name list in excel with your format automatically

$folder = Get-Location
$folder: A variable. PowerShell variables always start with $.

=: Assignment operator — assigns the output of the right-hand side to the variable on the left.

Get-Location: A cmdlet (PowerShell command) that gets the current working directory (like pwd in Bash).

🔁 So: You are storing the current folder path into a variable called $folder.

🔹 $output = "$folder\file_list.csv"
"...": This is a double-quoted string, which allows variable expansion (injects $folder value).

\: This is a file path separator in Windows.

Result: This creates a file path like C:\Users\YourName\file_list.csv.

📝 "$folder\file_list.csv" turns into something like "C:\WorkFolder\file_list.csv"

🔹 $counter = 1
Sets up a variable $counter to keep track of serial numbers, starting from 1.

🔹 $results = @()
@(): This creates an empty array in PowerShell.

$results: You are making a list (array) to store rows of data.

🔹 Get-ChildItem -Path $folder -File
Get-ChildItem: A cmdlet to list all items (files/folders) in a directory.

-Path $folder: Tells it which folder to look in (using your earlier $folder variable).

-File: Limits the results to only files (ignores folders).

🔹 | ForEach-Object { ... }
|: This is the pipeline symbol. It passes the output of the left-hand command to the command on the right.

ForEach-Object: Loops over each item passed in the pipeline (each file).

{ ... }: A script block — the code inside {} runs for every file.

🔹 Inside { ... }
powershell
Copy
Edit
$results += [PSCustomObject]@{
    "Sr."            = $counter
    "File Name"      = $_.Name
    ...
}
$results += ...: += means append to the array $results.

[PSCustomObject]: Creates a custom table-like row (structured object).

@{}: This creates a hashtable (key-value pairs) for column names and their values.

"Sr." = $counter: Sets a column called "Sr." to the value in $counter.

"File Name" = $_.Name: $_ is a special variable that refers to the current item in the loop (each file). Name is a property of the file (its name).

"": Empty strings for user-fillable columns like "Doc Type", etc.

$counter++: Increments the counter by 1 for the next row.

🔹 $results | Export-Csv -Path $output -NoTypeInformation -Encoding UTF8
$results: The full array of objects.

|: Pipe to...

Export-Csv: Exports the array into a CSV file.

-Path $output: The file to save to (path defined earlier).

-NoTypeInformation: Hides the #TYPE line in the first row of the CSV.

-Encoding UTF8: Makes the CSV readable by Excel with Unicode characters.

🔹 Start-Process "explorer.exe" -ArgumentList "/select,$output"
Start-Process: Launches a new process (opens an app).

"explorer.exe": Opens Windows File Explorer.

-ArgumentList "/select,$output": Tells it to open File Explorer and highlight the newly created CSV file.
