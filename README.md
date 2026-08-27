# SQL Attendance Analytics: 5-Week Practice Track

This repository is a hands-on SQL practice course built around a small student attendance data model. It is designed to take you from basic `SELECT` statements to multi-table analysis, conditional aggregation, data-quality checks, rates, and reusable analytical queries.

The goal is not to copy the answer notebooks. The goal is to write, run, inspect, explain, and improve your own queries until the patterns become natural.

## Course Commitment

Complete **at least one worksheet every week**. Choose the pace that fits your schedule:

- **Daily practice:** complete one question per day. Each worksheet has six questions, so you can finish it in six focused sessions.
- **Weekly intensive:** complete all six questions in one day, then review your mistakes during the rest of the week.

Both plans are valid. The non-negotiable requirement is one completed worksheet per week. A worksheet is complete when you have attempted every question, checked your result against the answer notebook where available, and written down what you learned from any errors.

## Repository Layout

```text
data/
	dim_batch.csv
	dim_class.csv
	dim_date.csv
	dim_student.csv
	fact_attendance.csv

worksheets/
	week1.ipynb
	week2.ipynb
	week3.ipynb
	week4.ipynb
	week5.ipynb

solutions/
	week1_answers.ipynb
	week2_answers.ipynb
	week3_answers.ipynb
	week4_answers.ipynb

solution/
	(empty legacy directory; use solutions/ instead)
```

### Where to work

- Start with the matching notebook in [worksheets](worksheets/).
- Use the notebooks in [solutions](solutions/) only after attempting the work. Solutions are available for Weeks 1-4.
- Week 5 is a capstone worksheet. Its SQL cells are intentionally blank so you can build the queries independently.
- The CSV files in [data](data/) are the source tables. Do not modify them.

## Five-Week Roadmap

| Week | Main focus | What you practice |
| --- | --- | --- |
| 1 | SQL foundations with attendance data | `SELECT`, aliases, `LEFT JOIN`, null-safe labels, `GROUP BY`, counts, conditional aggregation, `HAVING`, and `ORDER BY` |
| 2 | Grouped analysis and data quality | Grouping by location and topic, status summaries, attendance rates, missing profile data, repeated records, and zero-activity students |
| 3 | Filtering and business scorecards | `LIKE`, status filters, date-range filters, monthly grouping, conditional counts, class coverage, averages, and profile gaps |
| 4 | Complete reporting and quality checks | Detailed five-table joins, classes with no attendance, class-level rates, absence risk, student issue rates, and batch-key mismatches |
| 5 | Independent capstone and review | Student performance, ranking, class analysis, high-risk students, thresholds, and a batch-level multi-table summary |

The difficulty rises gradually. Weeks 1 and 2 break every question into guided subparts such as selecting columns, adding one join at a time, grouping, aggregating, filtering, and sorting. Weeks 3 and 4 expect you to combine those patterns with less scaffolding. Week 5 asks you to produce the full solutions yourself.

## Data Model

The model contains dimensions that describe entities and a fact table that records attendance:

| Table | Role | Important columns |
| --- | --- | --- |
| `dim_student` | Student profile | `student_key`, `student_id`, `student_name`, `city`, `phone_no` |
| `dim_batch` | Batch information | `batch_key`, `batch_id`, `batch_name`, `start_date`, `end_date` |
| `dim_class` | Class schedule and topic | `class_key`, `class_id`, `batch_id`, `class_date`, `class_day`, `topic`, `status` |
| `dim_date` | Calendar attributes | `date_key`, `date`, `year`, `month`, `month_name`, `day_name` |
| `fact_attendance` | Attendance events | `attendance_id`, `student_key`, `class_key`, `batch_key`, `date_key`, `attendance_status`, `attendance_count` |

### Relationships

```text
dim_student.student_key  -> fact_attendance.student_key
dim_class.class_key      -> fact_attendance.class_key
dim_batch.batch_key      -> fact_attendance.batch_key
dim_date.date_key        -> fact_attendance.date_key
dim_class.batch_id       -> dim_batch.batch_id
```

Use the surrogate `*_key` columns when joining the fact table. The `dim_class.batch_id` relationship is deliberately included in later exercises for comparing a class's expected batch with the batch recorded in attendance.

## Recommended Study Routine

For each question:

1. Read the question and list the output columns before writing SQL.
2. Identify the table that owns each column.
3. Choose the driving table and decide whether each join should be `JOIN` or `LEFT JOIN`.
4. Write the smallest query that returns the base columns.
5. Add joins one at a time and check that the row count still makes sense.
6. Add null handling with functions such as `COALESCE` or `NULLIF`.
7. Add grouping and one aggregate at a time.
8. Add `WHERE` or `HAVING` filters only after checking the unfiltered result.
9. Add the requested ordering and explain the result in your own words.
10. Compare with the answer notebook, then revise your query rather than simply replacing it.

For Weeks 1 and 2, follow the numbered subparts in the markdown immediately before each answer cell. Treat each subpart as a checkpoint. You can write each intermediate query in a new cell or temporarily edit your own working copy.

## SQL Patterns to Master

By the end of five weeks, you should be able to explain and use:

- Selecting only the columns needed for a question.
- Short, readable table aliases such as `s`, `f`, `c`, `b`, and `d`.
- The difference between an inner `JOIN` and a `LEFT JOIN`.
- Why filtering a right-hand table in `WHERE` can undo the purpose of a `LEFT JOIN`.
- `COUNT(*)`, `COUNT(column)`, and `COUNT(DISTINCT column)`.
- `CASE` expressions for conditional counts and rates.
- `COALESCE` for readable fallback labels.
- `NULLIF` for blank values and division-by-zero protection.
- The difference between `WHERE` and `HAVING`.
- Grouping by every selected non-aggregated expression.
- Ordering by a calculated result.
- Using date dimensions and date ranges correctly.
- Checking for duplicate or mismatched relationships before trusting an aggregate.

## Working With Results

Do not judge a query only by whether it runs. For every result, ask:

- Does the number of rows match the requested grain, such as one row per student, class, or batch?
- Are students or classes with no attendance preserved when the question asks for them?
- Are null and blank values represented deliberately?
- Do the status counts add up to the total where they should?
- Are rates calculated from the correct denominator?
- Could a join have duplicated rows and inflated an aggregate?

When a result looks suspicious, temporarily remove `GROUP BY`, inspect the joined rows, and verify the join keys. This habit is as important as memorizing SQL syntax.

## Weekly Completion Checklist

At the end of each week, confirm that you have:

- Completed all six questions in that week's worksheet.
- Run or otherwise checked every query in your SQL environment.
- Compared your work with the available answer notebook.
- Recorded at least one mistake, correction, or new insight.
- Rewritten one query from memory without looking at the answer.

After Week 5, choose two earlier questions and solve them again without opening the solutions. If you can explain the table grain, join path, aggregation, filter, and sort for both queries, you have built a strong foundation for more advanced SQL work.

## Getting Started

1. Open [worksheets/week1.ipynb](worksheets/week1.ipynb).
2. Read the table relationships and Question 1 subparts.
3. Connect the notebook to your SQL environment or use the CSV files to create equivalent local tables.
4. Complete Question 1 before moving to the next question.
5. Continue with one question per day or complete all six questions in one weekly session.

Keep the source data unchanged, use clear aliases, and prefer a query you can explain over a query you can merely execute.