### 📘 **Intro to Airflow**

| **Main Topic**    | **Subtopics**                       | **Key Concepts**                                                                      |
| ----------------- | ----------------------------------- | ------------------------------------------------------------------------------------- |
| What is Airflow?  | Workflow orchestration, DAGs, Tasks | Directed Acyclic Graph (DAG), Tasks, Dependencies                                     |
| DAG Basics        | DAG definition, arguments           | `dag_id`, `start_date`, `schedule_interval`, `default_args`, `retries`, `retry_delay` |
| Operators         | PythonOperator, BashOperator        | Used to define task behavior via `python_callable` or `bash_command`                  |
| Task Dependencies | Setting task order                  | Using bitshift operators (`>>`, `<<`) to link tasks                                   |
| DAG Execution     | Manual testing                      | `airflow tasks test <dag_id> <task_id> <execution_date>`                              |

---

### 🛠️ **Implementing Airflow DAGs**

| **Main Topic**     | **Subtopics**                                          | **Key Concepts**                                                                                           |
| ------------------ | ------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------- |
| Sensors            | FileSensor, ExternalTaskSensor                         | Used to wait for conditions, such as file presence (`poke_interval`, `timeout`, `mode`)                    |
| Template Rendering | Jinja2 templates in fields                             | `{{ ds }}`, `{{ ds_nodash }}`, `{{ params.filename }}`, loops in templates, `macros`, `conf`, `dag` access |
| Branching          | BranchPythonOperator                                   | Conditional logic based on runtime variables (e.g., even/odd day branching), `provide_context=True`        |
| Task Context       | kwargs dictionary, runtime access                      | Dynamic access to task instance context and Airflow variables                                              |
| Macros & Variables | `macros.datetime`, `macros.timedelta`, `macros.ds_add` | Useful for template logic and scheduling                                                                   |
| DAG Views          | Graph view, Tree view                                  | Visual understanding of task flows, branching paths, and skipped tasks                                     |

---

### 🧩 **Maintaining and Monitoring Workflows**

| **Main Topic**   | **Subtopics**                           | **Key Concepts**                                                                 |
| ---------------- | --------------------------------------- | -------------------------------------------------------------------------------- |
| Airflow CLI      | Triggering DAGs and tasks from terminal | `airflow dags trigger`, `airflow tasks test`, log outputs                        |
| Troubleshooting  | Detecting issues in tasks               | Using logs, error highlights in red, checking for missing dependencies           |
| Branching Issues | Task runs when it should be skipped     | Fix by setting proper dependencies from branch task to conditional tasks         |
| Template Fields  | Which fields support templates          | Use `help(BashOperator)` to check `template_fields`, e.g., `bash_command`, `env` |
| Task Skipping    | Behavior of branching                   | Only the returned task(s) run, others are marked as skipped                      |

---

### 🚀 **Building Production Pipelines** (Practical Focus)

| **Main Topic**          | **Subtopics**                                     | **Key Concepts**                                                                           |
| ----------------------- | ------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| Running DAGs & Tasks    | Test task and DAG executions                      | `airflow tasks test`, `airflow dags trigger` with date                                     |
| SLA Reporting           | Importance in production pipelines                | Used to ensure timely task completion                                                      |
| File Detection Pipeline | Using FileSensor to trigger further tasks         | `filepath`, `poke_interval`, troubleshooting missing imports                               |
| Production DAG Workflow | Full pipeline with sensors, tasks, SLA, templates | Real-world DAG with conditional logic, runtime macros, file monitoring, template rendering |

---

Let me know if you'd like this exported as a PDF or Notion page for reference or sharing!

---

### ✅ **Airflow Topics Summary Table**

| **Main Topic**                      | **Subtopics**                                                                      | **Key Points & Examples**                                                                                                                                                                                                   |
| ----------------------------------- | ---------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Templates in Airflow**            | - Basic templating <br> - Advanced Jinja templating                                | - Use `params.filename` to dynamically insert filenames <br> - Jinja for loops like `{% for filename in params.filenames %}` to iterate over lists <br> - Use `{{ filename }}` inside loops                                 |
| **Airflow Variables**               | - Runtime variables <br> - Common built-in variables                               | - `{{ ds }}`: execution date (YYYY-MM-DD) <br> - `{{ ds_nodash }}`, `{{ prev_ds }}`: date w/o dashes or previous run <br> - `{{ dag }}`, `{{ conf }}` give access to DAG and config objects                                 |
| **Airflow Macros**                  | - Built-in macro functions                                                         | - `macros.datetime`, `macros.timedelta`, `macros.uuid` <br> - `macros.ds_add('20250410', 10)` → adds 10 days <br> - Helpful for dynamic date manipulation                                                                   |
| **Branching in DAGs**               | - BranchPythonOperator <br> - Conditional task execution                           | - Returns `task_id` to decide which branch runs <br> - Uses `kwargs['ds_nodash']` to determine even/odd day <br> - Tasks not selected are marked **skipped** <br> - Use `provide_context=True` for runtime variables access |
| **Branching Troubleshooting**       | - Task dependencies <br> - Common issues                                           | - If tasks run regardless of branch decision, dependencies between branch task and downstream tasks are missing                                                                                                             |
| **Operators Recap**                 | - BashOperator <br> - PythonOperator <br> - BranchPythonOperator <br> - FileSensor | - `bash_command` for BashOperator <br> - `python_callable` for PythonOperator <br> - FileSensor needs `filepath`, `poke_interval`, `timeout`, and optionally `mode='poke'` or `'reschedule'`                                |
| **DAG & Task Execution**            | - Manual DAG/task run                                                              | - `airflow tasks test <dag_id> <task_id> -1` to test task without full DAG execution <br> - `airflow dags trigger -e <date> <dag_id>` to simulate full DAG run                                                              |
| **Template Usage & Fields**         | - How to find which fields are templatable                                         | - Use Python REPL with `help(BashOperator)` or any class <br> - Look for `template_fields` in the help output <br> - `bash_command` and `env` are templatable fields for BashOperator                                       |
| **Production Pipelines**            | - Combining sensors, branching, and operators <br> - SLA reporting                 | - Use FileSensor to detect uploaded file <br> - Trigger branching logic based on file/date <br> - Integrate with downstream tasks for ETL/data transformation workflows                                                     |
| **Troubleshooting Production DAGs** | - Errors in sensor <br> - Fixing import paths, parameters                          | - Import errors: check top of `pipeline.py` <br> - `File not found`: correct the `filepath` <br> - Use logs (`ERROR` in red) to diagnose                                                                                    |

---
