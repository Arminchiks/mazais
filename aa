import tkinter as tk
from tkinter import messagebox
import sqlite3


conn = sqlite3.connect("calculator_history.db")
cursor = conn.cursor()
cursor.execute("""
CREATE TABLE IF NOT EXISTS history (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    expression TEXT,
    result TEXT
)
""")
conn.commit()


def calculate():
    expression = entry.get()
    try:
        result = str(eval(expression))
        entry.delete(0, tk.END)
        entry.insert(tk.END, result)

        
        cursor.execute("INSERT INTO history (expression, result) VALUES (?, ?)", (expression, result))
        conn.commit()

    except Exception as e:
        messagebox.showerror("Kļūda", f"Nav iespējams\n{e}")

def show_history():
    history_window = tk.Toplevel(root)
    history_window.title("Vēsture")

    cursor.execute("SELECT expression, result FROM history ORDER BY id DESC LIMIT 10")
    rows = cursor.fetchall()

    for i, (expression, result) in enumerate(rows):
        label = tk.Label(history_window, text=f"{expression} = {result}")
        label.pack()

root = tk.Tk()
root.title("Kalkulators")

entry = tk.Entry(root, width=30, font=('Arial', 18))
entry.grid(row=0, column=0, columnspan=4, padx=10, pady=10)

buttons = [
    '7', '8', '9', '/',
    '4', '5', '6', '*',
    '1', '2', '3', '-',
    '0', '.', '=', '+'
]


row = 1
col = 0
for button in buttons:
    def click(b=button):
        if b == '=':
            calculate()
        else:
            entry.insert(tk.END, b)

    tk.Button(root, text=button, width=5, height=2, font=('Arial', 18), command=click).grid(row=row, column=col)
    col += 1
    if col > 3:
        col = 0
        row += 1


tk.Button(root, text='Vēsture', width=10, command=show_history).grid(row=row, column=0, columnspan=2)
tk.Button(root, text='Izdzēst visu', width=10, command=lambda: entry.delete(0, tk.END)).grid(row=row, column=2, columnspan=2)

root.mainloop()


conn.close()
