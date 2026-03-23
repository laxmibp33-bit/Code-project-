 import tkinter as tk
from tkinter import messagebox

def calculate_fcfs():
    try:
        burst = list(map(int, entry.get().split()))
        n = len(burst)
        
        if n == 0:
            messagebox.showerror("Error", "Please enter burst times!")
            return
        
        wt = [0] * n
        tat = [0] * n
        
        # Calculate Waiting Time
        for i in range(1, n):
            wt[i] = wt[i-1] + burst[i-1]
        
        # Calculate Turnaround Time
        for i in range(n):
            tat[i] = wt[i] + burst[i]
        
        # Prepare Output
        result = "Process  WT  TAT\n"
        result += "-" * 25 + "\n"
        
        for i in range(n):
            result += f"P{i+1}      {wt[i]}    {tat[i]}\n"
        
        avg_wt = sum(wt) / n
        avg_tat = sum(tat) / n
        
        result += "\nAverage WT: {:.2f}".format(avg_wt)
        result += "\nAverage TAT: {:.2f}".format(avg_tat)
        
        output_label.config(text=result)
        
        # Gantt Chart
        gantt = "Gantt Chart:\n|"
        for i in range(n):
            gantt += f" P{i+1} |"
        gantt_label.config(text=gantt)
    
    except ValueError:
        messagebox.showerror("Error", "Enter numbers separated by space!")

def clear_all():
    entry.delete(0, tk.END)
    output_label.config(text="")
    gantt_label.config(text="")

# Window
window = tk.Tk()
window.title("Laxmi OS Mini Project")
window.geometry("500x500")

# Heading
label = tk.Label(window, text="CPU Scheduling Simulator", font=("Arial", 16, "bold"))
label.pack(pady=10)

# Input
tk.Label(window, text="Enter Burst Times (space separated):").pack()
entry = tk.Entry(window, width=40)
entry.pack(pady=5)

# Buttons
tk.Button(window, text="Calculate FCFS", command=calculate_fcfs, bg="lightgreen").pack(pady=5)
tk.Button(window, text="Clear", command=clear_all, bg="lightcoral").pack(pady=5)

# Output
output_label = tk.Label(window, text="", justify="left")
output_label.pack(pady=10)

# Gantt Chart
gantt_label = tk.Label(window, text="", fg="blue")
gantt_label.pack()

window.mainloop()
