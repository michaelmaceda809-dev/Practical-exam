import tkinter as tk
from tkinter import ttk, messagebox


# ==============================
# MAIN WINDOW
# ==============================

window = tk.Tk()
window.title("Employee Profile")
window.geometry("650x500")
window.resizable(False, False)


# ==============================
# MINIMALIST COLORS
# ==============================

BG_COLOR = "#F5F5F5"          # Light gray background
TITLE_COLOR = "#222222"       # Dark black
BUTTON_COLOR = "#333333"      # Dark gray buttons
BUTTON_HOVER = "#555555"      # Slightly lighter gray
TEXT_COLOR = "#FFFFFF"        # White text
ENTRY_BG = "#FFFFFF"          # White input fields
BORDER_COLOR = "#D9D9D9"      # Soft gray border

window.configure(bg=BG_COLOR)


# ==============================
# VARIABLES
# ==============================

name_var = tk.StringVar()
age_var = tk.StringVar()
position_var = tk.StringVar()


# ==============================
# FUNCTIONS
# ==============================

def clear_fields():
    name_var.set("")
    age_var.set("")
    position_var.set("")


def add_employee():
    name = name_var.get().strip()
    age = age_var.get().strip()
    position = position_var.get().strip()

    if name == "" or age == "" or position == "":
        messagebox.showwarning(
            "Warning",
            "Please fill in all fields."
        )
        return

    if not age.isdigit():
        messagebox.showerror(
            "Error",
            "Age must be a number."
        )
        return

    employee_table.insert(
        "",
        tk.END,
        values=(name, age, position)
    )

    messagebox.showinfo(
        "Success",
        "Employee added successfully!"
    )

    clear_fields()


def select_employee(event):
    selected = employee_table.selection()

    if selected:
        item = employee_table.item(selected[0])
        values = item["values"]

        name_var.set(values[0])
        age_var.set(values[1])
        position_var.set(values[2])


def update_employee():
    selected = employee_table.selection()

    if not selected:
        messagebox.showwarning(
            "Warning",
            "Please select an employee to update."
        )
        return

    name = name_var.get().strip()
    age = age_var.get().strip()
    position = position_var.get().strip()

    if name == "" or age == "" or position == "":
        messagebox.showwarning(
            "Warning",
            "Please fill in all fields."
        )
        return

    if not age.isdigit():
        messagebox.showerror(
            "Error",
            "Age must be a number."
        )
        return

    employee_table.item(
        selected[0],
        values=(name, age, position)
    )

    messagebox.showinfo(
        "Success",
        "Employee updated successfully!"
    )

    clear_fields()


def delete_employee():
    selected = employee_table.selection()

    if not selected:
        messagebox.showwarning(
            "Warning",
            "Please select an employee to delete."
        )
        return

    confirm = messagebox.askyesno(
        "Confirm Delete",
        "Are you sure you want to delete this employee?"
    )

    if confirm:
        employee_table.delete(selected[0])
        clear_fields()

        messagebox.showinfo(
            "Success",
            "Employee deleted successfully!"
        )


def exit_program():
    confirm = messagebox.askyesno(
        "Exit",
        "Are you sure you want to exit?"
    )

    if confirm:
        window.destroy()


# ==============================
# TITLE
# ==============================

title_label = tk.Label(
    window,
    text="EMPLOYEE PROFILE",
    font=("Arial", 20, "bold"),
    bg=BG_COLOR,
    fg=TITLE_COLOR
)

title_label.pack(pady=15)


# ==============================
# INPUT FRAME
# ==============================

input_frame = tk.Frame(
    window,
    bg=BG_COLOR
)

input_frame.pack(pady=5)


# ==============================
# EMPLOYEE NAME
# ==============================

tk.Label(
    input_frame,
    text="Employee Name:",
    font=("Arial", 11),
    bg=BG_COLOR,
    fg=TITLE_COLOR
).grid(
    row=0,
    column=0,
    padx=10,
    pady=8,
    sticky="e"
)

name_entry = tk.Entry(
    input_frame,
    textvariable=name_var,
    width=35,
    bg=ENTRY_BG,
    fg=TITLE_COLOR,
    relief="solid",
    bd=1
)

name_entry.grid(
    row=0,
    column=1,
    padx=10,
    pady=8
)


# ==============================
# AGE
# ==============================

tk.Label(
    input_frame,
    text="Age:",
    font=("Arial", 11),
    bg=BG_COLOR,
    fg=TITLE_COLOR
).grid(
    row=1,
    column=0,
    padx=10,
    pady=8,
    sticky="e"
)

age_entry = tk.Entry(
    input_frame,
    textvariable=age_var,
    width=35,
    bg=ENTRY_BG,
    fg=TITLE_COLOR,
    relief="solid",
    bd=1
)

age_entry.grid(
    row=1,
    column=1,
    padx=10,
    pady=8
)


# ==============================
# POSITION
# ==============================

tk.Label(
    input_frame,
    text="Position:",
    font=("Arial", 11),
    bg=BG_COLOR,
    fg=TITLE_COLOR
).grid(
    row=2,
    column=0,
    padx=10,
    pady=8,
    sticky="e"
)

position_entry = tk.Entry(
    input_frame,
    textvariable=position_var,
    width=35,
    bg=ENTRY_BG,
    fg=TITLE_COLOR,
    relief="solid",
    bd=1
)

position_entry.grid(
    row=2,
    column=1,
    padx=10,
    pady=8
)


# ==============================
# BUTTON FRAME
# ==============================

button_frame = tk.Frame(
    window,
    bg=BG_COLOR
)

button_frame.pack(pady=10)


# ==============================
# BUTTON STYLE
# ==============================

button_style = {
    "width": 10,
    "bg": BUTTON_COLOR,
    "fg": TEXT_COLOR,
    "font": ("Arial", 9, "bold"),
    "relief": "flat",
    "bd": 0,
    "cursor": "hand2",
    "activebackground": BUTTON_HOVER,
    "activeforeground": TEXT_COLOR
}


# ==============================
# ADD
# ==============================

add_button = tk.Button(
    button_frame,
    text="Add",
    command=add_employee,
    **button_style
)

add_button.grid(
    row=0,
    column=0,
    padx=5
)


# ==============================
# UPDATE
# ==============================

update_button = tk.Button(
    button_frame,
    text="Update",
    command=update_employee,
    **button_style
)

update_button.grid(
    row=0,
    column=1,
    padx=5
)


# ==============================
# DELETE
# ==============================

delete_button = tk.Button(
    button_frame,
    text="Delete",
    command=delete_employee,
    **button_style
)

delete_button.grid(
    row=0,
    column=2,
    padx=5
)


# ==============================
# CLEAR
# ==============================

clear_button = tk.Button(
    button_frame,
    text="Clear",
    command=clear_fields,
    **button_style
)

clear_button.grid(
    row=0,
    column=3,
    padx=5
)


# ==============================
# EXIT
# ==============================

exit_button = tk.Button(
    button_frame,
    text="Exit",
    command=exit_program,
    **button_style
)

exit_button.grid(
    row=0,
    column=4,
    padx=5
)


# ==============================
# EMPLOYEE INFORMATION
# ==============================

info_label = tk.Label(
    window,
    text="EMPLOYEE INFORMATION",
    font=("Arial", 14, "bold"),
    bg=BG_COLOR,
    fg=TITLE_COLOR
)

info_label.pack(pady=(10, 5))


# ==============================
# TABLE FRAME
# ==============================

table_frame = tk.Frame(
    window,
    bg=BG_COLOR
)

table_frame.pack(pady=5)


# ==============================
# TREEVIEW STYLE
# ==============================

style = ttk.Style()

style.theme_use("clam")

style.configure(
    "Treeview",
    background="#FFFFFF",
    foreground="#222222",
    rowheight=28,
    fieldbackground="#FFFFFF",
    font=("Arial", 10),
    borderwidth=1
)

style.configure(
    "Treeview.Heading",
    background="#333333",
    foreground="#FFFFFF",
    font=("Arial", 10, "bold"),
    relief="flat"
)

style.map(
    "Treeview",
    background=[("selected", "#D9D9D9")],
    foreground=[("selected", "#222222")]
)


# ==============================
# EMPLOYEE TABLE
# ==============================

employee_table = ttk.Treeview(
    table_frame,
    columns=("Name", "Age", "Position"),
    show="headings",
    height=8
)


# TABLE HEADINGS

employee_table.heading(
    "Name",
    text="Name"
)

employee_table.heading(
    "Age",
    text="Age"
)

employee_table.heading(
    "Position",
    text="Position"
)


# TABLE COLUMNS

employee_table.column(
    "Name",
    width=200,
    anchor="center"
)

employee_table.column(
    "Age",
    width=80,
    anchor="center"
)

employee_table.column(
    "Position",
    width=200,
    anchor="center"
)

employee_table.pack()


# ==============================
# EVENT HANDLING
# ==============================

employee_table.bind(
    "<<TreeviewSelect>>",
    select_employee
)


# ==============================
# RUN APPLICATION
# ==============================

window.mainloop()
