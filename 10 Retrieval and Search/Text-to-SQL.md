# Text-to-SQL

Text-to-SQL is the task of converting natural language questions into SQL queries using [[Large Language Models]]. It is one of the most practical applications of AI — enabling non-technical users to query databases and demonstrating how LLMs bridge human intent and structured data.

## How It Works

```
User: "What were our top 5 products by revenue last quarter?"
          ↓ LLM
SQL:  SELECT product_name, SUM(revenue) as total_revenue
      FROM sales
      WHERE sale_date >= '2025-10-01' AND sale_date < '2026-01-01'
      GROUP BY product_name
      ORDER BY total_revenue DESC
      LIMIT 5;
          ↓ execute
Result: [table of products and revenue]
```

## The Challenge

Text-to-SQL is harder than it appears:

| Challenge | Example |
|---|---|
| **Schema understanding** | Which table has the data? What are the column names? |
| **Ambiguity** | "last quarter" — which quarter? Fiscal or calendar? |
| **Joins** | "Show me customer orders" — requires joining customers and orders tables |
| **Aggregation** | "average", "total", "top N" require GROUP BY, SUM, AVG |
| **Filters** | "active customers" — what defines "active"? |
| **SQL dialect** | PostgreSQL vs MySQL vs BigQuery syntax differences |

## Architecture

### Basic Approach
```
Schema description + User question → LLM → SQL query → Execute → Results
```

### Production Approach
```
User question
    ↓
Schema retrieval (find relevant tables/columns)
    ↓
Few-shot examples (similar past queries)
    ↓
LLM generates SQL
    ↓
SQL validation (parse, check syntax)
    ↓
Safety check (no DROP, DELETE, etc.)
    ↓
Execute on read-only replica
    ↓
Format results for user
```

## Schema Representation

How you present the database schema to the LLM matters enormously:

### CREATE TABLE Statements
```sql
CREATE TABLE customers (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    email TEXT,
    created_at TIMESTAMP
);

CREATE TABLE orders (
    id INTEGER PRIMARY KEY,
    customer_id INTEGER REFERENCES customers(id),
    total DECIMAL(10,2),
    status TEXT CHECK (status IN ('pending', 'shipped', 'delivered')),
    created_at TIMESTAMP
);
```

Most effective — LLMs understand SQL DDL well.

### Column Descriptions
Add natural language descriptions for ambiguous columns:
```
-- customers.status: 'active' (logged in within 90 days), 'inactive', 'churned'
-- orders.total: in USD, includes tax, excludes shipping
```

### Sample Rows
Include a few example rows to show data format:
```
-- Example: customers
-- | id | name        | email           | created_at |
-- | 1  | Alice Smith | alice@mail.com  | 2024-01-15 |
```

## Text-to-SQL via [[Model Context Protocol|MCP]]

[[AI Coding Harnesses]] connect to databases through MCP servers:

```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": { "DATABASE_URL": "postgresql://..." }
    }
  }
}
```

The agent can then query the database as part of its [[Agentic Coding Paradigm|agent loop]]:
```
User: "Find all customers who haven't ordered in 6 months"
Agent: *reads schema via MCP* → *generates SQL* → *executes* → *returns results*
```

## Safety

Text-to-SQL with LLMs has serious safety implications:

| Risk | Mitigation |
|---|---|
| **SQL injection via prompt** | Parameterized queries, input sanitization |
| **Destructive queries** | Read-only database connection, block DDL/DML |
| **Data exfiltration** | Row limits, column-level access control |
| **Performance** | Query timeout, EXPLAIN analysis, complexity limits |
| **[[Prompt Injection]]** | User input could inject malicious SQL instructions |

**Best practice:** Always execute against a **read-only replica** with row limits and query timeouts.

## Benchmarks

| Benchmark | What It Tests | Top Performance |
|---|---|---|
| **Spider** | Complex cross-database SQL | 85%+ execution accuracy |
| **BIRD** | Real-world databases with dirty data | 70%+ |
| **WikiSQL** | Simple single-table queries | 90%+ (largely solved) |

## Beyond Simple Queries

### Multi-Turn SQL
```
User: "Show me total sales by region"
Agent: *generates and runs query*
User: "Now break that down by month"
Agent: *modifies previous query to add month grouping*
User: "Only for the EMEA region"
Agent: *adds WHERE clause*
```

The agent maintains conversation context and iteratively refines the query.

### Query Explanation
```
User: "Explain this query: SELECT ... FROM ... WHERE ..."
Agent: "This query finds all customers who placed more than 3 orders
        in the last month by joining the customers and orders tables..."
```

### Query Optimization
```
User: "This query is slow"
Agent: *runs EXPLAIN ANALYZE* → "The bottleneck is a sequential scan on
        the orders table. Adding an index on (customer_id, created_at)
        would improve performance."
```

## Related

- [[Large Language Models]]
- [[Model Context Protocol]]
- [[Tool Use in AI]]
- [[Structured Output]]
- [[Prompt Injection]]
- [[AI Coding Harnesses]]
