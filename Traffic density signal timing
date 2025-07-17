import tkinter as tk
from tkinter import ttk
import random
import time
import threading

class TrafficSignal:
    def _init_(self, name, display_frame):
        self.name = name
        self.green_light_duration = 10
        self.traffic_density = random.randint(1, 10)
        self.display_frame = display_frame
        self.running = False
        self.thread = None

        # Create title and labels within the frame
        self.title_label = tk.Label(self.display_frame, text=self.name, font=("Arial", 16, "bold"), bg="#4A90E2", fg="white")
        self.title_label.pack(pady=10)

        self.info_label = tk.Label(self.display_frame, text="", font=("Arial", 12), bg="#E7E7E7")
        self.info_label.pack(pady=5)

        self.color_label = tk.Label(self.display_frame, text="", width=20, height=3, bg="gray", relief="raised")
        self.color_label.pack(pady=10)

        self.current_time_label = tk.Label(self.display_frame, text="", font=("Arial", 12), bg="#E7E7E7")
        self.current_time_label.pack(pady=5)

        self.next_time_label = tk.Label(self.display_frame, text="", font=("Arial", 12), bg="#E7E7E7")
        self.next_time_label.pack(pady=5)

    def update_traffic_density(self):
        self.traffic_density = random.randint(1, 10)
        self.green_light_duration = max(5, min(20, 10 + (self.traffic_density - 5)))

    def operate(self):
        if not self.running:
            self.update_traffic_density()
            self.info_label.config(text=f"Traffic Density: {self.traffic_density}\nGreen Light Duration: {self.green_light_duration}s")
            self.running = True
            self.start_light_cycle()

    def start_light_cycle(self):
        if self.thread is None or not self.thread.is_alive():
            self.thread = threading.Thread(target=self.light_cycle, daemon=True)
            self.thread.start()

    def light_cycle(self):
        while self.running:
            self.update_color("green", "Green", self.green_light_duration, 3)
            self.update_color("orange", "Orange", 3, 5)
            self.update_color("red", "Red", 5, 0)
            self.update_traffic_density()
            self.info_label.config(text=f"Traffic Density: {self.traffic_density}\nGreen Light Duration: {self.green_light_duration}s")

    def update_color(self, color, text, current_duration, next_duration):
        for remaining in range(current_duration, 0, -1):
            self.color_label.config(bg=color, text=text)
            self.current_time_label.config(text=f"Current Color Countdown: {remaining}s")
            self.next_time_label.config(text=f"Next Color Countdown: {next_duration}s")
            self.color_label.update_idletasks()
            time.sleep(1)

        if next_duration > 0:
            self.next_time_label.config(text=f"Next Color Countdown: {next_duration}s")

    def stop(self):
        self.running = False

class TrafficSimulatorApp:
    def _init_(self, root):
        self.root = root
        self.root.title("Traffic Signal Simulator")
        self.root.configure(bg="#F5F5F5")

        # Create a grid for 8 traffic signals
        self.signals = []
        for i in range(2):
            for j in range(4):
                frame = ttk.Frame(root, padding="10", borderwidth=2, relief="raised")
                frame.grid(row=i, column=j, padx=5, pady=5, sticky="nsew")

                # Create a traffic signal instance
                traffic_signal = TrafficSignal(f"Traffic Signal {i * 4 + j + 1}", frame)
                self.signals.append(traffic_signal)

        # Control button
        self.start_button = ttk.Button(root, text="Start Simulation", command=self.start_simulation, style='TButton')
        self.start_button.grid(row=2, column=0, pady=10, columnspan=4)

        # Configure grid weights to make it responsive
        for i in range(2):
            self.root.grid_rowconfigure(i, weight=1)
        self.root.grid_rowconfigure(2, weight=0)  # Last row for buttons
        for j in range(4):
            self.root.grid_columnconfigure(j, weight=1)

    def start_simulation(self):
        for signal in self.signals:
            signal.operate()

# Initialize the Tkinter window
root = tk.Tk()
app = TrafficSimulatorApp(root)

# Start the Tkinter event loop
root.mainloop()
