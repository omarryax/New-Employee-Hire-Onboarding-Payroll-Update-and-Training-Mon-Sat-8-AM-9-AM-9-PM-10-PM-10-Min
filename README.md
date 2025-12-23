# New Employee Onboarding & Payroll Automation Workflow

## 📋 Project Overview

This project contains an automated workflow for handling new employee onboarding, payroll setup, and training management. 

### Workflow Purpose

The automation streamlines the entire new employee onboarding process by:
- Triggering when a new employee is added to BambooHR
- Fetching employee data and metadata from various systems
- Creating onboarding tasks in Asana
- Setting up payroll sheets in Google Sheets
- Managing team structures and assignments
- Generating appointment letters and documents
- Handling time off policies and training schedules

**Schedule**: Runs Monday-Saturday, 8 AM/9 AM - 9 PM/10 PM, every 10 minutes

---

## 🗂️ Project Files

- **`New Employee edited.json`** - Original Make.com workflow export
- **`New Employee n8n.json`** - Converted n8n workflow (ready to import)
- **`README.md`** - This documentation file

---

## 🔄 Conversion Process

This workflow was automatically converted from Make.com to n8n format using a custom conversion script. The conversion process:

1. **Module Mapping**: Converted Make.com modules to equivalent n8n nodes
2. **Parameter Translation**: Transformed Make.com parameters to n8n format
3. **Expression Conversion**: Updated Make.com mapper syntax (`{{module.field}}`) to n8n expressions (`{{ $('NodeName').item.json.field }}`)
4. **Connection Preservation**: Maintained all workflow connections and routing logic
5. **Nested Routes**: Handled complex routing structures (BasicRouter modules)

### Key Conversions

| Make.com Module | n8n Node Type |
|----------------|---------------|
| `bamboohr:TriggerNewEmployee` | `n8n-nodes-base.bambooHrTrigger` |
| `bamboohr:GetEmployee` | `n8n-nodes-base.bambooHr` |
| `asana:CreateTask` | `n8n-nodes-base.asana` |
| `google-sheets:updateCell` | `n8n-nodes-base.googleSheets` |
| `builtin:BasicRouter` | `n8n-nodes-base.switch` |
| `builtin:BasicFeeder` | `n8n-nodes-base.splitInBatches` |
| `builtin:BasicAggregator` | `n8n-nodes-base.aggregate` |
| `http:Action*` | `n8n-nodes-base.httpRequest` |

---

## 🚀 Setup Instructions

### Prerequisites

1. **n8n Installation**
   - Install n8n (self-hosted or cloud)
   - Ensure n8n version supports all required node types

2. **Required Services & Credentials**
   - BambooHR account with API access
   - Hubstaff account with API credentials
   - Asana workspace with API token
   - Google Sheets API credentials
   - APITemplate.io account (for PDF generation)
   - HTTP endpoints for file operations

### Installation Steps

1. **Import Workflow**
   ```
   1. Open n8n
   2. Go to Workflows → Import from File
   3. Select "New Employee n8n.json"
   4. Click Import
   ```

2. **Configure Credentials**
   
   For each service, set up credentials in n8n:
   
   - **BambooHR**: 
     - Go to Credentials → Add Credential
     - Select "BambooHR"
     - Enter your API key and subdomain
   
   - **Asana**:
     - Add Asana credential
     - Enter Personal Access Token
     - Select workspace ID
   
   - **Google Sheets**:
     - Add Google Sheets credential
     - Authenticate with OAuth2
     - Grant necessary permissions
   
   - **Hubstaff**:
     - Configure HTTP Request nodes with Hubstaff API base URL
     - Add authentication headers

3. **Update Node Credentials**
   - Click on each node that requires credentials
   - Select the appropriate credential from the dropdown
   - Save the workflow

4. **Review and Adjust Parameters**
   - Check all HTTP Request nodes for correct URLs
   - Verify Google Sheets spreadsheet IDs
   - Update Asana workspace and project IDs
   - Review all expression references

5. **Test the Workflow**
   - Use "Test workflow" button
   - Check execution logs for errors
   - Verify data flow between nodes

---

## 📊 Workflow Structure

### Main Flow

```
1. BambooHR Trigger (New Employee)
   ↓
2. Hubstaff API Call (Get Users)
   ↓
3. Router (Conditional Logic)
   ├─→ Get Employee Details (BambooHR)
   │   ↓
   │   Split Users Array (Feeder)
   │   ↓
   │   Get Asana Custom Field
   │   ↓
   │   Create Asana Task
   │   ↓
   │   Google Sheets Operations
   │   ├─→ List Sheets
   │   ├─→ Update Cells
   │   ├─→ Create/Update Sheets
   │   └─→ Payroll Data Management
   │   ↓
   │   BambooHR API Calls
   │   ├─→ Assign Time Off Policy
   │   └─→ Update Employee Data
   │   ↓
   │   Generate Documents (APITemplate)
   │   ↓
   │   HTTP Operations (File Management)
   │
   └─→ Alternative Route (Direct API Calls)
       ↓
       Asana Team Management
       ├─→ List Teams
       ├─→ Create Team (if missing)
       ├─→ Create Project
       ├─→ Create Portfolio
       └─→ Add User to Team
```

### Node Count

- **Total Nodes**: 79
- **Trigger Nodes**: 1 (BambooHR)
- **Action Nodes**: 78
- **Router/Switch Nodes**: Multiple (for conditional logic)
- **Aggregator Nodes**: Multiple (for data collection)

---

## 🔧 Node Types Used

### Integration Nodes

- **BambooHR** (`n8n-nodes-base.bambooHr`, `n8n-nodes-base.bambooHrTrigger`)
  - Trigger: New employee creation
  - Actions: Get employee, API calls, update employee data

- **Asana** (`n8n-nodes-base.asana`)
  - Get custom fields
  - Create tasks
  - Create teams
  - Create projects
  - Create portfolios
  - Add users to teams

- **Google Sheets** (`n8n-nodes-base.googleSheets`)
  - List sheets
  - Update cells
  - Rename sheets
  - Copy sheets
  - Add new sheets
  - API calls for advanced operations

- **HTTP Request** (`n8n-nodes-base.httpRequest`)
  - Hubstaff API calls
  - File downloads/uploads
  - APITemplate.io PDF generation
  - Custom API integrations

### Control Flow Nodes

- **Switch** (`n8n-nodes-base.switch`)
  - Conditional routing (replaces Make.com BasicRouter)
  - Multiple output paths based on conditions

- **Split in Batches** (`n8n-nodes-base.splitInBatches`)
  - Process arrays item by item (replaces Make.com BasicFeeder)

- **Aggregate** (`n8n-nodes-base.aggregate`)
  - Combine multiple items (replaces Make.com BasicAggregator)

- **Set** (`n8n-nodes-base.set`)
  - Set variables
  - Transform data to JSON

---

## ⚙️ Configuration Details

### Key Parameters

#### BambooHR Trigger
- **Event**: `employeeCreated`
- **Fields Retrieved**: Department, Division, Employment Status, First Name, Last Name, Hire Date, Email, Job Title, Location, Pay Rate, Pay Schedule, Manager, etc.

#### Google Sheets Operations
- Multiple spreadsheet operations for payroll management
- Cell updates with employee data
- Sheet creation and management
- Dynamic sheet naming based on employee information

---

## ⚠️ Important Notes

### Manual Adjustments Required

1. **Credentials Setup**
   - All `__IMTCONN__` references need manual credential configuration
   - Each service requires proper authentication setup

2. **Expression Review**
   - Some complex Make.com expressions may need manual conversion
   - Check expressions using Make.com-specific functions
   - Verify all node name references in expressions

3. **Switch Node Rules**
   - Router nodes (BasicRouter) converted to Switch nodes
   - Switch rules need to be configured manually based on your logic
   - Review conditional routing paths

4. **API Endpoints**
   - Verify all HTTP Request URLs are correct
   - Update base URLs if needed
   - Check authentication headers

5. **Field Mappings**
   - Some field IDs (like BambooHR custom fields) may need adjustment
   - Verify Google Sheets cell ranges
   - Check Asana workspace/project IDs

6. **Schedule Configuration**
   - Original schedule: Mon-Sat, 8 AM/9 AM - 9 PM/10 PM, every 10 min
   - Configure n8n schedule trigger if needed
   - Adjust based on your requirements

### Testing Recommendations

1. **Start with Test Mode**
   - Use n8n's "Test workflow" feature
   - Test each branch individually
   - Verify data transformations

2. **Check Execution Logs**
   - Review execution history
   - Check for errors or warnings
   - Verify data flow between nodes

3. **Validate Outputs**
   - Confirm Asana tasks are created correctly
   - Verify Google Sheets updates
   - Check document generation
   - Validate API responses

4. **Error Handling**
   - Add error handling nodes where needed
   - Set up notifications for failures
   - Implement retry logic if required

---

## 🔍 Troubleshooting

### Common Issues

1. **Credential Errors**
   - **Problem**: Nodes show authentication errors
   - **Solution**: Verify credentials are properly configured and have correct permissions

2. **Expression Errors**
   - **Problem**: Expressions not resolving correctly
   - **Solution**: Check node names match exactly, verify JSON paths exist

3. **Connection Issues**
   - **Problem**: Nodes not connecting properly
   - **Solution**: Review connections in workflow, ensure proper data flow

4. **API Rate Limits**
   - **Problem**: API calls failing due to rate limits
   - **Solution**: Add delays between requests, implement rate limiting

5. **Data Format Mismatches**
   - **Problem**: Data not in expected format
   - **Solution**: Add data transformation nodes, verify field mappings

---

## 📝 Maintenance

### Regular Tasks

- Monitor workflow executions for errors
- Update credentials before expiration
- Review and update field mappings as services change
- Adjust schedules based on business needs
- Keep n8n and node packages updated

### Version Control

- Export workflow JSON regularly for backup
- Document any manual changes made
- Keep track of credential updates
- Maintain changelog for workflow modifications

---

## 📚 Additional Resources

- [n8n Documentation](https://docs.n8n.io/)
- [BambooHR API Documentation](https://documentation.bamboohr.com/docs)
- [Asana API Documentation](https://developers.asana.com/docs)
- [Google Sheets API Documentation](https://developers.google.com/sheets/api)
- [Hubstaff API Documentation](https://developer.hubstaff.com/)

---

## 📄 License

This workflow is provided as-is for internal use. Ensure compliance with all integrated service terms of service and data privacy regulations.

---

## 👤 Support

For issues or questions regarding this workflow:
1. Check n8n execution logs
2. Review node-specific documentation
3. Verify service API status
4. Consult n8n community forums

---

**Last Updated**: January 2025  
**Workflow Version**: 1.0  
**n8n Compatibility**: n8n 1.0+

