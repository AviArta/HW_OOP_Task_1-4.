# HW OOP

```
students_list = []
lecturer_list = []

class Student:

    def __init__(self, name, surname, gender):
        self.name = name
        self.surname = surname
        self.gender = gender
        self.finished_courses = []
        self.courses_in_progress = []
        self.grades = {}
        students_list.append(self)

    def add_courses(self, course_name):
        self.finished_courses.append(course_name)

# метод, который проверяет, что оценка выставляется именно экземпляру класса Lecturer, при этом
# лектор д.б. прикреплён к этому курсу, а студент его проходить
    def rate_lection(self, lecturer, course, grade):
        if isinstance(lecturer, Lecturer) and course in self.courses_in_progress and course in lecturer.courses_attached:
            if course in lecturer.grade_lection:
                lecturer.grade_lection[course] += [grade]
            else:
                lecturer.grade_lection[course] = [grade]
        else:
            return 'Ошибка'

    def midle_grade_hw(self):
        total = 0
        grade_list = []
        for key, gra in self.grades.items():
            for g in gra:
                grade_list.append(g)
                total += g
        midle_grade_h_w = round(total / len(grade_list), 2)
        return midle_grade_h_w

    def __str__(self):
        res = f'''Имя: {self.name} \nФамилия: {self.surname} \nСредняя оценка за домашние задания: {self.midle_grade_hw()} \nКурсы в процессе изучения: {', '.join([i for i in self.courses_in_progress])} \nЗавершённые курсы: {', '.join([i for i in self.finished_courses])}'''
        return res

    def __lt__(self, other):
        if not isinstance(other, Student):
            print('Такого студента нет.')
            return
        return self.midle_grade_hw() < other.midle_grade_hw()

class Mentor:
    def __init__(self, name, surname):
        self.name = name
        self.surname = surname
        self.courses_attached = []

class Lecturer(Mentor):
    def __init__(self, name, surname):
        super().__init__(name, surname)
        self.grade_lection = {}
        lecturer_list.append(self)

    def midl_grade_lection(self):
        total = 0
        grade_list = []
        for key, gra in self.grade_lection.items():
            for g in gra:
                grade_list.append(g)
                total += g
        midl_grade_lect = round(total / len(grade_list), 2)
        return midl_grade_lect

    def __str__(self):
        res = f'Имя: {self.name} \nФамилия: {self.surname} \nСредняя оценка за лекцию: {self.midl_grade_lection()}'
        return res

    def __lt__(self, other):
        if not isinstance(other, Lecturer):
            print('Такого лектора нет.')
            return
        return self.midl_grade_lection() < other.midl_grade_lection()

class Reviewer(Mentor):
    def __str__(self):
        res = f'Имя: {self.name} \nФамилия: {self.surname}'
        return res

# метод, который проверяет, что оценка выставляется именно экземпляру класса Student, при этом
# проверяющий д.б. прикреплён к этому курсу, а студент его проходить
    def rate_hw(self, student, course, grade):
        if isinstance(student, Student) and course in self.courses_attached and course in student.courses_in_progress:
            if course in student.grades:
                student.grades[course] += [grade]
            else:
                student.grades[course] = [grade]
        else:
            return 'Ошибка'

student_1 = Student('Dima', 'Dmitriev', 'male')
student_1.finished_courses += ['Git']
student_1.courses_in_progress += ['Python']
student_1.grades['Git'] = [10, 10, 8, 8, 10]
student_1.grades['Python'] = [10, 10]
student_2 = Student('Lisa', 'Buzlova', 'female')
student_2.finished_courses += ['Python']
student_2.courses_in_progress += ['Git']
student_2.courses_in_progress += ['Java']
student_2.grades['Python'] = [6, 6, 8, 8, 10]
student_2.grades['Git'] = [8, 8]
student_2.grades['Java'] = [9, 10, 7]

lecturer_1 = Lecturer('Ivan', 'Ivanov')
lecturer_1.courses_attached += ['Git']
lecturer_1.courses_attached += ['Python']
lecturer_2 = Lecturer('Victor', 'Victorov')
lecturer_2.courses_attached += ['Python']
lecturer_2.courses_attached += ['Java']

# task_2
# print(lecturer_1.courses_attached)
student_1.rate_lection(lecturer_1, 'Python', 8)
student_1.rate_lection(lecturer_1, 'Python', 6)
student_1.rate_lection(lecturer_2, 'Python', 8)
student_2.rate_lection(lecturer_1, 'Python', 10)
student_2.rate_lection(lecturer_2, 'Python', 10)
student_2.rate_lection(lecturer_1, 'Git', 7)
student_2.rate_lection(lecturer_1, 'Git', 10)
student_2.rate_lection(lecturer_2, 'Java', 4)
student_2.rate_lection(lecturer_2, 'Java', 8)

print(f'Оценки лектора: {lecturer_1.grade_lection}.')
# print(f'Оценки лектора: {lecturer_2.grade_lection}.')

# task_3
reviewer_1 = Reviewer('Vladimir', 'Vladimirov')
reviewer_2 = Reviewer('Mila', 'Milova')
print(reviewer_1)
# print(reviewer_2)
# print(lecturer_1.grade_lection)
print(lecturer_1)
# print(lecturer_2.grade_lection)
# print(lecturer_2)
print(student_1)
# print(student_2)

# Реализация сравнения:
# print(lecturer_1.midl_grade_lection())
# print(lecturer_2.midl_grade_lection())
print(lecturer_1 < lecturer_2)
# print(student_1.midle_grade_hw())
# print(student_2.midle_grade_hw())
print(student_1 < student_2)

# task_4
def midl_grade_students(students_list, course):
    total = 0
    count = 0
    for student in students_list:
        if course in student.courses_in_progress or course in student.finished_courses:
            for grade in student.grades[course]:
                total += grade
                count += 1
            return f'Ср. балл студентов по курсу {course}: {round(total / count, 2)}.'
    return 'Такого курса нет.'

print(midl_grade_students(students_list, 'Python'))
# print(midl_grade_students(students_list, 'Git'))
# print(midl_grade_students(students_list, 'Java'))
print(midl_grade_students(students_list, 'C++'))

def midl_grade_lecturer(lecturer_list, course):
    total = 0
    count = 0
    for lecturer in lecturer_list:
        if course in lecturer.courses_attached:
            for grade in lecturer.grade_lection[course]:
                total += grade
                count += 1
            return f'Ср. балл лекторов по курсу {course}: {round(total / count, 2)}.'
    return 'Такого курса нет.'

print(midl_grade_lecturer(lecturer_list, 'Python'))
# print(midl_grade_lecturer(lecturer_list, 'Git'))
# print(midl_grade_lecturer(lecturer_list, 'Java'))
print(midl_grade_lecturer(lecturer_list, 'C++'))
```