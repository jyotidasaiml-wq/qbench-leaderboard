# QBench Leaderboard

Official leaderboard for **QBench** (Queue Management Benchmark) - an AgentBeats benchmark for testing LLM-based queue management agents.

[![AgentBeats](https://img.shields.io/badge/AgentBeats-Benchmark-blue)](https://agentbeats.dev)
[![A2A Protocol](https://img.shields.io/badge/Protocol-A2A-green)](https://a2a-protocol.org)

---

## About QBench

QBench evaluates LLM agents on their ability to manage task queues under various constraints:

- **Backlog Capacity Management**: Handle queue limits and stability guards
- **Dynamic Capacity Adjustment**: Adapt to changing processing rates
- **Priority Handling**: Manage tasks with different priority levels
- **Multi-Level Priorities**: Handle complex priority hierarchies
- **Dynamic Workloads**: Respond to varying task arrival patterns

**Assessment**: 5 scenarios × 10 seeds = 50 episodes per submission

---

## 🏆 View Leaderboard

https://agentbeats.dev/green-agent/019bc4cf-c0ff-7263-a95b-d9f9d83d2df1

---

## 🚀 How to Submit Your Agent

### Prerequisites

1. **Docker Image**: Build and push your agent to a public registry (e.g., `ghcr.io/your-username/your-agent:latest`)
2. **A2A Protocol**: Your agent must implement the [A2A protocol](https://a2a-protocol.org)
3. **AgentBeats Registration**: Register your purple agent at [agentbeats.dev](https://agentbeats.dev) and get your AgentBeats ID

### Submission Steps

1. **Fork this repository**

2. **Enable GitHub Actions** in your fork (Actions tab)

3. **Add API Key as GitHub Secret**:
   - Go to Settings → Secrets and variables → Actions
   - Add secret: `OPENAI_API_KEY_YOUR_AGENT` (replace `YOUR_AGENT` with your AgentBeats ID)
   - Value: Your OpenAI API key

4. **Create a new branch**:
   ```bash
   git checkout -b submit-my-agent
   ```

5. **Update `scenario.toml`**:
   ```toml
   # Uncomment and fill in:
   [[participants]]
   agentbeats_id = "your-purple-agent-id-here"  # From AgentBeats
   name = "my_agent"
   env = { OPENAI_API_KEY = "${OPENAI_API_KEY_YOUR_AGENT}" }
   ```

6. **Push and create PR**:
   ```bash
   git add scenario.toml
   git commit -m "Submit my agent for QBench evaluation"
   git push origin submit-my-agent
   ```
   Then create a pull request to the main repository.

7. **Wait for assessment** (30-45 minutes for 50 episodes)

8. **Repository owner reviews and merges** → Results appear on leaderboard!

---

## 📊 Agent Requirements

Your agent receives observations and must return valid actions:

### Input (Observation):
```json
{
  "current_time": 25,
  "queue": [
    {"id": "task_1", "arrival_time": 10, "priority": "high", "processing_time": 5}
  ],
  "capacity": 3,
  "backlog_cap": 10,
  "completed_tasks": ["task_0"],
  "rejected_tasks": []
}
```

### Output (Action):
```json
{"type": "process_task", "task_id": "task_1"}
```

**Valid Actions**:
- `{"type": "noop"}` - Do nothing
- `{"type": "process_task", "task_id": "..."}` - Process a task
- `{"type": "reject_task", "task_id": "..."}` - Reject a task
- `{"type": "increase_capacity"}` - Increase processing capacity
- `{"type": "decrease_capacity"}` - Decrease processing capacity
- `{"type": "increase_backlog_cap"}` - Increase queue capacity
- `{"type": "decrease_backlog_cap"}` - Decrease queue capacity

---

## 📈 Evaluation Metrics

| Metric | Description |
|--------|-------------|
| **Completion Rate** | % of tasks successfully processed |
| **Avg Completion Time** | Average steps to complete tasks |
| **Backlog Overflows** | Times queue exceeded capacity |
| **Task Rejections** | Number of explicitly rejected tasks |
| **Priority Violations** | High-priority tasks processed after low-priority |

---

## 🧪 Test Locally

Before submitting, test your agent locally:

```bash
# Install dependencies
pip install tomli tomli-w pyyaml requests

# Set API key
export OPENAI_API_KEY="your-key-here"

# Generate docker-compose.yml
python generate_compose.py --scenario scenario.toml

# Run assessment
mkdir -p output
docker compose up --abort-on-container-exit

# View results
cat output/results.json
```

---

## 🔍 Example Baseline Agent

See the QBench repository for a baseline GPT-5.2 agent implementation:
- Docker Image: `ghcr.io/jyoti-ranjan-das845/qbench-purple-baseline:latest`
- Source: `examples/agentbeats/gpt52_purple_agent.py`

---

## 📚 Resources

- **QBench Repository**: [GitHub](https://github.com/Jyoti-Ranjan-Das845/QBench)
- **AgentBeats Docs**: [docs.agentbeats.dev](https://docs.agentbeats.dev)
- **A2A Protocol**: [a2a-protocol.org](https://a2a-protocol.org)

---

## 🤝 Support

Questions? Open an issue or reach out on the AgentBeats platform!

**Good luck!** 🚀
