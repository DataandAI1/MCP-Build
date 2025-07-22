# Tableau Metadata Extractor

A Python GUI application that extracts comprehensive metadata from Tableau workbooks (.twb and .twbx files) and exports it to a structured CSV format.

## Features

- **Comprehensive Field Extraction**: Extracts all fields, dimensions, measures, and calculated fields from Tableau workbooks
- **Calculation Resolution**: Automatically resolves calculation IDs to human-readable names/aliases
- **Worksheet Usage Tracking**: Shows which fields are used in which worksheets
- **Connection Details**: Captures data source connection information
- **Formula Cleaning**: Converts cryptic calculation references (e.g., `[Calculation_1071856779099492353]`) to readable names (e.g., `[This Year]`)
- **Batch Processing**: Process multiple Tableau files in a single run
- **User-Friendly GUI**: Simple interface for selecting input/output folders

## Output Format

The tool generates a single CSV file (`tableau_metadata.csv`) with the following columns:

- **Column ID**: Auto-incrementing identifier
- **Column Name**: Technical field name
- **Column Alias**: User-friendly display name
- **Field Type**: Dimension, Measure, Calculated Field, Table Calculation, etc.
- **Connection Name**: Data source connection details
- **Connection Alias**: Data source display name
- **datatype**: Tableau data type (string, integer, etc.)
- **role**: Field role (dimension/measure)
- **Calculation Formula**: Cleaned formula with resolved references
- **Original Calculation**: Raw formula with calculation IDs
- **Calc Clean Status**: Success, Partially Resolved, Unresolved References, or No Calculation
- **Field Used in Worksheets**: Yes/No indicator
- **Worksheet Name**: Name of worksheet using the field
- **File Name**: Source Tableau file

## Requirements

- Python 3.x
- Standard library only (no external dependencies)
- tkinter (usually included with Python)

## Usage

1. Run the script:
   ```bash
   python tableau_metadata_extractor.py
   ```

2. In the GUI:
   - Click "Browse..." next to "Input Folder" and select the folder containing your Tableau files
   - Click "Browse..." next to "Output Folder" and select where you want the CSV output saved
   - Click "Extract Metadata" to process all .twb and .twbx files

3. The tool will:
   - Scan all subdirectories for Tableau files
   - Extract metadata from each file
   - Create one row per field-worksheet combination
   - Save results to `tableau_metadata.csv` in your output folder

## How It Works

1. **XML Parsing**: Tableau workbooks are XML-based, so the tool parses the structure
2. **TWBX Handling**: For packaged workbooks (.twbx), it extracts the embedded .twb file
3. **ID Resolution**: Builds a mapping of calculation IDs to their captions/aliases
4. **Multi-Pattern Matching**: Handles various calculation reference formats:
   - `[Calculation_1234567890123]`
   - `[1234567890123]`
   - `@{1234567890123}`

## Example

If your Tableau workbook contains a calculation like:
```
[Calculation_1071856779099492353] / [Calculation_1071856779102896130]
```

The tool will output the cleaned formula as:
```
[This Year] / [Weeks Cover LW]
```

## Troubleshooting

- **Empty columns**: Ensure your Tableau files are saved with captions/aliases for calculated fields
- **Unresolved references**: Some calculation IDs may not have corresponding definitions in the workbook
- **Permission errors**: Make sure you have read access to input files and write access to the output folder

## License

This tool is provided as-is for metadata extraction and documentation purposes.