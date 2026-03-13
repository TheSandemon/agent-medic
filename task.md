# Task Assignment: Continuous Health Monitoring

## From: Studio Manager
## To: agent_medic
## Date: 2026-03-13

### OBJECTIVE
Run continuous health monitoring on all agents in the studio with a 60-second pulse cycle.

### PULSE CYCLE (60 seconds)
Execute this loop continuously:

1. **Write Heartbeat**
   - Write your status to `shared_workspace/heartbeats/agent_medic.json`
   - Include: timestamp, status (alive/degraded/error), current_task, error_count

2. **Health Check All Agents**
   - Read `studio_roster.json` to get current agent list
   - For each agent in roster:
     - Read their heartbeat file from `shared_workspace/heartbeats/<agent_name>.json`
     - Check for staleness (if heartbeat older than 2 minutes = unresponsive)
     - Check for error escalation (error_count increasing)
     - Check for status degradation

3. **Alert Generation**
   - If any agent is unresponsive, write alert to `shared_workspace/alerts/`
   - Alert format: JSON with subject_agent, issue, evidence, recommendation

4. **Inspection Log**
   - Log all inspection results to `agent_medic/inspection_log.md`
   - Include timestamp, all agent states, any issues found

5. **Manager Report**
   - Write summary to `shared_workspace/outbox/medic_report.json`
   - Include: total agents checked, any alerts triggered, system health status

### PRIORITY ISSUES TO DETECT
- Agents with missing heartbeats (newly hired may have grace period)
- Agents with increasing error_count
- Agents with stale timestamps (>2 minutes old)
- Submodule sync issues
- Git repository problems

### ERROR HANDLING
- If you cannot read a file, log error and continue
- If you cannot write to shared_workspace/, log locally and flag on next successful write
- **STOP after 3 consecutive failures** - log [MEDIC BLOCKED] and await Manager

### COMPLETION CRITERIA (Continuous - Never "Done")
- [ ] Heartbeat written every 60 seconds
- [ ] All roster agents checked each cycle
- [ ] Alerts written for any issues detected
- [ ] Inspection log updated
- [ ] Manager report sent each cycle