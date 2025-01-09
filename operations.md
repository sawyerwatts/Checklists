# Operations

- What are the governance policies, if any?
- Is FinOps desired and applied?
- Are there any reports that we'd want written?
- Are there backups? 3-2-1?
- What is the disaster recovery plan?

## Recurring Tasks

- What is the cadence to perform dependency wellness checks?
- What is the cadence for updating packages?
- What is the cadence for updating the language itself?
- What is the cadence for updating the DB, OS, and other third-party services?
    - What is the patching policy?
- What is the cadence for performing a full restore in a fresh environment?
- What is the cadence for reviewing the documentation?
- Periodically review substantial decisions and preconditions

## Monitoring/Alering

- How many/rate failures/errors is too many over what period?
- How many/rate successful requests/outputs/sideEffects?
- Is there monitoring of the various scalability and SLA requirements?
- VM/Container
    - Disk space
    - CPU usage
    - RAM usage
- Cloud services: cost usage
- Are you tracking and dashboarding/alerting for response times, run times, etc?

