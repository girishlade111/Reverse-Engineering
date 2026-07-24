========================================================================
SUPPLEMENTARY PROMPT S11: SCHEDULED TASKS
========================================================================
Supplementary Analysis: Cron Jobs, Schedulers, and Timed Operations
Enterprise Reverse Engineering Prompt Framework

========================================================================
PURPOSE
========================================================================

This supplementary prompt analyzes scheduled tasks, cron jobs,
timers, and time-based operations in the system.

========================================================================
WHEN TO USE
========================================================================

Execute this prompt if:
- Cron job definitions exist
- Scheduled task configurations are present
- Timer/interval-based operations exist
- Batch processing schedules are defined
- Time-based triggers are used

Execute after Phase 4.

========================================================================
ACTIVITIES
========================================================================

S11.1. SCHEDULED TASK INVENTORY
- Identify all scheduled/recurring tasks
- Document schedule expressions (cron syntax)
- Document task trigger mechanisms
- Document task ordering dependencies
- Document missed/misfired task handling

S11.2. TASK IMPLEMENTATION ANALYSIS
- For each scheduled task, document:
  - Task purpose and business logic
  - Execution interval and timing
  - Input data sources
  - Output data destinations
  - Error handling and retry strategy
  - Timeout configuration
  - Concurrency/prevention of overlap

S11.3. SCHEDULER INFRASTRUCTURE
- Document scheduling library/system
- Document scheduler configuration
- Document distributed scheduling (if applicable)
- Document scheduler monitoring
- Document scheduler failover

========================================================================
REQUIRED ARTIFACTS
========================================================================

ARTIFACT S11.1: SCHEDULED_TASK_INVENTORY.md
ARTIFACT S11.2: TASK_IMPLEMENTATIONS.md
ARTIFACT S11.3: SCHEDULER_INFRASTRUCTURE.md

========================================================================
END OF PROMPT S11
========================================================================
