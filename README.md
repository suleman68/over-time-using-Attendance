class Student:
    def _init_(self, name):
        self.name = name
        self.attendance = {}

    def mark_attendance(self, date, status):
        self.attendance[date] = status

    def get_attendance(self):
        return self.attendance


class AttendanceTracker:
    def _init_(self, students):
        self.students = {}
        for student in students:
            self.students[student] = Student(student)

    def mark_attendance(self, student, date, status):
        if student in self.students:
            self.students[student].mark_attendance(date, status)
        else:
            print(f"Student '{student}' not found.")

    def get_attendance(self, student):
        if student in self.students:
            return self.students[student].get_attendance()
        else:
            print(f"Student '{student}' not found.")
            return {}

    def get_total_attendance(self, date):
        total_present = 0
        total_absent = 0
        for student in self.students.values():
            if date in student.attendance:
                if student.attendance[date] == "present":
                    total_present += 1
                elif student.attendance[date] == "absent":
                    total_absent += 1
        return total_present, total_absent

    def get_cumulative_attendance(self):
        cumulative_attendance = {}
        for student in self.students.values():
            for date, status in student.attendance.items():
                if date not in cumulative_attendance:
                    cumulative_attendance[date] = {"present": 0, "absent": 0}
                cumulative_attendance[date][status] += 1
        return cumulative_attendance


# Example usage
students = ["Alice", "Bob", "Charlie"]
tracker = AttendanceTracker(students)

# Mark attendance for Alice and Bob on different dates
tracker.mark_attendance("Alice", "2023-03-27", "present")
tracker.mark_attendance("Bob", "2023-03-27", "absent")
tracker.mark_attendance("Alice", "2023-03-28", "absent")
tracker.mark_attendance("Bob", "2023-03-28", "present")
tracker.mark_attendance("Charlie", "2023-03-27", "present")
tracker.mark_attendance("Charlie", "2023-03-28", "absent")

# Get Alice's attendance for all dates
print(tracker.get_attendance("Alice"))

# Get the total attendance for all students on March 27
total_present, total_absent = tracker.get_total_attendance("2023-03-27")
print(f"Present: {total_present}, Absent: {total_absent}")

# Get cumulative attendance for all students
print("Cumulative Attendance:")
cumulative_attendance = tracker.get_cumulative_attendance()
for date, counts in cumulative_attendance.items():
    print(f"Date: {date}, Present: {counts['present']}, Absent: {counts['absent']}")
