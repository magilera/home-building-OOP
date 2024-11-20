class Mentor:
    def __init__(self, name, surname):
        self.name = name
        self.surname = surname
        self.courses_attached = []

class Reviewer(Mentor):
    def rate_hw(self, student, course, grade):
        if isinstance(student, Student) and course in self.courses_attached and course in student.courses_in_progress:
            if course in student.grades:
                student.grades[course] += [grade]
            else:
                student.grades[course] = [grade]
        else:
            return 'Ошибка'

    def __str__(self):
        return f'Имя: {self.name}\nФамилия: {self.surname}'

class Lecturer(Mentor):
    def __init__(self, name, surname):
        super().__init__(name, surname)
        self.grades = {}  # Словарь для хранения оценок по каждому курсу

    def add_grade(self, course, grade):
        """Метод добавления оценки для лектора."""
        if course in self.grades:
            self.grades[course] += [grade]
        else:
            self.grades[course] = [grade]

    def average_rating(self):
        """Подсчитывает среднюю оценку лектора по всем курсам."""
        total_sum = sum(sum(grade_list) for grade_list in self.grades.values())
        total_count = sum(len(grade_list) for grade_list in self.grades.values()) or 0
        return round(total_sum / total_count, 1) if total_count > 0 else 0

    def __str__(self):
        return f'Имя: {self.name}\nФамилия: {self.surname}\nСредняя оценка за лекции: {self.average_rating()}'

    def rate_lecture(self, lecturer, course, grade):
            """Выставляет оценку лектору за проведенную лекцию."""
            if isinstance(lecturer, Lecturer) and self.is_course_attached(course) and \
              course in lecturer.courses_attached:
                lecturer.add_grade(course, grade)
            else:
                print("Ошибка")

def average_lecture_grade_for_course(lecturers, course):
  grades = []
  for lecturer in lecturers:
    if course in lecturer.grades:
      grades.extend(lecturer.grades[course])
    return round(sum(grades) / len(grades), 1) if grades else 0

def compare_lecturers_by_average_rating(lecturer1, lecturer2):
  if lecturer1.average_rating() == lecturer2.average_rating():
    return f"Лекторы {lecturer1.name} и {lecturer2.name} получили одинаковые средние оценки."
  elif lecturer1.average_rating() > lecturer2.average_rating():
    return f" {lecturer1.name} > {lecturer2.name}."
  else:
    return f" {lecturer2.name} > {lecturer1.name}"


class Student:
    def __init__(self, name, surname, gender):
        self.name = name
        self.surname = surname
        self.gender = gender
        self.finished_courses = []
        self.courses_in_progress = []
        self.grades = {}

    def add_courses(self, course_name):
        """Добавляет новый курс в список курсов студента."""
        self.courses_in_progress.append(course_name)

    def is_course_attached(self, course):
        """Проверяет, прикреплён ли данный курс к студенту."""
        return course in self.courses_in_progress

    def add_finished_course(self, course_name):
      """Метод для добавления завершённых курсов"""
      self.finished_courses.append(course_name)

    def rate_lecture(self, lecturer, course, grade):
        """Выставляет оценку лектору за проведенную лекцию."""
        if isinstance(lecturer, Lecturer) and self.is_course_attached(course) and \
           course in lecturer.courses_attached:
            lecturer.add_grade(course, grade)
        else:
            print("Ошибка")

    def add_courses(self, course_name):
        """Добавляет новый курс в список курсов студента."""
        self.courses_in_progress.append(course_name)

    def is_course_attached(self, course):
        """Проверяет, прикреплён ли данный курс к студенту."""
        return course in self.courses_in_progress


    def average_grades(self):
        """Подсчитывает среднюю оценку студента по всем домашним заданиям."""
        total_sum = sum(sum(grade_list) for grade_list in self.grades.values())
        total_count = sum(len(grade_list) for grade_list in self.grades.values()) or 0
        return round(total_sum / total_count, 1) if total_count > 0 else 0

    def __str__(self):
      return f'Имя: {self.name}\nФамилия: {self.surname}\nСредняя оценка за домашние задания: {self.average_grades()}\nКурсы в процессе изучения: {", ".join(self.courses_in_progress)}\nЗавершенные курсы: {", ".join(self.finished_courses)}'


def compare_students_by_average_grade(student1, student2, student3):
  if student1.average_grades() == student2.average_grades():
    return f'{student1.name} и {student2.name} имеют одинаковую среднюю оценку.'
  elif student1.average_grades() > student2.average_grades():
    return f"{student1.name} > {student2.name}"
  else:
    return f'{student2.name} > {student1.name}'

  """Подсчет средней оценки за домашнее задание по конкретному курсу."""
def average_hw_grade_for_course(students, course):
  grades = []
  for student in students:
    if course in student.grades:
      grades.extend(student.grades[course])
    return round(sum(grades) / len(grades), 1) if grades else 0

reviewer1 = Reviewer('Some', 'Buddy')
reviewer2 = Reviewer('William ', 'Brandywine')

lecturer1 = Lecturer('Derek ', 'Knight')
lecturer2 = Lecturer('Johnny ', 'Worthington ')

student1 = Student('Ruoy', 'Eman', 'male')
student2 = Student('Michael ', 'Wazowski', 'male')
student3 = Student('Margaret ', 'Gesner ', 'female')
# Добавляем курсы и оценки
student1.add_courses('Git')
student1.add_courses('Python')
student2.add_courses('Python')
student2.add_courses('Java')
student3.add_courses('Python')
student3.add_courses('Git')

student1.add_finished_course('Java')
student2.add_finished_course('Git')
student3.add_finished_course('Java')

reviewer1.courses_attached.append('Python')
reviewer1.courses_attached.append('Java')
reviewer2.courses_attached.append('Python')
reviewer2.courses_attached.append('Git')

lecturer1.courses_attached.append('Python')
lecturer1.courses_attached.append('Java')
lecturer2.courses_attached.append('Python')
lecturer2.courses_attached.append('Git')


reviewer1.rate_hw(student1, 'Python', 9)
reviewer1.rate_hw(student2, 'Python', 8)
reviewer1.rate_hw(student3, 'Python', 7)
reviewer2.rate_hw(student1, 'Python', 6)
reviewer2.rate_hw(student2, 'Python', 9)
reviewer2.rate_hw(student3, 'Python', 6)

student1.rate_lecture(lecturer1, 'Python', 9)
student1.rate_lecture(lecturer2, 'Python', 10)
student2.rate_lecture(lecturer1, 'Python', 8)
student2.rate_lecture(lecturer2, 'Python', 7)
student3.rate_lecture(lecturer1, 'Python', 6)
student3.rate_lecture(lecturer2, 'Python', 9)

students = [student1,student2, student3]
lecturers = [lecturer1, lecturer2]
average_student_grade = average_hw_grade_for_course(students, 'Python')
average_lecturer_grade = average_lecture_grade_for_course(lecturers, 'Python')

comparison_of_students = compare_students_by_average_grade(student1, student2, student3)
comparison_of_lecturers = compare_lecturers_by_average_rating(lecturer1, lecturer2)

# Печать информации о каждом объекте
print('\nИнформация о студентах:')

print(student1)
print(student2)
print(student3)

print('\nИнформация о проверяющих :')
print(reviewer1)
print(reviewer2)

print('\nИнформация о лекторах:')
print(lecturer1)
print(lecturer2)


print(f"\nСредняя оценка за домашние задания по курсу 'Python': {average_student_grade}")
print(f"\nСредняя оценка за лекции по курсу 'Python': {average_lecturer_grade}")

print(f"\nСравнение лекторов по средней оценке за лекции: {comparison_of_lecturers}")
print(f"\nСравнение студентов по средней оценке за домашние задания: {comparison_of_students}")