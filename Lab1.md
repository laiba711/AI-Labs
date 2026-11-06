# Exercise 1
```python
{
    "id": "RULE_07",
    "conditions": {"cgpa_min": 3.70, "disciplinary_warnings": 0},
    "condition_type": "honors_rule",
    "grade": "A+",
    "status": "Graduated with Magna Cum Laude Honors",
    "action": "Awarded degree with highest distinction."
}
```
# Exercise 2
```python
def evaluate_student(student_record, kb):
    print(f"\n--- EVALUATING STUDENT: {student_record['student_name']} (ID: {student_record['student_id']}) ---")
    
    print("[TRACE] Checking attendance constraint (< 75%)...")
    if student_record["attendance"] < 75:
        attendance_rule = next(r for r in kb["rules"] if r["condition_type"] == "attendance_below")
        return {
            "rule_id": attendance_rule["id"],
            "grade": attendance_rule["grade"],
            "status": attendance_rule["status"],
            "action": attendance_rule["action"]
        }
    
    print("[TRACE] Evaluating marks range rules...")
    marks = student_record["total_marks"]
    for rule in kb["rules"]:
        if rule["condition_type"] == "marks_range":
            print(f"[TRACE] Testing rule {rule['id']} for range {rule['conditions']['marks_min']} - {rule['conditions']['marks_max']}")
            if rule["conditions"]["marks_min"] <= marks <= rule["conditions"]["marks_max"]:
                print(f"[TRACE] Match found! Rule {rule['id']} triggered.")
                return {
                    "rule_id": rule["id"],
                    "grade": rule["grade"],
                    "status": rule["status"],
                    "action": rule["action"]
                }
                
    return {
        "rule_id": "UNKNOWN",
        "grade": "N/A",
        "status": "Invalid Score Entry",
        "action": "Verify submitted marks with the exam department."
    }
