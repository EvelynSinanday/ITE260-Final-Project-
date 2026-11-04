import math
from datetime import datetime

expenses = []
budget = 0.0


# ==============================
# ADD / SET BUDGET
# ==============================
def add_set_budget():
    global budget

    while True:
        try:
            new_budget = float(
                input("Enter additional budget: ₱")
            )

            if not math.isfinite(new_budget) or new_budget <= 0:
                print("Budget must be a finite number greater than 0.")
                continue

            if not math.isfinite(budget + new_budget):
                print("The updated budget is too large to store.")
                continue

            budget += new_budget

            print("\nBudget successfully added for this session!")
            print(f"Current Student Budget: ₱{budget:.2f}")

            break

        except ValueError:
            print("Invalid input. Please enter a valid number.")


# ==============================
# GET CURRENT DATE
# ==============================
def get_valid_date():
    # Automatically use the computer's current local date.
    return datetime.now().strftime("%Y-%m-%d")


# ==============================
# ADD NEW EXPENSE
# ==============================
def add_new_expense():

    print("\n========== ADD NEW EXPENSE ==========")

    remaining_budget = budget - sum(
        expense["Amount"] for expense in expenses
    )

    if remaining_budget <= 0:
        print(
            "\nYou're out of budget. Please add a budget "
            "before recording an expense."
        )
        return

    date = get_valid_date()
    print(f"Expense date (today): {date}")

    while True:
        try:
            amount = float(
                input("Enter expense amount: ₱")
            )

            if not math.isfinite(amount) or amount <= 0:
                print("Amount must be a finite number greater than 0.")
                continue

            if amount > remaining_budget:
                print(
                    f"Expense exceeds your remaining budget "
                    f"of ₱{remaining_budget:.2f}."
                )
                continue

            break

        except ValueError:
            print(
                "Invalid amount. Please enter a valid number."
            )

    categories = {
        "1": "Food",
        "2": "Transportation",
        "3": "School Supplies",
        "4": "Others"
    }

    while True:
        print("\nChoose an expense category:")
        for number, name in categories.items():
            print(f"{number}. {name}")

        category_choice = input("Enter category number (1-4): ").strip()
        if category_choice in categories:
            category = categories[category_choice]
            break
        print("Invalid category. Please choose a number from 1 to 4.")

    while True:
        description = input("Enter expense description: ").strip()
        if description:
            break
        print("Description cannot be empty.")

    expense = {
        "Date": date,
        "Amount": amount,
        "Category": category,
        "Description": description
    }

    expenses.append(expense)

    print("\nExpense successfully recorded!")
    print("Expense recorded for this session.")


# ==============================
# VIEW ALL RECORDED EXPENSES
# ==============================
def view_all_recorded_expenses():

    if len(expenses) == 0:
        print("\nNo expenses recorded yet.")
        return

    print("\n==============================================")
    print("          ALL RECORDED EXPENSES")
    print("==============================================")

    for number, expense in enumerate(expenses, start=1):

        print(f"\nExpense #{number}")
        print(f"Date        : {expense['Date']}")
        print(f"Amount      : ₱{expense['Amount']:.2f}")
        print(f"Category    : {expense['Category']}")
        print(f"Description : {expense['Description']}")

    print("\n==============================================")


# ==============================
# DISPLAY TOTAL EXPENSES
# AND REMAINING BUDGET
# ==============================
def display_total_expenses_calculated():

    total = 0.0

    for expense in expenses:
        total += expense["Amount"]

    remaining = budget - total

    print("\n================================")
    print("        EXPENSE SUMMARY")
    print("================================")
    print(f"Student Budget  : ₱{budget:.2f}")
    print(f"Total Expenses  : ₱{total:.2f}")
    print(f"Remaining Budget: ₱{remaining:.2f}")
    print("================================")


# ==============================
# DISPLAY BUDGET ANALYTICS
# ==============================
def display_budget_analytics():

    total = sum(expense["Amount"] for expense in expenses)
    remaining = budget - total
    category_totals = {}

    for expense in expenses:
        category = expense["Category"].strip() or "Uncategorized"
        category_totals[category] = (
            category_totals.get(category, 0.0) + expense["Amount"]
        )

    print("\n========================================")
    print("           BUDGET ANALYTICS")
    print("========================================")
    print(f"Total Budget    : ₱{budget:.2f}")
    print(f"Total Spent     : ₱{total:.2f}")

    if budget > 0:
        percent_used = total / budget * 100
        print(f"Budget Used     : {percent_used:.1f}%")
    else:
        print("Budget Used     : N/A")

    if remaining >= 0:
        print(f"Budget Remaining: ₱{remaining:.2f}")
    else:
        print(f"Over Budget By  : ₱{abs(remaining):.2f}")

    if budget <= 0:
        print("Status          : No budget set")
        print("Suggestion      : Add a budget before recording expenses.")
    elif remaining < 0:
        print("Status          : OVER BUDGET")
        print(
            "Suggestion      : Pause non-essential spending and review "
            "your expenses."
        )
    elif remaining == 0:
        print("Status          : BUDGET FULLY SPENT")
        print(
            "Suggestion      : You are out of budget. Add more budget "
            "before recording another expense."
        )
    elif total == 0:
        print("Status          : No expenses recorded")
        print("Suggestion      : Record expenses to keep your report up to date.")
    elif remaining / budget <= 0.2:
        print("Status          : NEAR BUDGET LIMIT")
        print(
            "Suggestion      : Only 20% or less remains; delay non-essential "
            "purchases."
        )
    else:
        print("Status          : WITHIN BUDGET")
        print("Suggestion      : Keep tracking expenses to stay within budget.")

    if category_totals:
        print("\nSpending by Category:")
        for category, amount in sorted(
            category_totals.items(),
            key=lambda item: item[1],
            reverse=True
        ):
            print(f"  {category}: ₱{amount:.2f}")

        highest_category = max(category_totals, key=category_totals.get)
        if remaining < 0 or (budget > 0 and remaining / budget <= 0.2):
            print(
                f"Review '{highest_category}' first; it is your "
                "highest-spending category."
            )

    print("========================================")


# ==============================
# SHOW EXPENSE LIST
# ==============================
def show_expense_list():

    if len(expenses) == 0:
        print("\nNo expenses recorded yet.")
        return

    print("\n==============================================================")
    print("                      EXPENSE LIST")
    print("==============================================================")

    print(
        f"{'No.':<5}"
        f"{'Date':<15}"
        f"{'Amount':<12}"
        f"{'Category':<20}"
        f"Description"
    )

    print("-" * 70)

    for number, expense in enumerate(expenses, start=1):

        print(
            f"{number:<5}"
            f"{expense['Date']:<15}"
            f"₱{expense['Amount']:<11.2f}"
            f"{expense['Category']:<20}"
            f"{expense['Description']}"
        )

    print("==============================================================")


# ==============================
# DISPLAY MENU
# ==============================
def display_menu():

    print("\n========================================")
    print("       STUDENT EXPENSE TRACKER")
    print("========================================")
    print("1. Add/Set Budget Input")
    print("2. Add New Expense")
    print("3. View All Recorded Expenses")
    print("4. Display Total Expenses Calculated")
    print("5. Show Expense List")
    print("6. View Budget Analytics")
    print("7. Exit")
    print("========================================")


# ==============================
# MAIN PROGRAM
# ==============================
def main():

    print("========================================")
    print("       STUDENT EXPENSE TRACKER")
    print("========================================")
    print("Budget and expenses are kept for this session only.")

    while True:

        display_menu()

        choice = input("Enter your choice: ")

        if choice == "1":
            add_set_budget()

        elif choice == "2":
            add_new_expense()

        elif choice == "3":
            view_all_recorded_expenses()

        elif choice == "4":
            display_total_expenses_calculated()

        elif choice == "5":
            show_expense_list()

        elif choice == "6":
            display_budget_analytics()

        elif choice == "7":
            print("\nThank you for using Student Expense Tracker!")
            print("Program terminated.")
            break

        else:
            print("\nInvalid choice. Please select 1-7.")


# ==============================
# START PROGRAM
# ==============================
if __name__ == "__main__":
    main()
