# Excel to JSON Integration Feature

This feature enables seamless conversion of ExamRoom.AI startup configuration files (Excel) into JSON format for better data management and API integration.

## Overview

The `startup_questions_config.json` file contains all test rules and configuration parameters needed to send configurations to ExamRoom.AI. This JSON format allows for:

- Better data management and version control
- Easy API integration
- Structured configuration validation
- Programmatic test rule management
- Integration with automation workflows

## File Structure

### Main Sections

#### 1. **Metadata**
Contains information about the conversion process:
- `source_file`: Original Excel filename
- `conversion_date`: Timestamp of conversion
- `converter_version`: Version of conversion tool
- `description`: File purpose

#### 2. **Test Configuration**
Organized test rules by category:

##### `organization_info`
- Organization name representation on ExamRoom
- Candidate/organization payment responsibility
- Candidate ID creation preferences

##### `contact_info`
- Proctor contact information
- Proctor response procedures for various scenarios

##### `platform_config`
- Assessment platform details
- Test console URL
- Exam URLs and access details

##### `exam_settings`
- Timer behavior on disconnection
- Answer saving preferences
- Candidate reconnection rules

##### `proctoring_config`
- Test access credential requirements
- Proctor response procedures
- Technical support protocols

##### `other_settings`
- Testing rules and eligibility requirements
- Database/LMS integration details
- Check-in procedures and identification methods
- Exam rules and regulations

#### 3. **Additional Exams Info**
Extended configuration for specific exam scenarios:
- Special check-in procedures
- Technology requirements (apps, extensions)
- Candidate registration processes
- Examination-specific configurations

## JSON Structure Example

```json
{
  "metadata": {
    "source_file": "Startup Questions.xlsx",
    "conversion_date": "2026-03-23T21:13:41.629319",
    "converter_version": "1.0",
    "description": "ExamRoom.AI Startup Questions - Test Rules Configuration"
  },
  "test_configuration": {
    "organization_info": {
      "How would you like your organization's name to appear on ExamRoom?": {
        "answer": null,
        "comments": null
      }
    },
    "contact_info": { ... },
    "platform_config": { ... },
    "exam_settings": { ... },
    "proctoring_config": { ... },
    "other_settings": { ... }
  },
  "additional_exams_info": [ ... ]
}
```

## Usage

### Converting Excel to JSON

```bash
python3 scripts/excel_to_json_converter.py
```

This will:
1. Analyze the Excel structure
2. Extract all questions and answers
3. Categorize them by configuration type
4. Output `startup_questions_config.json`

### Programmatic Usage

```python
from scripts.excel_to_json_converter import convert_excel_to_json

# Convert file
result = convert_excel_to_json('Startup Questions.xlsx', 'config.json')

# Access specific configuration
org_info = result['test_configuration']['organization_info']
exam_settings = result['test_configuration']['exam_settings']
```

### API Integration

The JSON can be used to populate ExamRoom.AI API requests:

```javascript
// Example: Send configuration to ExamRoom.AI
const config = await fetch('startup_questions_config.json').then(r => r.json());

const payload = {
  organization: config.test_configuration.organization_info,
  examSettings: config.test_configuration.exam_settings,
  proctoring: config.test_configuration.proctoring_config,
  // ... other fields
};

await fetch('https://api.examroom.ai/configuration', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify(payload)
});
```

## Categories

### Organization Info (3 items)
- Organization name preferences
- Payment responsibility model
- Candidate ID creation method

### Contact Info (2 items)
- Support contact details
- Incident response procedures

### Platform Config (3 items)
- Assessment platform type
- Console URLs
- Exam access URLs

### Exam Settings (2 items)
- Timer behavior on disconnection
- Answer persistence on disconnection

### Proctoring Config (2 items)
- Access credential requirements
- Technical support response procedures

### Other Settings (44 items)
- Eligibility requirements and sources
- Check-in procedures
- Identification methods
- Registration requirements
- Technology requirements
- Exam rules and regulations

## Data Flow

```
Startup Questions.xlsx
        ↓
[Excel to JSON Converter]
        ↓
startup_questions_config.json
        ↓
[API Integration] → ExamRoom.AI Platform
        ↓
[Test Rules Applied]
        ↓
[Exam Administration]
```

## Scripts

### `excel_to_json_converter.py`

**Purpose**: Converts Excel startup questions into structured JSON format

**Functions**:
- `load_excel_file()`: Loads and validates Excel file
- `extract_startup_questions()`: Parses questions from sheet
- `convert_excel_to_json()`: Main conversion orchestration
- `analyze_excel_structure()`: Diagnostic analysis tool

**Output**: `startup_questions_config.json`

## Integration Workflow

1. **Capture Configuration**: Client fills out `Startup Questions.xlsx`
2. **Convert to JSON**: Run converter script
3. **Validate**: Check JSON structure and completeness
4. **Store**: Commit to version control
5. **Deploy**: Send to ExamRoom.AI API
6. **Monitor**: Track configuration in dashboard

## Benefits

✅ **Version Control**: Track configuration changes over time
✅ **Automation**: Programmatically generate and update rules
✅ **Integration**: Easy API communication
✅ **Validation**: Structured data prevents configuration errors
✅ **Portability**: JSON format works across systems
✅ **Documentation**: Self-documenting configuration structure

## Technical Details

- **Language**: Python 3.6+
- **Dependencies**: `openpyxl` (Excel reading)
- **Input Format**: `.xlsx` (Excel)
- **Output Format**: `.json` (JSON)
- **Encoding**: UTF-8
- **Size**: ~12 KB per configuration file

## Error Handling

The converter includes:
- File validation and error reporting
- Missing file detection
- Invalid Excel format handling
- UTF-8 encoding support
- Row extraction verification

## Future Enhancements

- [ ] JSON to Excel conversion (reverse)
- [ ] Configuration validation schema
- [ ] API direct upload integration
- [ ] Configuration diff and merge tools
- [ ] Multi-organization configuration management
- [ ] Configuration templates
- [ ] Batch processing for multiple files

## Support

For questions about the conversion process:
1. Review sample output in `startup_questions_config.json`
2. Check Excel file structure matches expected format
3. Verify all required sheets are present
4. Run diagnostic with `analyze_excel_structure()`

---

**Created**: March 23, 2026
**Feature Branch**: `feature/excel-to-json-integration`
**Status**: Active Development
