#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_COURSES 10 //Based on available courses 
#define MAX_STUDENTS 100 //Based on school capacity

//Course declaration
typedef struct {
    char course_name[50];
    int capacity;
    int current_students;
} Course;

//Declaration
typedef struct {
    char name[50];
    int score;
    int course_index;
    int is_approved;
} Student;

//Accessing the CoursesStruct & StudentStruct
Course courses[MAX_COURSES];
Student students[MAX_STUDENTS];
int num_courses = 0;
int num_students = 0;

// Functions
void addCourse();
void applyForCourse();
void approveDeclineApplications();
void generateReports();


//Main function for the program
int main() {
    int choice;
    
    do {
        printf("\nSTUDENT MANAGEMENT SYSTEM\n");
        printf("=====================================\n");
        printf("1. Add courses\n");
        printf("2. Apply for course\n");
        printf("3. Approve/Decline course application\n");
        printf("4. Generate Reports\n");
        printf("5. Exit\n");
        printf("-------------------------------------\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch(choice) {
            case 1:
                addCourse();
                break;
            case 2:
                applyForCourse();
                break;
            case 3:
                approveDeclineApplications();
                break;
            case 4:
                generateReports();
                break;
            case 5:
            printf("====================================\n");
                printf("Exiting program. Goodbye!\n");
                break;
            default:
            printf("==================================================\n");
                printf("Invalid choice! Please enter a valid option.\n");
        }
    } while(choice != 5);

    return 0;
}


//Adding courses
void addCourse() {
    if (num_courses == MAX_COURSES) {
        printf("================================================\n");
        printf("Cannot add more courses. Maximum limit reached.\n");
        return;
    }
    printf("======================================\n");
    printf("Enter course name: ");
    scanf("%s", courses[num_courses].course_name);
    printf("======================================\n");
    printf("Enter course capacity: ");
    scanf("%d", &courses[num_courses].capacity);
    courses[num_courses].current_students = 0;
    num_courses++;
    printf("======================================\n");
    printf("Course added successfully.\n");
}

//Applying for courses
void applyForCourse() {
    if (num_students == MAX_STUDENTS) {
        printf("==================================\n");
        printf("Cannot accept more applications. Maximum limit reached.\n");
        return;
    }
    printf("=========================================\n");
    printf("Enter student name: ");
    scanf("%s", students[num_students].name);
    printf("=========================================\n");
    printf("Enter student score: ");
    scanf("%d", &students[num_students].score);
    printf("=========================================\n");
    printf("Available courses:\n");
    for (int i = 0; i < num_courses; i++) {
        printf("%d. %s\n", i+1, courses[i].course_name);
    }
    printf("=========================================\n");
    printf("Enter course number to apply: ");
    scanf("%d", &students[num_students].course_index);
    students[num_students].is_approved = 0;
    num_students++;
    printf("========================================\n");
    printf("Application submitted successfully.\n");
}

//For approving or declining applications based on student score
void approveDeclineApplications() {
    for (int i = 0; i < num_students; i++) {
        if (!students[i].is_approved) {
            int course_index = students[i].course_index - 1;
            int score = students[i].score;
            if (score >= 70 && score <= 100 && courses[course_index].current_students < courses[course_index].capacity) {
                courses[course_index].current_students++;
                students[i].is_approved = 1;
                printf("==============================================\n");
                printf("Application approved for %s\n", students[i].name);
            } else {

                // Marks not approved if score is below 70
                students[i].is_approved = 0; 
                printf("================================================\n");
                printf("Application declined for %s due to score criteria.\n", students[i].name);
            }
        }
    }
}

//For generating reports
void generateReports() {
    if (num_courses == 0) {
        printf("====================================================================\n");
        printf("No courses available. Do you want to add courses? (1: Yes, 2: No): ");
        int choice;
        scanf("%d", &choice);
        if (choice == 1) {
            addCourse();
            return;
        } else {
            printf("===============================\n");
            printf("Exiting report generation.\n");
            return;
        }
    }
    printf("===============================\n");
    printf("List of available courses:\n");
    for (int i = 0; i < num_courses; i++) {
        printf("%s (Capacity: %d, Current Students: %d)\n", courses[i].course_name, courses[i].capacity, courses[i].current_students);
    }
    printf("=====================================\n");
    printf("\nList of successful applicants:\n");
    int success = 0;
    for (int i = 0; i < num_students; i++) {
        if (students[i].is_approved) {
            printf("%s - Course: %s\n", students[i].name, courses[students[i].course_index - 1].course_name);
            success = 1;
        }
    }
    if (!success) {
        printf("============================\n");
        printf("No successful applicants.\n");
    }
    printf("======================================\n");
    printf("\nList of unsuccessful applicants:\n");
    int failure = 0;
    for (int i = 0; i < num_students; i++) {
        if (!students[i].is_approved || students[i].score < 70) {
            printf("%s - Course: %s\n", students[i].name, courses[students[i].course_index - 1].course_name);
            failure = 1;
        }
    }
    if (!failure) {
        printf("==============================\n");
        printf("No unsuccessful applicants.\n");
    }
}
