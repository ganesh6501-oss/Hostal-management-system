# Hostal-management-system
Hostal management system
#include <stdio.h>
#include <string.h>

#define MAX 100

// Structure for Student
struct Student {
    char id[20];
    char name[50];
    char room[10];
    int fees_paid;
    int total_fees;
};

struct Student students[MAX];
int count = 0;

// Find student index
int findStudent(char id[]) {
    for (int i = 0; i < count; i++) {
        if (strcmp(students[i].id, id) == 0) {
            return i;
        }
    }
    return -1;
}

// Add Student
void addStudent() {
    if (count >= MAX) {
        printf("Limit reached!\n");
        return;
    }

    printf("Enter Student ID: ");
    scanf("%s", students[count].id);

    if (findStudent(students[count].id) != -1) {
        printf("Student already exists!\n");
        return;
    }

    printf("Enter Name: ");
    scanf("%s", students[count].name);

    printf("Enter Total Fees: ");
    scanf("%d", &students[count].total_fees);

    students[count].fees_paid = 0;
    strcpy(students[count].room, "None");

    count++;
    printf("Student added successfully!\n");
}

// Allocate Room
void allocateRoom() {
    char id[20], room[10];
    printf("Enter Student ID: ");
    scanf("%s", id);

    int index = findStudent(id);
    if (index == -1) {
        printf("Student not found!\n");
        return;
    }

    if (strcmp(students[index].room, "None") != 0) {
        printf("Student already has a room!\n");
        return;
    }

    printf("Enter Room No: ");
    scanf("%s", room);

    // Check if room already taken
    for (int i = 0; i < count; i++) {
        if (strcmp(students[i].room, room) == 0) {
            printf("Room already occupied!\n");
            return;
        }
    }

    strcpy(students[index].room, room);
    printf("Room allocated successfully!\n");
}

// Deallocate Room
void deallocateRoom() {
    char id[20];
    printf("Enter Student ID: ");
    scanf("%s", id);

    int index = findStudent(id);
    if (index == -1) {
        printf("Student not found!\n");
        return;
    }

    if (strcmp(students[index].room, "None") == 0) {
        printf("No room assigned!\n");
    } else {
        strcpy(students[index].room, "None");
        printf("Room deallocated!\n");
    }
}

// Pay Fees
void payFees() {
    char id[20];
    int amount;

    printf("Enter Student ID: ");
    scanf("%s", id);

    int index = findStudent(id);
    if (index == -1) {
        printf("Student not found!\n");
        return;
    }

    printf("Enter amount to pay: ");
    scanf("%d", &amount);

    if (students[index].fees_paid + amount > students[index].total_fees) {
        printf("Payment exceeds total fees!\n");
        return;
    }

    students[index].fees_paid += amount;
    printf("Payment successful!\n");
}

// Check Due Fees
void checkDue() {
    char id[20];
    printf("Enter Student ID: ");
    scanf("%s", id);

    int index = findStudent(id);
    if (index == -1) {
        printf("Student not found!\n");
        return;
    }

    int due = students[index].total_fees - students[index].fees_paid;

    printf("Total Fees: %d\n", students[index].total_fees);
    printf("Paid: %d\n", students[index].fees_paid);
    printf("Due: %d\n", due);
}

// Display Students
void displayStudents() {
    if (count == 0) {
        printf("No students available.\n");
        return;
    }

    for (int i = 0; i < count; i++) {
        printf("\nID: %s\n", students[i].id);
        printf("Name: %s\n", students[i].name);
        printf("Room: %s\n", students[i].room);
        printf("Paid: %d\n", students[i].fees_paid);
    }
}

// Main Menu
int main() {
    int choice;

    while (1) {
        printf("\n--- Hostel Management System ---\n");
        printf("1. Add Student\n");
        printf("2. Allocate Room\n");
        printf("3. Deallocate Room\n");
        printf("4. Display Students\n");
        printf("5. Pay Fees\n");
        printf("6. Check Due Fees\n");
        printf("7. Exit\n");

        printf("Enter choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: addStudent(); break;
            case 2: allocateRoom(); break;
            case 3: deallocateRoom(); break;
            case 4: displayStudents(); break;
            case 5: payFees(); break;
            case 6: checkDue(); break;
            case 7: printf("Exiting...\n"); return 0;
            default: printf("Invalid choice!\n");
        }
    }

    return 0;
}
