# IT Service Request Processing Automation

**Week 3 Assessment | UiPath RPA | REFramework | Orchestrator Queue**

## 1. Project Overview

The **IT Service Request Processing Automation** project is designed to automate the processing of employee IT service requests using UiPath.

The automation validates incoming IT requests against employee master data and role-access mapping information. Valid requests are added to an UiPath Orchestrator Queue and processed using the REFramework architecture.

The solution follows a **Dispatcher–Performer model** to provide modularity, reliability, exception handling, and transaction-level processing.

**Important:** This project runs in simulation mode. It does not create real employee accounts, grant actual access, purchase licenses, or perform real IT provisioning.

## 2. Project Objectives

- Automate IT service request processing.
- Validate employee and request information.
- Identify invalid or incomplete requests.
- Use Orchestrator Queues for transaction management.
- Implement UiPath REFramework for reliable processing.
- Simulate device and software fulfilment based on employee roles.
- Prevent duplicate request processing.
- Implement business and system exception handling.
- Generate processing reports for tracking and auditing.
- Maintain source code using Git and GitHub.

## 3. Technology Stack

| Technology | Purpose |
|---|---|
| UiPath Studio | RPA workflow development |
| UiPath REFramework | Transaction processing |
| UiPath Orchestrator | Queue and process management |
| Excel | Input data and output reports |
| Git | Version control |
| GitHub | Source code repository |
| VB.NET | UiPath workflow expressions |

## 4. Solution Architecture

The project follows a Dispatcher–Performer architecture.

### Dispatcher

The Dispatcher is responsible for preparing and validating IT service requests.

Responsibilities:

1. Read configuration values from `Config.xlsx`.
2. Read incoming requests from `IT_Requests.xlsx`.
3. Read employee information from `Employee_Master.xlsx`.
4. Read role and access information from `Role_Access_Mapping.xlsx`.
5. Validate mandatory request fields.
6. Verify employee existence and employment status.
7. Validate department and role mapping.
8. Identify duplicate requests.
9. Add valid requests to the Orchestrator Queue.
10. Record invalid requests in `Dispatcher_Exceptions.xlsx`.

### Performer

The Performer processes requests from the Orchestrator Queue using REFramework.

Responsibilities:

1. Initialize configuration and required resources.
2. Retrieve transactions from the Orchestrator Queue.
3. Validate transaction information.
4. Identify the employee's device and software profile.
5. Simulate IT service fulfilment.
6. Update `IT_Service_Register.xlsx`.
7. Prevent duplicate fulfilment.
8. Handle business and system exceptions.
9. Generate processing reports.
10. Complete transaction processing.

## 5. Workflow Architecture

```text
                IT Service Requests
                         |
                         v
                    Dispatcher
                         |
             Read Config and Input Files
                         |
                         v
                Validate Request Data
                         |
               +---------+---------+
               |                   |
          Valid Request        Invalid Request
               |                   |
               v                   v
        Orchestrator Queue   Dispatcher Exceptions
               |
               v
          REFramework
               |
          Get Transaction
               |
          Process Request
               |
       Validate Employee & Role
               |
       Map Device and Software
               |
       Simulate Fulfilment
               |
       Update IT Service Register
               |
       Generate Processing Summary
```

## 6. Input Files

The automation uses the following input files:

| File Name | Description |
|---|---|
| `IT_Requests.xlsx` | Contains incoming IT service requests |
| `Employee_Master.xlsx` | Contains employee information and status |
| `Role_Access_Mapping.xlsx` | Contains department, role, device, and software mappings |
| `Config.xlsx` | Contains configurable automation settings |

## 7. Configuration

The automation uses `Config.xlsx` to manage workflow settings without hardcoding values.

| Config Key | Description |
|---|---|
| `InputFile` | IT request input file |
| `EmployeeMasterFile` | Employee master file |
| `RoleMappingFile` | Role mapping file |
| `OutputFolder` | Output report location |
| `LogFolder` | Log folder location |
| `QueueName` | Orchestrator queue name |
| `OrchestratorFolder` | Orchestrator folder name |
| `ProcessName` | Performer process name |
| `MaxRetryCount` | Maximum retry count |
| `SimulationMode` | Enables simulation mode |
| `DuplicateCheck` | Enables duplicate request validation |
| `AllowedRequestTypes` | Allowed request types |

**Safety:** `SimulationMode` must remain `True` throughout the assessment.

## 8. Orchestrator Configuration

| Component | Name |
|---|---|
| Orchestrator Folder | `F_Assessment_Week3` |
| Queue Name | `Q_ITServiceRequests` |
| Process Name | `P_ITServiceRequestProcessor` |

The Dispatcher adds eligible requests to the queue, and the Performer retrieves and processes them.

Each request uses its `RequestID` as the unique queue reference to support duplicate prevention.

## 9. REFramework Implementation

The Performer follows the standard REFramework states.

### Init

- Read configuration.
- Initialize required variables.
- Validate configuration
