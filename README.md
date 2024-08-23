# Coding-Raja-Technologies-Internship
import csv
from datetime import datetime

TASKS_FILE = 'tasks.csv'

def add_task():
    description = input("Enter task description: ")
    priority = input("Enter task priority (high, medium, low): ").lower()
    due_date = input("Enter due date (YYYY-MM-DD): ")
    task = {
        'description': description,
        'priority': priority,
        'due_date': due_date,
        'completed': 'No'
    }
    with open(TASKS_FILE, 'a', newline='') as file:
        writer = csv.DictWriter(file, fieldnames=task.keys())
        if file.tell() == 0:  # If the file is empty, write the header
            writer.writeheader()
        writer.writerow(task)
    print(f"Added task: {description}")

def remove_task():
    task_id = int(input("Enter task ID to remove: "))
    tasks = []
    with open(TASKS_FILE, 'r') as file:
        reader = csv.DictReader(file)
        tasks = list(reader)
    
    if task_id < 1 or task_id > len(tasks):
        print(f"Task ID {task_id} not found.")
        return
    
    removed_task = tasks.pop(task_id - 1)
    with open(TASKS_FILE, 'w', newline='') as file:
        writer = csv.DictWriter(file, fieldnames=removed_task.keys())
        writer.writeheader()
        writer.writerows(tasks)
    print(f"Removed task: {removed_task['description']}")

def complete_task():
    task_id = int(input("Enter task ID to mark as completed: "))
    tasks = []
    with open(TASKS_FILE, 'r') as file:
        reader = csv.DictReader(file)
        tasks = list(reader)
    
    if task_id < 1 or task_id > len(tasks):
        print(f"Task ID {task_id} not found.")
        return
    
    tasks[task_id - 1]['completed'] = 'Yes'
    with open(TASKS_FILE, 'w', newline='') as file:
        writer = csv.DictWriter(file, fieldnames=tasks[0].keys())
        writer.writeheader()
        writer.writerows(tasks)
    print(f"Marked task as completed: {tasks[task_id - 1]['description']}")

def list_tasks():
    with open(TASKS_FILE, 'r') as file:
        reader = csv.DictReader(file)
        tasks = list(reader)
        if not tasks:
            print("No tasks found.")
            return
        print(f"{'ID':<4}{'Description':<20}{'Priority':<10}{'Due Date':<12}{'Completed'}")
        print("-" * 60)
        for i, task in enumerate(tasks, start=1):
            print(f"{i:<4}{task['description']:<20}{task['priority']:<10}{task['due_date']:<12}{task['completed']}")

def main():
    while True:
        print("\nTo-Do List Application")
        print("1. Add Task")
        print("2. Remove Task")
        print("3. Complete Task")
        print("4. List Tasks")
        print("5. Exit")
        choice = input("Enter your choice: ")
        
        if choice == '1':
            add_task()
        elif choice == '2':
            remove_task()
        elif choice == '3':
            complete_task()
        elif choice == '4':
            list_tasks()
        elif choice == '5':
            print("Exiting...")
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == '__main__':
    main()

