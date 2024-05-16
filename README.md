import tkinter as tk
from tkinter import ttk, messagebox

def calculate_squat(speed, beam, draft, block_coefficient):
    try:
        squat = (float(speed) ** 2) / (100 * (float(beam) / float(draft)) * float(block_coefficient))
        return squat
    except ZeroDivisionError:
        return "Invalid calculation: Division by zero."

def calculate_ukc(depth_of_water, draft, tide_height, squat, other_factors):
    """
    Calculate the Under Keel Clearance (UKC).

    Parameters:
    depth_of_water (float): Depth of the water.
    draft (float): Draft of the vessel.
    tide_height (float): Height of the tide.
    squat (float): Squat of the vessel.
    other_factors (float): Any other factors affecting UKC.

    Returns:
    float: The Under Keel Clearance (UKC).
    """
    ukc = depth_of_water + tide_height - draft - squat - other_factors
    return ukc

def calculate_and_display_ukc():
    try:
        depth_of_water = float(entry_depth.get())
        draft = float(entry_draft.get())
        tide_height = float(entry_tide.get())
        speed = float(entry_speed.get())
        beam = float(entry_beam.get())
        block_coefficient = float(entry_block_coefficient.get())
        other_factors = float(entry_other_factors.get())

        squat = calculate_squat(speed, beam, draft, block_coefficient)
        if isinstance(squat, str):  # Check for error message
            messagebox.showerror("Error", squat)
            return
        
        ukc = calculate_ukc(depth_of_water, draft, tide_height, squat, other_factors)
        label_result.config(text=f"The Under Keel Clearance (UKC) is: {ukc:.2f} meters")
    except ValueError:
        messagebox.showerror("Invalid Input", "Please enter valid numeric values.")

# Create the main window
root = tk.Tk()
root.title("UKC Calculator")
root.geometry("400x550")

# Create and place the widgets
label_title = tk.Label(root, text="UKC Calculator", font=("Helvetica", 16, "bold"))
label_title.pack(pady=10)

frame_inputs = ttk.Frame(root, padding="10 10 10 10")
frame_inputs.pack(fill=tk.BOTH, expand=True)

# Depth of water
ttk.Label(frame_inputs, text="Depth of water (meters):").grid(row=0, column=0, sticky=tk.W, pady=5)
entry_depth = ttk.Entry(frame_inputs, width=20)
entry_depth.grid(row=0, column=1, pady=5)

# Draft of the vessel
ttk.Label(frame_inputs, text="Draft of the vessel (meters):").grid(row=1, column=0, sticky=tk.W, pady=5)
entry_draft = ttk.Entry(frame_inputs, width=20)
entry_draft.grid(row=1, column=1, pady=5)

# Tide height
ttk.Label(frame_inputs, text="Tide height (meters):").grid(row=2, column=0, sticky=tk.W, pady=5)
entry_tide = ttk.Entry(frame_inputs, width=20)
entry_tide.grid(row=2, column=1, pady=5)

# Speed of the vessel
ttk.Label(frame_inputs, text="Speed of the vessel (knots):").grid(row=3, column=0, sticky=tk.W, pady=5)
entry_speed = ttk.Entry(frame_inputs, width=20)
entry_speed.grid(row=3, column=1, pady=5)

# Beam of the vessel
ttk.Label(frame_inputs, text="Beam (width) of the vessel (meters):").grid(row=4, column=0, sticky=tk.W, pady=5)
entry_beam = ttk.Entry(frame_inputs, width=20)
entry_beam.grid(row=4, column=1, pady=5)

# Block coefficient of the vessel
ttk.Label(frame_inputs, text="Block coefficient of the vessel:").grid(row=5, column=0, sticky=tk.W, pady=5)
entry_block_coefficient = ttk.Entry(frame_inputs, width=20)
entry_block_coefficient.grid(row=5, column=1, pady=5)

# Other factors
ttk.Label(frame_inputs, text="Other factors affecting UKC (meters):").grid(row=6, column=0, sticky=tk.W, pady=5)
entry_other_factors = ttk.Entry(frame_inputs, width=20)
entry_other_factors.grid(row=6, column=1, pady=5)

# Calculate button
button_calculate = ttk.Button(frame_inputs, text="Calculate UKC", command=calculate_and_display_ukc)
button_calculate.grid(row=7, column=0, columnspan=2, pady=20)

# Result label
label_result = tk.Label(root, text="", font=("Helvetica", 12, "bold"))
label_result.pack(pady=10)

# Run the application
root.mainloop()
