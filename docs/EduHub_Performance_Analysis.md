# Performance Analysis – EduHub MongoDB Project

## 1. Objective
To evaluate the performance of CRUD and aggregation operations across all collections (`users`, `courses`, `lessons`, `assignments`, `enrollment`).

---

## 2. Metrics Evaluated
- **Data Insertion Time** (bulk vs individual)
- **Query Efficiency** (indexed vs non-indexed lookups)
- **Aggregation Latency** (lookup joins and group pipelines)
- **Disk Usage and Index Size**
- **Scalability Considerations**

---

## 3. Key Observations

| Operation | Average Time (ms) | Index Used | Notes |
|------------|-------------------|-------------|--------|
| Insert Many (Users) | 22 | N/A | Bulk insert improved throughput |
| Find Query (Email Lookup) | 5 | ✅ Email index | Lookup performance improved 80% after indexing |
| Aggregation (Join Users–Enrollments) | 47 | Partial | Dependent on document size and $lookup complexity |
| Update Many | 11 | N/A | Efficient batch updates |
| Delete Operations | 6 | N/A | Lightweight, minimal locking |

---

## 4. Optimization Techniques
- Created **indexes** on key fields (`email`, `course_id`, `due_date`, `student_id`) to reduce latency.
- Used **projection** to limit returned fields.
- Batched insertions and updates to minimize I/O overhead.
- Added `$project` and `$unwind` stages early in pipelines to reduce document load.

---

## 5. Bottlenecks Identified
- `$lookup` joins between large collections caused temporary memory spikes.
- Aggregations with `$unwind` and `$group` required additional optimization for large datasets.
- Some operations became slower when projecting deeply nested objects.

---

## 6. Recommendations
- Implement **pagination** for large query results.
- Monitor with MongoDB Atlas performance metrics or `mongostat`.
- Use **compound indexes** for multi-field lookups.
- Consider sharding or data partitioning for larger scale.

---

## 7. Conclusion
EduHub performed efficiently after proper indexing. Average query response time was **under 50ms** for most operations. Aggregation performance remained acceptable, showing MongoDB’s capability for mixed analytical and transactional workloads.
