# Hospital Management Database Analysis (SQL)

A SQL Server project that analyses a hospital database made up of five related tables: patients, doctors, appointments, treatments and billing. I linked the tables with foreign keys, profiled each table, created new patient columns, and wrote queries to answer questions about patients, doctors, appointments and billing.

## Database

| Table | What it holds |
|---|---|
| patients | Patient details: name, gender, date of birth, insurance provider, registration date |
| doctors | Doctor details: name, specialization, years of experience, hospital branch |
| appointments | Each appointment, with its date, reason for visit and status |
| treatments | Treatments given at an appointment, with type, description and date |
| billing | Bills for treatments, with amount, bill date, payment method and payment status |

**Relationships**

| Table | Column | References |
|---|---|---|
| appointments | patient_id | patients |
| appointments | doctor_id | doctors |
| treatments | appointment_id | appointments |
| billing | patient_id | patients |
| billing | treatment_id | treatments |

A bill connects to a doctor and hospital branch through its treatment and appointment: **billing → treatments → appointments → doctors**.

## What I did

1. **Linked the tables.** I added five foreign key constraints so the tables are connected and the data stays consistent.
2. **Profiled each table.** I counted records by category to understand the data before analysing it: patients by insurance provider and gender, treatments by type and description, bills by payment method and payment status, appointments by reason for visit and status, and doctors by specialization and hospital branch.
3. **Created new columns on the patients table.**
   - `FullName`: first and last name joined together.
   - `Age`: calculated from date of birth.
   - `Age_Category`: Young Adult (21 to 35), Middle Age (36 to 55) and Senior Adult (56 and above).
4. **Answered business questions** with multi-table joins, grouping and aggregate functions (listed below).

## Questions answered

| Question | Tables used |
|---|---|
| How many patients does each insurance provider cover? | patients |
| How are patients spread across age categories, and what is the average age by gender? | patients |
| In which years did patients register? | patients |
| Which months have treatments and completed appointments? | treatments, appointments |
| What are the most common reasons for visits and appointment statuses? | appointments |
| Which payment methods and payment statuses are most common? | billing |
| How many doctors are there per specialization and per hospital branch? | doctors |
| Which doctors completed the most appointments? | doctors, appointments |
| How do doctors compare by experience, specialization and patient load? | doctors, appointments |
| What is the average bill for each treatment type? | billing, treatments |
| What is the total billed amount by month? | billing |
| What is the total billed amount by hospital branch? | billing, treatments, appointments, doctors |
| Which patients have pending or failed payments? | patients, billing |

## Example: billed amount by hospital branch

Bills are linked to branches through treatments and appointments, so each bill is counted once:

```sql
SELECT D.hospital_branch,
       ROUND(SUM(B.amount), 2) AS Total_Billed
FROM billing AS B
INNER JOIN Treatments AS T ON B.treatment_id = T.treatment_id
INNER JOIN appointments AS A ON T.appointment_id = A.appointment_id
INNER JOIN doctors AS D ON A.doctor_id = D.doctor_id
GROUP BY D.hospital_branch
ORDER BY Total_Billed DESC;
```

## SQL skills used

- `INNER JOIN` across two to four tables
- `GROUP BY` with `COUNT`, `AVG` and `SUM`
- `ALTER TABLE` to add foreign keys, computed columns and new columns
- `CASE` with `UPDATE` to build categories
- Date functions: `DATENAME`, `MONTH`, `YEAR`, `DATEDIFF`, `GETDATE`
- `CONCAT`, `ROUND`, `DISTINCT`, `WHERE` and `ORDER BY`

## Tools

Microsoft SQL Server (T-SQL)

## Author

Rofee'ah Tijani
GitHub: [Rofeeah-Tijani](https://github.com/Rofeeah-Tijani)
